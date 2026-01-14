# AGENTS.md - Multi-Agent Orchestration Guide

> **FlashFusion AI Agent Architecture**
> Comprehensive guide for building, deploying, and orchestrating AI agents within FlashFusion.

---

## Overview

FlashFusion uses a multi-agent architecture where specialized AI agents handle different aspects of application generation. This document describes the agent system, their capabilities, and how to extend them.

---

## Agent Types

### 1. Code Generation Agent
**Purpose**: Generate production-ready code from natural language descriptions.

```typescript
// Agent Configuration
interface CodeGenerationAgent {
  model: 'gpt-5';
  capabilities: [
    'full-stack-generation',
    'component-generation',
    'api-generation',
    'database-schema',
    'test-generation'
  ];
  outputFormat: 'json';
  maxTokens: 8192;
}
```

**Prompt Template**:
```typescript
const systemPrompt = `You are an expert full-stack developer. Generate production-ready code based on the user's requirements.

Type: ${type}
Requirements: ${prompt}

Generate a complete project with:
1. Clean, well-structured code following best practices
2. All necessary files (HTML, CSS, JavaScript/TypeScript, etc.)
3. Comments explaining key functionality
4. Ready to run without modifications

Respond with JSON in this exact format:
{
  "files": [
    { "path": "filename", "content": "..." }
  ],
  "description": "Brief description",
  "instructions": "How to run/use"
}`;
```

**API Endpoint**: `POST /api/generate-code`

**File Location**: `server/routes.ts:288-397`

---

### 2. Image Generation Agent
**Purpose**: Create AI-generated images using DALL-E 3.

```typescript
// Agent Configuration
interface ImageGenerationAgent {
  model: 'dall-e-3';
  capabilities: [
    'text-to-image',
    'style-transfer',
    'aspect-ratio-control',
    'quality-settings'
  ];
  styles: [
    'photorealistic',
    'digital-art',
    'sketch',
    'cinematic',
    'anime',
    'fantasy'
  ];
  sizes: ['1024x1024', '1024x1792', '1792x1024'];
  quality: ['standard', 'hd'];
}
```

**API Endpoint**: `POST /api/generate-image`

**File Location**: `server/routes.ts:458-565`

---

### 3. Workflow Orchestration Agent
**Purpose**: Manage multi-step workflows and coordinate between agents.

```typescript
// Agent Configuration
interface WorkflowOrchestrationAgent {
  workflows: [
    'ai-creation',      // Full-stack app generation
    'publishing',       // Deployment automation
    'commerce',         // E-commerce integration
    'analytics',        // Tracking setup
    'security',         // Vulnerability scanning
    'quality'           // Code review & testing
  ];
  maxSteps: 10;
  stateManagement: 'database-backed';
}
```

**Workflow States**:
```typescript
type WorkflowStatus = 'in_progress' | 'completed' | 'failed';
type StepStatus = 'pending' | 'active' | 'completed' | 'skipped';
```

---

## Agent Communication Protocol

### Request Format
```typescript
interface AgentRequest {
  agentType: 'code' | 'image' | 'workflow' | 'security' | 'quality';
  userId: string;
  prompt: string;
  configuration: Record<string, any>;
  context?: {
    workflowId?: string;
    previousSteps?: any[];
    projectFiles?: Array<{ path: string; content: string }>;
  };
}
```

### Response Format (SSE Streaming)
```typescript
// Progress Event
interface ProgressEvent {
  type: 'progress';
  message: string;
  progress: number; // 0-100
}

// Completion Event
interface CompleteEvent {
  type: 'complete';
  result: any;
  progress: 100;
}

// Error Event
interface ErrorEvent {
  type: 'error';
  error: string;
}
```

---

## Agent Implementation Pattern

### Basic Agent Structure
```typescript
// server/agents/base-agent.ts
export abstract class BaseAgent {
  protected model: string;
  protected maxTokens: number;

  constructor(config: AgentConfig) {
    this.model = config.model;
    this.maxTokens = config.maxTokens;
  }

  abstract async execute(request: AgentRequest): Promise<AgentResponse>;

  protected async callOpenAI(prompt: string): Promise<string> {
    const completion = await openai.chat.completions.create({
      model: this.model,
      messages: [{ role: 'user', content: prompt }],
      max_completion_tokens: this.maxTokens,
    });
    return completion.choices[0]?.message?.content || '';
  }

  protected streamProgress(res: Response, message: string, progress: number) {
    res.write(`data: ${JSON.stringify({ type: 'progress', message, progress })}\n\n`);
  }
}
```

### Specialized Agent Example
```typescript
// server/agents/code-generation-agent.ts
import { BaseAgent } from './base-agent';

export class CodeGenerationAgent extends BaseAgent {
  constructor() {
    super({
      model: 'gpt-5',
      maxTokens: 8192,
    });
  }

  async execute(request: AgentRequest): Promise<AgentResponse> {
    const systemPrompt = this.buildSystemPrompt(request);

    const response = await this.callOpenAI(systemPrompt);
    const result = JSON.parse(response);

    // Persist to database
    const project = await storage.createGeneratedProject({
      userId: request.userId,
      title: result.description,
      files: JSON.stringify(result.files),
      projectType: request.configuration.type,
    });

    return {
      success: true,
      projectId: project.id,
      files: result.files,
    };
  }

  private buildSystemPrompt(request: AgentRequest): string {
    // Build context-aware prompt
  }
}
```

---

## Workflow Execution Pipeline

### Step-by-Step Flow
```
1. User initiates workflow
   └── POST /api/workflows
       └── Create workflow_run record (status: in_progress)

2. Frontend polls for updates
   └── GET /api/workflows/:id
       └── Return current step and status

3. Agent executes current step
   └── Call appropriate agent based on workflowType
       └── Update workflow_steps record

4. Step completion
   └── PATCH /api/workflows/:id
       └── Increment currentStep
       └── Create workflow_results if final step

5. Workflow completion
   └── Update status to 'completed'
   └── Set completedAt timestamp
```

### Workflow State Machine
```
                    ┌─────────────────┐
                    │   in_progress   │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
        ┌─────────┐    ┌─────────┐    ┌─────────┐
        │  step1  │───▶│  step2  │───▶│  stepN  │
        └─────────┘    └─────────┘    └────┬────┘
                                           │
                        ┌──────────────────┼──────────────────┐
                        │                                     │
                        ▼                                     ▼
                  ┌───────────┐                         ┌──────────┐
                  │ completed │                         │  failed  │
                  └───────────┘                         └──────────┘
```

---

## Agent Configuration

### Environment Variables
```bash
# OpenAI Configuration
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-5
OPENAI_MAX_TOKENS=8192

# Image Generation
DALLE_MODEL=dall-e-3
DALLE_DEFAULT_SIZE=1024x1024
DALLE_DEFAULT_QUALITY=standard

# Agent Rate Limits
AGENT_RATE_LIMIT_WINDOW=3600000  # 1 hour
AGENT_MAX_REQUESTS=100           # per window
```

### Feature Flags for Agents
```typescript
// client/src/utils/featureFlags.ts
export const featureFlags = {
  AGENT_TEASERS_ENABLED: true,    // Show agent preview cards
  CODE_GENERATION_V2: false,       // New code gen algorithm
  STREAMING_ENABLED: true,         // SSE streaming responses
  MULTI_AGENT_COLLAB: false,       // Agent-to-agent communication
};
```

---

## Adding a New Agent

### 1. Define Agent Schema
```typescript
// shared/schema.ts
export const agentConfigs = pgTable("agent_configs", {
  id: varchar("id").primaryKey(),
  name: text("name").notNull(),
  type: text("type").notNull(),
  model: text("model").notNull(),
  systemPrompt: text("system_prompt"),
  configuration: text("configuration"), // JSON
  isActive: boolean("is_active").default(true),
  createdAt: timestamp("created_at").defaultNow(),
});
```

### 2. Implement Agent Class
```typescript
// server/agents/my-new-agent.ts
import { BaseAgent } from './base-agent';

export class MyNewAgent extends BaseAgent {
  constructor() {
    super({ model: 'gpt-5', maxTokens: 4096 });
  }

  async execute(request: AgentRequest): Promise<AgentResponse> {
    // Implementation
  }
}
```

### 3. Register Agent Route
```typescript
// server/routes.ts
import { MyNewAgent } from './agents/my-new-agent';

app.post('/api/agents/my-new-agent', async (req, res) => {
  const agent = new MyNewAgent();
  const result = await agent.execute(req.body);
  res.json(result);
});
```

### 4. Create Frontend Integration
```typescript
// client/src/hooks/useMyNewAgent.ts
export function useMyNewAgent() {
  return useMutation({
    mutationFn: async (request: AgentRequest) => {
      const response = await fetch('/api/agents/my-new-agent', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(request),
      });
      return response.json();
    },
  });
}
```

---

## Agent Testing

### Unit Testing Agents
```typescript
// tests/agents/code-generation.test.ts
describe('CodeGenerationAgent', () => {
  it('should generate valid project structure', async () => {
    const agent = new CodeGenerationAgent();
    const result = await agent.execute({
      userId: 'test-user',
      prompt: 'Create a todo app',
      configuration: { type: 'webapp' },
    });

    expect(result.files).toBeDefined();
    expect(result.files.length).toBeGreaterThan(0);
  });
});
```

### E2E Testing Workflows
```typescript
// tests/e2e/workflows.spec.ts
test('complete ai-creation workflow', async ({ page }) => {
  await page.goto('/workflows/ai-creation');
  await page.fill('[data-testid="prompt"]', 'Create a blog');
  await page.click('[data-testid="generate"]');

  await expect(page.locator('[data-testid="progress"]'))
    .toHaveText('100%', { timeout: 60000 });
});
```

---

## Agent Monitoring

### Metrics to Track
```typescript
interface AgentMetrics {
  requestCount: number;
  successRate: number;
  averageLatency: number;
  tokenUsage: number;
  errorRate: number;
  costPerRequest: number;
}
```

### Logging Pattern
```typescript
// server/utils/agent-logger.ts
export function logAgentRequest(
  agentType: string,
  userId: string,
  startTime: number,
  status: 'success' | 'error',
  details?: any
) {
  const duration = Date.now() - startTime;
  console.log(JSON.stringify({
    timestamp: new Date().toISOString(),
    type: 'agent_request',
    agent: agentType,
    userId,
    duration,
    status,
    ...details,
  }));
}
```

---

## Best Practices

### 1. Prompt Engineering
- Be specific about output format
- Include examples in system prompts
- Use JSON response format for structured data
- Limit context to relevant information

### 2. Error Handling
```typescript
try {
  const result = await agent.execute(request);
  return { success: true, data: result };
} catch (error) {
  // Log error with context
  logAgentError(agentType, userId, error);

  // Return user-friendly message
  return {
    success: false,
    error: 'Generation failed. Please try again.',
    retryable: true,
  };
}
```

### 3. Rate Limiting
```typescript
// Check rate limit before agent execution
const canProceed = await checkRateLimit(userId, agentType);
if (!canProceed) {
  return res.status(429).json({
    error: 'Rate limit exceeded',
    retryAfter: 3600,
  });
}
```

### 4. Cost Management
```typescript
// Track token usage for billing
const tokenCount = completion.usage?.total_tokens || 0;
await storage.incrementTokenUsage(userId, tokenCount);

// Check if user has sufficient credits
if (user.tokenBalance < estimatedTokens) {
  return res.status(402).json({
    error: 'Insufficient credits',
    required: estimatedTokens,
    balance: user.tokenBalance,
  });
}
```

---

## Future Roadmap

### Planned Agent Types
1. **Security Scanning Agent** - Vulnerability detection
2. **Code Review Agent** - Quality analysis
3. **Documentation Agent** - Auto-generate docs
4. **Testing Agent** - Test suite generation
5. **Deployment Agent** - CI/CD automation

### Multi-Agent Collaboration
```typescript
// Future: Agent-to-agent communication
interface AgentCollaboration {
  primaryAgent: string;
  supportingAgents: string[];
  communicationProtocol: 'sync' | 'async';
  resultAggregation: 'merge' | 'chain' | 'parallel';
}
```

---

## Resources

- **OpenAI API Docs**: https://platform.openai.com/docs
- **Prompt Engineering Guide**: https://platform.openai.com/docs/guides/prompt-engineering
- **FlashFusion Routes**: `server/routes.ts`
- **Storage Interface**: `server/storage.ts`

---

**Last Updated**: December 30, 2025
**Version**: 1.0.0
