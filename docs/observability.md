---
layout: default
title: "Observability"
nav_order: 6
description: "Monitor and optimize your agents with Langfuse"
---

# Observability with Langfuse

Monitor your AI agents using Langfuse for tracking, debugging, and optimization.

## What You Get

- **Track conversations** - See all agent interactions
- **Measure performance** - Response times and latency
- **Debug issues** - Identify and fix problems
- **Monitor costs** - Track token usage and API costs
- **Evaluate quality** - Assess agent responses

## Setup

### 1. Get Langfuse Credentials

1. Sign up at [cloud.langfuse.com](https://cloud.langfuse.com)
2. Create a new project
3. Copy your **API Key** and **Public Key**

### 2. Configure in Orchestrate

```bash
uv run orchestrate settings observability langfuse configure \
  --url "https://cloud.langfuse.com/api/public/otel" \
  --api-key "YOUR_LANGFUSE_API_KEY" \
  --health-uri "https://cloud.langfuse.com" \
  --config-json '{"public_key": "YOUR_LANGFUSE_PUBLIC_KEY"}'
```

### 3. Verify

1. Check logs: `uv run orchestrate server logs`
2. Interact with an agent in Chat UI
3. View traces in Langfuse dashboard

## Key Metrics to Monitor

| Metric | What to Track |
|--------|---------------|
| **Response Time** | How long agents take to respond |
| **Token Usage** | Input/output tokens per interaction |
| **Error Rate** | Failed interactions and tool calls |
| **Cost** | $ per interaction based on token usage |

## Viewing Traces

Each agent interaction creates a trace showing:
- User input
- Agent reasoning steps
- Tool calls and results
- Model responses
- Final output

Navigate to **Traces** in Langfuse to see detailed breakdowns.

## Debugging

### Common Issues

**Slow responses:**
- Check LLM response times in traces
- Review tool execution duration
- Look for network delays

**Incorrect outputs:**
- Review agent's reasoning in trace
- Check tool call parameters
- Verify knowledge base retrievals

**Tool failures:**
- View error messages in trace
- Check tool parameters
- Verify dependencies are installed

## Cost Optimization

### Reduce Token Usage

- Use smaller models for simple tasks
- Shorten system prompts
- Optimize tool descriptions
- Cache frequent queries

### Model Selection

- **Fast tasks** → Smaller models (Granite 3-8B)
- **Complex reasoning** → Larger models (Llama 4)
- **Specialized** → Third-party models with specific capabilities

## Commands

```bash
# View configuration
uv run orchestrate settings observability langfuse get

# Remove configuration
uv run orchestrate settings observability langfuse remove
```

## Self-Hosted Option

For enterprise deployments, you can self-host Langfuse:

```bash
uv run orchestrate settings observability langfuse configure \
  --url "https://your-langfuse-instance.com/api/public/otel" \
  --api-key "YOUR_API_KEY" \
  --health-uri "https://your-langfuse-instance.com" \
  --config-json '{"public_key": "YOUR_PUBLIC_KEY"}'
```

Benefits:
- Data stays in your infrastructure
- Lower latency
- Full control over settings
- No per-trace pricing

## Resources

- [Langfuse Documentation](https://langfuse.com/docs)
- [OpenTelemetry Integration](https://langfuse.com/docs/integrations/opentelemetry)
- [Langfuse GitHub](https://github.com/langfuse/langfuse)