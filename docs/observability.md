---
layout: default
title: "Observability"
nav_order: 6
description: "Monitor and optimize your agents with Langfuse"
parent: "Watsonx Orchestrate ADK"
---

# Observability with Langfuse

Monitor, debug, and optimize your AI agents using Langfuse observability platform.

## Overview

Observability is crucial for production AI agents. With Langfuse integration, you can:

- **Track conversations** - Monitor all agent interactions
- **Measure performance** - Analyze response times and latency
- **Debug issues** - Identify and fix problems quickly
- **Optimize costs** - Track token usage and API costs
- **Improve quality** - Evaluate agent responses and accuracy

## What is Langfuse?

Langfuse is an open-source observability platform designed specifically for LLM applications. It provides:

- Real-time monitoring dashboards
- Detailed trace analysis
- Cost tracking and optimization
- Quality evaluation tools
- Team collaboration features

## Getting Started

### Prerequisites

Before configuring Langfuse:

1. **Sign up for Langfuse**:
   - Visit [cloud.langfuse.com](https://cloud.langfuse.com)
   - Create a free account
   - Create a new project

2. **Get your credentials**:
   - Navigate to project settings
   - Copy your **API Key**
   - Copy your **Public Key**

{: .important }
> Keep your API key secure! It provides full access to your Langfuse project.

### Configuration

Configure Langfuse in Orchestrate with a single command:

```bash
uv run orchestrate settings observability langfuse configure \
  --url "https://cloud.langfuse.com/api/public/otel" \
  --api-key "YOUR_LANGFUSE_API_KEY" \
  --health-uri "https://cloud.langfuse.com" \
  --config-json '{"public_key": "YOUR_LANGFUSE_PUBLIC_KEY"}'
```

**Parameters:**
- `--url`: Langfuse OTEL endpoint (use the URL above for cloud)
- `--api-key`: Your Langfuse API key
- `--health-uri`: Health check endpoint
- `--config-json`: Additional configuration including public key

### Verify Configuration

After configuration, verify the connection:

1. **Check Orchestrate logs**:
   ```bash
   uv run orchestrate server logs
   ```

2. **Test with an agent interaction**:
   - Use the Chat UI to interact with an agent
   - Check Langfuse dashboard for new traces

3. **View in Langfuse**:
   - Log into Langfuse
   - Navigate to your project
   - You should see traces appearing

## Understanding Traces

### What is a Trace?

A trace represents a complete agent interaction, including:

- User input
- Agent reasoning steps
- Tool calls
- Model responses
- Final output

### Trace Components

#### Spans

Individual operations within a trace:
- **LLM calls** - Model inference requests
- **Tool executions** - Python tool invocations
- **Retrievals** - Knowledge base queries
- **Processing** - Data transformations

#### Metadata

Additional context for each trace:
- Timestamp
- Duration
- Token counts
- Cost estimates
- User information
- Session IDs

## Monitoring Dashboard

### Key Metrics

#### Response Time

Track how long agents take to respond:
- Average response time
- P50, P95, P99 percentiles
- Slowest interactions
- Response time trends

#### Token Usage

Monitor token consumption:
- Input tokens
- Output tokens
- Total tokens per interaction
- Tokens by model
- Cost per interaction

#### Error Rates

Track failures and issues:
- Error count
- Error types
- Failed tool calls
- Model errors
- Success rate percentage

#### Usage Patterns

Understand how agents are used:
- Interactions per hour/day
- Most active users
- Popular queries
- Tool usage frequency

## Debugging with Langfuse

### Viewing Detailed Traces

1. **Navigate to Traces** in Langfuse dashboard
2. **Click on a trace** to see details
3. **Expand spans** to see individual operations
4. **Review inputs/outputs** at each step

### Common Issues to Debug

#### Slow Responses

Identify bottlenecks:
- Check LLM response times
- Review tool execution duration
- Look for network delays
- Identify inefficient operations

#### Incorrect Outputs

Trace reasoning errors:
- Review agent's thought process
- Check tool call parameters
- Verify knowledge base retrievals
- Examine model responses

#### Tool Failures

Debug tool issues:
- View tool call parameters
- Check error messages
- Review tool execution logs
- Verify dependencies

## Cost Optimization

### Tracking Costs

Langfuse automatically calculates costs based on:
- Model pricing
- Token usage
- API calls
- Tool executions

### Cost Reduction Strategies

#### Model Selection

Choose cost-effective models:
- Use smaller models for simple tasks
- Reserve large models for complex reasoning
- Compare model performance vs. cost

#### Prompt Optimization

Reduce token usage:
- Shorten system prompts
- Use concise descriptions
- Avoid redundant context
- Optimize tool descriptions

#### Caching

Implement caching strategies:
- Cache frequent queries
- Store common responses
- Reuse tool results
- Minimize redundant API calls

## Quality Evaluation

### Manual Evaluation

Review agent interactions:
1. **Sample conversations** regularly
2. **Rate responses** for quality
3. **Identify patterns** in good/bad responses
4. **Document findings** for improvement

### Automated Evaluation

Set up automated quality checks:

```python
# Example evaluation criteria
evaluation_criteria = {
    "accuracy": "Is the response factually correct?",
    "relevance": "Does it address the user's question?",
    "completeness": "Is all necessary information included?",
    "clarity": "Is the response clear and understandable?"
}
```

### Feedback Collection

Collect user feedback:
- Add rating buttons in Chat UI
- Track user satisfaction
- Identify improvement areas
- Measure changes over time

## Advanced Features

### Custom Metadata

Add custom metadata to traces:

```python
# Example: Adding custom metadata
metadata = {
    "user_id": "user123",
    "session_id": "session456",
    "environment": "production",
    "version": "1.0.0"
}
```

### Filtering and Search

Use Langfuse filters to:
- Find specific interactions
- Filter by user or session
- Search by keywords
- Filter by date range
- Group by metadata

### Annotations

Add annotations to traces:
- Mark important interactions
- Tag issues for review
- Add notes for team members
- Create improvement tasks

## Team Collaboration

### Sharing Insights

Collaborate with your team:
- Share specific traces
- Create dashboards
- Export reports
- Discuss findings

### Access Control

Manage team access:
- Invite team members
- Set role permissions
- Control data access
- Audit team activity

## Best Practices

### Regular Monitoring

Establish monitoring routines:
- **Daily**: Check error rates and critical issues
- **Weekly**: Review performance trends
- **Monthly**: Analyze cost and usage patterns
- **Quarterly**: Evaluate overall agent quality

### Alert Configuration

Set up alerts for:
- High error rates
- Slow response times
- Unusual cost spikes
- System failures

### Documentation

Document your findings:
- Keep a changelog of improvements
- Document known issues
- Share best practices
- Maintain runbooks

### Continuous Improvement

Use observability data to:
- Identify optimization opportunities
- Test improvements
- Measure impact
- Iterate on agent design

## Troubleshooting

### No Traces Appearing

If traces aren't showing in Langfuse:

1. **Verify configuration**:
   ```bash
   uv run orchestrate settings observability langfuse get
   ```

2. **Check credentials**:
   - Ensure API key is correct
   - Verify public key matches
   - Test keys in Langfuse UI

3. **Review logs**:
   ```bash
   uv run orchestrate server logs | grep langfuse
   ```

4. **Test connection**:
   - Make a test agent interaction
   - Wait a few seconds for data to appear
   - Refresh Langfuse dashboard

### Incomplete Traces

If traces are missing information:

- Check that all tools are properly instrumented
- Verify model calls are being tracked
- Review Orchestrate version compatibility
- Check for network issues

### Performance Impact

If observability affects performance:

- Review sampling rate settings
- Check network latency to Langfuse
- Consider self-hosted Langfuse for lower latency
- Optimize trace data size

## Self-Hosted Langfuse

For enterprise deployments, consider self-hosting:

### Benefits

- **Data sovereignty** - Keep data in your infrastructure
- **Lower latency** - Reduce network overhead
- **Custom configuration** - Full control over settings
- **Cost control** - No per-trace pricing

### Configuration

For self-hosted Langfuse, update the URL:

```bash
uv run orchestrate settings observability langfuse configure \
  --url "https://your-langfuse-instance.com/api/public/otel" \
  --api-key "YOUR_API_KEY" \
  --health-uri "https://your-langfuse-instance.com" \
  --config-json '{"public_key": "YOUR_PUBLIC_KEY"}'
```

## Integration with Other Tools

### Exporting Data

Export Langfuse data to:
- Data warehouses (BigQuery, Snowflake)
- Analytics platforms (Tableau, Looker)
- Monitoring systems (Datadog, New Relic)
- Custom dashboards

### API Access

Use Langfuse API for:
- Custom integrations
- Automated reporting
- Data analysis
- Workflow automation

## Next Steps

Now that you have observability set up:

- **[Monitor your agents](agents)** - Track agent performance
- **[Optimize tools](tools)** - Improve tool efficiency
- **[Review integrations](integrations)** - Check external service performance

---

## Quick Reference

### Configuration Commands

```bash
# Configure Langfuse
uv run orchestrate settings observability langfuse configure \
  --url "https://cloud.langfuse.com/api/public/otel" \
  --api-key "YOUR_API_KEY" \
  --health-uri "https://cloud.langfuse.com" \
  --config-json '{"public_key": "YOUR_PUBLIC_KEY"}'

# View current configuration
uv run orchestrate settings observability langfuse get

# Remove configuration
uv run orchestrate settings observability langfuse remove
```

### Key Metrics to Monitor

| Metric | What to Track | Target |
|--------|---------------|--------|
| Response Time | Average latency | < 2 seconds |
| Error Rate | Failed interactions | < 1% |
| Token Usage | Tokens per interaction | Minimize |
| Cost | $ per interaction | Budget dependent |
| User Satisfaction | Feedback ratings | > 4.0/5.0 |

### Useful Langfuse Features

- **Traces** - View complete interaction flows
- **Sessions** - Group related interactions
- **Users** - Track per-user metrics
- **Scores** - Evaluate response quality
- **Datasets** - Create test sets
- **Playground** - Test prompts

## Additional Resources

- [Langfuse Documentation](https://langfuse.com/docs)
- [Langfuse GitHub](https://github.com/langfuse/langfuse)
- [OpenTelemetry Integration](https://langfuse.com/docs/integrations/opentelemetry)
- [Best Practices Guide](https://langfuse.com/docs/guides/best-practices)