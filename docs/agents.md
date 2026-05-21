---
layout: default
title: "Creating Agents"
nav_order: 3
description: "Build agents programmatically with YAML configurations"
parent: "Watsonx Orchestrate ADK"
---

# Creating Agents Programmatically

Learn how to define and deploy AI agents using YAML configuration files.

## Overview

Watsonx Orchestrate allows you to create agents programmatically using simple YAML files. This approach provides:

- **Version control** - Track agent configurations in Git
- **Reproducibility** - Deploy identical agents across environments
- **Automation** - Integrate agent creation into CI/CD pipelines
- **Flexibility** - Easily modify and iterate on agent designs

## Basic Agent Configuration

### Creating Your First Agent

1. **Create an agent YAML file** (`agent.yaml`):

```yaml
spec_version: v1 
kind: native 
name: nicks_stock_agent
description: a stock research agent that can find stock prices, stock info and income details
llm: watsonx/meta-llama/llama-4-maverick-17b-128e-instruct-fp8
style: react
```

2. **Import the agent into Watsonx Orchestrate**:

```bash
uv run orchestrate agents import -f agent.yaml
```

3. **Verify the agent was created**:

```bash
uv run orchestrate agents list
```

You should see your agent listed with its name and description.

## Configuration Parameters

### Required Fields

| Parameter | Description | Example |
|-----------|-------------|---------|
| `spec_version` | Version of the specification | `v1` |
| `kind` | Type of agent | `native` |
| `name` | Unique identifier for the agent | `nicks_stock_agent` |
| `description` | What the agent does | `a stock research agent...` |
| `llm` | Language model to use | `watsonx/meta-llama/...` |
| `style` | Reasoning pattern | `react` |

### spec_version

Currently, only `v1` is supported. This field ensures compatibility with the Orchestrate platform.

### kind

The `kind` field specifies the agent type:
- **`native`** - Standard Orchestrate agents with full platform integration
- Other types may be available for specialized use cases

### name

The agent's unique identifier. Must be:
- Lowercase letters, numbers, and underscores only
- Unique within your Orchestrate environment
- Descriptive of the agent's purpose

{: .note }
> Choose meaningful names that clearly indicate the agent's function, like `customer_support_agent` or `data_analysis_agent`.

### description

A clear explanation of what the agent does. This helps:
- Users understand when to use the agent
- The system route requests appropriately
- Documentation stay current

**Best practices:**
- Be specific about capabilities
- Mention key tools or knowledge areas
- Keep it concise but informative

### llm

Specifies which language model powers the agent. Options include:

**Watsonx Models:**
```yaml
llm: watsonx/meta-llama/llama-4-maverick-17b-128e-instruct-fp8
llm: watsonx/ibm/granite-3-8b-instruct
```

**Third-Party Models** (after configuration):
```yaml
llm: anthropic/claude-haiku-4-5
llm: openai/gpt-4
```

See [Integrations](integrations#third-party-models) for adding third-party models.

### style

The reasoning pattern the agent uses:

- **`react`** - ReAct (Reasoning + Acting) pattern
  - Thinks through problems step-by-step
  - Decides which tools to use
  - Best for complex, multi-step tasks

Other styles may be available depending on your Orchestrate version.

## Advanced Configuration Examples

### Research Agent

```yaml
spec_version: v1
kind: native
name: research_assistant
description: An AI research assistant that can search the web, analyze documents, and summarize findings
llm: watsonx/meta-llama/llama-4-maverick-17b-128e-instruct-fp8
style: react
```

### Customer Support Agent

```yaml
spec_version: v1
kind: native
name: customer_support_bot
description: A customer support agent that can answer questions, look up orders, and escalate issues
llm: watsonx/ibm/granite-3-8b-instruct
style: react
```

### Data Analysis Agent

```yaml
spec_version: v1
kind: native
name: data_analyst
description: An agent specialized in analyzing datasets, generating insights, and creating visualizations
llm: anthropic/claude-haiku-4-5
style: react
```

## Managing Agents

### List All Agents

View all agents in your environment:

```bash
uv run orchestrate agents list
```

### Update an Agent

To update an agent, modify its YAML file and re-import:

```bash
uv run orchestrate agents import -f agent.yaml
```

{: .warning }
> Re-importing with the same name will update the existing agent.

### Delete an Agent

Remove an agent from your environment:

```bash
uv run orchestrate agents delete -n agent_name
```

### Export an Agent

Export an agent's configuration:

```bash
uv run orchestrate agents export -n agent_name -o exported_agent.yaml
```

## Best Practices

### Naming Conventions

- Use descriptive, lowercase names with underscores
- Include the agent's domain or function
- Examples: `sales_assistant`, `code_reviewer`, `data_processor`

### Descriptions

Write clear, actionable descriptions:

✅ **Good:**
```yaml
description: A financial analyst agent that retrieves stock prices, analyzes market trends, and provides investment insights
```

❌ **Avoid:**
```yaml
description: An agent that does stuff with stocks
```

### Model Selection

Choose models based on your needs:

- **Fast responses** - Smaller models like Granite 3-8B
- **Complex reasoning** - Larger models like Llama 4 Maverick
- **Specialized tasks** - Third-party models with specific capabilities

### Version Control

Store agent YAML files in Git:

```bash
agents/
├── customer_support.yaml
├── data_analyst.yaml
└── research_assistant.yaml
```

This enables:
- Change tracking
- Collaboration
- Rollback capabilities
- Environment consistency

## Testing Your Agent

After creating an agent:

1. **Start the Chat UI**:
   ```bash
   uv run orchestrate chat start
   ```

2. **Select your agent** from the available agents list

3. **Test with sample queries** relevant to the agent's purpose

4. **Iterate** - Modify the YAML and re-import based on results

## Common Issues

### Agent Not Appearing

If your agent doesn't show up after import:

1. Check for YAML syntax errors
2. Verify the name is unique
3. Ensure the LLM model exists
4. Review server logs: `uv run orchestrate server logs`

### Import Errors

Common import issues:

- **Invalid YAML syntax** - Use a YAML validator
- **Missing required fields** - Check all required parameters are present
- **Invalid model name** - Verify the LLM exists in your environment

### Agent Not Responding

If the agent doesn't respond properly:

1. Check the model is properly configured
2. Verify tools are imported (if the agent needs them)
3. Review the agent's description for clarity
4. Check server logs for errors

## Next Steps

Now that you can create agents, learn to enhance them with:

- **[Python Tools](tools)** - Add custom capabilities to your agents
- **[Integrations](integrations)** - Connect Langflow workflows and third-party models
- **[Observability](observability)** - Monitor and optimize agent performance

---

## Quick Reference

```bash
# Agent management commands
uv run orchestrate agents list                    # List all agents
uv run orchestrate agents import -f agent.yaml    # Import/update agent
uv run orchestrate agents delete -n agent_name    # Delete agent
uv run orchestrate agents export -n agent_name    # Export agent config
```

## Example Agent Templates

### Minimal Agent

```yaml
spec_version: v1
kind: native
name: simple_agent
description: A basic conversational agent
llm: watsonx/ibm/granite-3-8b-instruct
style: react
```

### Production Agent

```yaml
spec_version: v1
kind: native
name: production_assistant
description: A production-ready agent with comprehensive capabilities for customer service, including order lookup, FAQ responses, and issue escalation
llm: watsonx/meta-llama/llama-4-maverick-17b-128e-instruct-fp8
style: react