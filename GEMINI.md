# GEMINI.md - Google AI Integration Guide

> **FlashFusion Google Gemini & Vertex AI Integration**
> Guide for integrating Google's AI capabilities alongside OpenAI in FlashFusion.

---

## Overview

This document provides guidance for integrating Google's Gemini models as an alternative or complementary AI backend to OpenAI in FlashFusion. Gemini offers competitive pricing, long context windows, and multimodal capabilities.

---

## Why Gemini?

### Comparison with OpenAI

| Feature | OpenAI GPT-5 | Gemini 2.0 Flash |
|---------|-------------|-------------------|
| Context Window | 128K tokens | 1M tokens |
| Pricing | Higher | 50-75% lower |
| Speed | Fast | Very Fast |
| Multimodal | GPT-4V, DALL-E | Native multimodal |
| Code Generation | Excellent | Excellent |
| Availability | Global | Global |

### When to Use Gemini
- **Long Context Tasks**: Documents, codebases, lengthy prompts
- **Cost Optimization**: High-volume generation tasks
- **Multimodal Workflows**: Image understanding + generation
- **Fallback/Redundancy**: When OpenAI is unavailable

---

## Integration Architecture

### Proposed Structure
```
FlashFusion/
├── server/
│   ├── openai.ts          # Existing OpenAI client
│   ├── gemini.ts          # New: Gemini client
│   ├── ai-router.ts       # New: Route to appropriate AI
│   └── ai-providers/
│       ├── openai.ts      # OpenAI implementation
│       ├── gemini.ts      # Gemini implementation
│       └── types.ts       # Shared interfaces
```

### AI Provider Interface
```typescript
// server/ai-providers/types.ts
export interface AIProvider {
  name: string;
  generateText(prompt: string, options: GenerateOptions): Promise<TextResult>;
  generateImage(prompt: string, options: ImageOptions): Promise<ImageResult>;
  generateCode(prompt: string, options: CodeOptions): Promise<CodeResult>;
  estimateCost(tokens: number): number;
}

export interface GenerateOptions {
  model?: string;
  maxTokens?: number;
  temperature?: number;
  responseFormat?: 'text' | 'json';
}

export interface TextResult {
  content: string;
  tokensUsed: number;
  model: string;
  cost: number;
}
```

---

## Gemini Client Implementation

### Setup
```bash
# Install Google AI SDK
npm install @google/generative-ai
```

### Environment Variables
```bash
# Add to .env
GOOGLE_AI_API_KEY=your-api-key
GEMINI_MODEL=gemini-2.0-flash-exp
GEMINI_VISION_MODEL=gemini-pro-vision
VERTEX_PROJECT_ID=your-project-id
VERTEX_LOCATION=us-central1
```

### Client Implementation
```typescript
// server/ai-providers/gemini.ts
import { GoogleGenerativeAI } from '@google/generative-ai';
import type { AIProvider, GenerateOptions, TextResult } from './types';

export class GeminiProvider implements AIProvider {
  name = 'gemini';
  private client: GoogleGenerativeAI;
  private model: GenerativeModel;

  constructor() {
    const apiKey = process.env.GOOGLE_AI_API_KEY;
    if (!apiKey) {
      throw new Error('GOOGLE_AI_API_KEY is required');
    }
    this.client = new GoogleGenerativeAI(apiKey);
    this.model = this.client.getGenerativeModel({
      model: process.env.GEMINI_MODEL || 'gemini-2.0-flash-exp',
    });
  }

  async generateText(prompt: string, options: GenerateOptions): Promise<TextResult> {
    const generationConfig = {
      maxOutputTokens: options.maxTokens || 8192,
      temperature: options.temperature || 0.7,
    };

    const result = await this.model.generateContent({
      contents: [{ role: 'user', parts: [{ text: prompt }] }],
      generationConfig,
    });

    const response = result.response;
    const text = response.text();
    const tokensUsed = response.usageMetadata?.totalTokenCount || 0;

    return {
      content: text,
      tokensUsed,
      model: process.env.GEMINI_MODEL || 'gemini-2.0-flash-exp',
      cost: this.estimateCost(tokensUsed),
    };
  }

  async generateCode(prompt: string, options: GenerateOptions): Promise<TextResult> {
    const systemInstruction = `You are an expert full-stack developer.
Generate production-ready code with proper structure and best practices.
Always respond with valid JSON containing files array.`;

    const result = await this.model.generateContent({
      contents: [
        { role: 'user', parts: [{ text: systemInstruction + '\n\n' + prompt }] }
      ],
      generationConfig: {
        maxOutputTokens: options.maxTokens || 8192,
        temperature: 0.3, // Lower for code
        responseMimeType: 'application/json',
      },
    });

    return {
      content: result.response.text(),
      tokensUsed: result.response.usageMetadata?.totalTokenCount || 0,
      model: process.env.GEMINI_MODEL || 'gemini-2.0-flash-exp',
      cost: this.estimateCost(result.response.usageMetadata?.totalTokenCount || 0),
    };
  }

  estimateCost(tokens: number): number {
    // Gemini 2.0 Flash pricing (approximate)
    // Input: $0.10 per 1M tokens, Output: $0.40 per 1M tokens
    // Using average estimate
    return (tokens / 1_000_000) * 0.25;
  }
}
```

---

## AI Router Implementation

### Smart Routing Logic
```typescript
// server/ai-router.ts
import { OpenAIProvider } from './ai-providers/openai';
import { GeminiProvider } from './ai-providers/gemini';
import type { AIProvider } from './ai-providers/types';

interface RouteConfig {
  preferredProvider: 'openai' | 'gemini' | 'auto';
  fallbackEnabled: boolean;
  costOptimization: boolean;
  maxRetries: number;
}

export class AIRouter {
  private openai: OpenAIProvider;
  private gemini: GeminiProvider;
  private config: RouteConfig;

  constructor(config: Partial<RouteConfig> = {}) {
    this.config = {
      preferredProvider: 'auto',
      fallbackEnabled: true,
      costOptimization: true,
      maxRetries: 2,
      ...config,
    };

    this.openai = new OpenAIProvider();
    this.gemini = new GeminiProvider();
  }

  selectProvider(task: TaskType): AIProvider {
    if (this.config.preferredProvider !== 'auto') {
      return this.config.preferredProvider === 'openai'
        ? this.openai
        : this.gemini;
    }

    // Smart selection based on task
    switch (task) {
      case 'code-generation':
        // OpenAI slightly better for complex code
        return this.openai;

      case 'long-document':
        // Gemini has 1M context
        return this.gemini;

      case 'image-understanding':
        // Gemini Pro Vision
        return this.gemini;

      case 'image-generation':
        // OpenAI DALL-E
        return this.openai;

      case 'cost-sensitive':
        // Gemini is cheaper
        return this.gemini;

      default:
        return this.openai;
    }
  }

  async execute(task: TaskType, request: any): Promise<any> {
    const primaryProvider = this.selectProvider(task);

    try {
      return await this.executeWithProvider(primaryProvider, task, request);
    } catch (error) {
      if (this.config.fallbackEnabled) {
        const fallbackProvider = primaryProvider === this.openai
          ? this.gemini
          : this.openai;

        console.log(`Falling back to ${fallbackProvider.name}`);
        return await this.executeWithProvider(fallbackProvider, task, request);
      }
      throw error;
    }
  }

  private async executeWithProvider(
    provider: AIProvider,
    task: TaskType,
    request: any
  ): Promise<any> {
    switch (task) {
      case 'code-generation':
        return provider.generateCode(request.prompt, request.options);
      case 'text-generation':
        return provider.generateText(request.prompt, request.options);
      default:
        throw new Error(`Unsupported task: ${task}`);
    }
  }
}

type TaskType =
  | 'code-generation'
  | 'text-generation'
  | 'long-document'
  | 'image-understanding'
  | 'image-generation'
  | 'cost-sensitive';
```

---

## Integration with Existing Routes

### Updated Code Generation Endpoint
```typescript
// server/routes.ts - Updated
import { AIRouter } from './ai-router';

const aiRouter = new AIRouter({
  preferredProvider: process.env.AI_PROVIDER as any || 'auto',
  fallbackEnabled: true,
});

app.post('/api/generate-code', async (req, res) => {
  try {
    const { prompt, type, userId, provider } = req.body;

    // Set headers for streaming
    res.setHeader('Content-Type', 'text/event-stream');
    res.setHeader('Cache-Control', 'no-cache');
    res.setHeader('Connection', 'keep-alive');

    res.write(`data: ${JSON.stringify({
      type: 'progress',
      message: 'Initializing AI generation...',
      progress: 10
    })}\n\n`);

    // Use AI router for provider selection
    const result = await aiRouter.execute('code-generation', {
      prompt,
      options: {
        maxTokens: 8192,
        responseFormat: 'json',
      },
    });

    res.write(`data: ${JSON.stringify({
      type: 'progress',
      message: 'Processing generated code...',
      progress: 80
    })}\n\n`);

    const parsed = JSON.parse(result.content);

    // Persist to database
    const project = await storage.createGeneratedProject({
      userId,
      title: parsed.description || 'AI Generated Project',
      description: prompt,
      projectType: type,
      files: JSON.stringify(parsed.files || []),
      metadata: JSON.stringify({
        model: result.model,
        provider: result.provider,
        tokensUsed: result.tokensUsed,
        cost: result.cost,
      }),
    });

    res.write(`data: ${JSON.stringify({
      type: 'complete',
      result: { ...parsed, projectId: project.id },
      progress: 100
    })}\n\n`);
    res.end();

  } catch (error) {
    // Error handling
  }
});
```

---

## Vertex AI (Enterprise)

### When to Use Vertex AI
- Enterprise compliance requirements
- VPC-SC security controls
- Custom model fine-tuning
- Higher rate limits
- SLA guarantees

### Vertex AI Setup
```typescript
// server/ai-providers/vertex.ts
import { VertexAI } from '@google-cloud/vertexai';

export class VertexProvider implements AIProvider {
  name = 'vertex';
  private client: VertexAI;

  constructor() {
    this.client = new VertexAI({
      project: process.env.VERTEX_PROJECT_ID,
      location: process.env.VERTEX_LOCATION || 'us-central1',
    });
  }

  async generateText(prompt: string, options: GenerateOptions): Promise<TextResult> {
    const model = this.client.getGenerativeModel({
      model: 'gemini-2.0-flash',
    });

    const result = await model.generateContent({
      contents: [{ role: 'user', parts: [{ text: prompt }] }],
    });

    return {
      content: result.response.text(),
      tokensUsed: result.response.usageMetadata?.totalTokenCount || 0,
      model: 'gemini-2.0-flash',
      cost: 0, // Vertex pricing is different
    };
  }
}
```

---

## Multimodal Capabilities

### Image Understanding
```typescript
// server/ai-providers/gemini.ts - Extended
async analyzeImage(imageUrl: string, prompt: string): Promise<TextResult> {
  const model = this.client.getGenerativeModel({
    model: 'gemini-pro-vision',
  });

  // Fetch image and convert to base64
  const imageData = await fetchImageAsBase64(imageUrl);

  const result = await model.generateContent([
    {
      inlineData: {
        mimeType: 'image/jpeg',
        data: imageData,
      },
    },
    { text: prompt },
  ]);

  return {
    content: result.response.text(),
    tokensUsed: result.response.usageMetadata?.totalTokenCount || 0,
    model: 'gemini-pro-vision',
    cost: this.estimateCost(result.response.usageMetadata?.totalTokenCount || 0),
  };
}
```

### Combined Code + Image Analysis
```typescript
// Analyze UI screenshot and generate code
async generateFromScreenshot(imageUrl: string): Promise<CodeResult> {
  const analysis = await this.analyzeImage(
    imageUrl,
    'Describe this UI in detail, including layout, colors, components, and interactions.'
  );

  const code = await this.generateCode(
    `Generate React components that recreate this UI:\n\n${analysis.content}`,
    { maxTokens: 8192 }
  );

  return {
    analysis: analysis.content,
    code: code.content,
  };
}
```

---

## Cost Optimization

### Pricing Comparison (Approximate)

| Model | Input (per 1M) | Output (per 1M) |
|-------|----------------|-----------------|
| GPT-5 | $2.50 | $10.00 |
| GPT-4o | $2.50 | $10.00 |
| Gemini 2.0 Flash | $0.10 | $0.40 |
| Gemini 1.5 Pro | $1.25 | $5.00 |

### Cost Optimization Strategy
```typescript
// server/cost-optimizer.ts
export function selectCostEffectiveProvider(
  task: TaskType,
  estimatedTokens: number,
  qualityRequired: 'high' | 'medium' | 'low'
): AIProvider {
  const openaiCost = estimatedTokens / 1_000_000 * 6.25; // Average
  const geminiCost = estimatedTokens / 1_000_000 * 0.25; // Flash

  if (qualityRequired === 'high' || task === 'code-generation') {
    // Use OpenAI for critical tasks
    return new OpenAIProvider();
  }

  if (geminiCost < openaiCost * 0.2) {
    // Gemini is >80% cheaper, use it
    return new GeminiProvider();
  }

  return new OpenAIProvider();
}
```

---

## Feature Flags

```typescript
// client/src/utils/featureFlags.ts - Updated
export const featureFlags = {
  // Existing flags...

  // AI Provider flags
  GEMINI_ENABLED: false,
  VERTEX_AI_ENABLED: false,
  AI_PROVIDER_SELECTION: false,  // Let users choose provider
  COST_DISPLAY_ENABLED: false,   // Show cost estimates to users
  MULTI_PROVIDER_FALLBACK: true, // Auto-fallback on errors
};
```

---

## Testing

### Provider Tests
```typescript
// tests/ai-providers/gemini.test.ts
describe('GeminiProvider', () => {
  it('should generate text successfully', async () => {
    const provider = new GeminiProvider();
    const result = await provider.generateText('Hello, world!', {});

    expect(result.content).toBeDefined();
    expect(result.tokensUsed).toBeGreaterThan(0);
  });

  it('should generate code in JSON format', async () => {
    const provider = new GeminiProvider();
    const result = await provider.generateCode('Create a hello world function', {});

    const parsed = JSON.parse(result.content);
    expect(parsed.files).toBeDefined();
  });
});
```

### E2E Tests
```typescript
// tests/e2e/ai-generation.spec.ts
test('should generate code with Gemini provider', async ({ page }) => {
  await page.goto('/workflows/ai-creation');

  // Select Gemini provider if feature flag enabled
  if (await page.locator('[data-testid="provider-select"]').isVisible()) {
    await page.selectOption('[data-testid="provider-select"]', 'gemini');
  }

  await page.fill('[data-testid="prompt"]', 'Create a simple counter component');
  await page.click('[data-testid="generate"]');

  await expect(page.locator('[data-testid="result"]'))
    .toBeVisible({ timeout: 30000 });
});
```

---

## Migration Path

### Phase 1: Preparation
1. Add Gemini SDK dependency
2. Create `gemini.ts` provider
3. Implement `ai-router.ts`
4. Add feature flags

### Phase 2: Testing
1. Enable for internal testing
2. A/B test quality comparison
3. Monitor cost savings
4. Gather user feedback

### Phase 3: Rollout
1. Enable as fallback provider
2. Add provider selection UI
3. Roll out to all users
4. Update documentation

---

## Resources

- **Google AI Studio**: https://aistudio.google.com/
- **Gemini API Docs**: https://ai.google.dev/docs
- **Vertex AI Docs**: https://cloud.google.com/vertex-ai/docs
- **Pricing**: https://ai.google.dev/pricing

---

**Last Updated**: December 30, 2025
**Version**: 1.0.0
**Status**: Planning Phase
