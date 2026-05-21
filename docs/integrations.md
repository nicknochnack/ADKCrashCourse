---
layout: default
title: "Integrations"
nav_order: 5
description: "Integrate Langflow, third-party models, and external agents"
parent: "Watsonx Orchestrate ADK"
---

# Integrations

Extend your Orchestrate environment with Langflow workflows, third-party AI models, and external agents.

## Table of Contents

- [Langflow Integration](#langflow-integration)
- [Third-Party Models](#third-party-models)
- [External Agents via A2A](#external-agents)
- [Microsoft Copilot Integration](#copilot-integration)

---

## Langflow Integration {#langflow-integration}

Integrate visual workflows from Langflow into your Orchestrate agents.

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7394963536256516096" height="284" width="504" frameborder="0" allowfullscreen="" title="Embedded post" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe></div>

### What is Langflow?

Langflow is a visual framework for building AI workflows using a drag-and-drop interface. By integrating Langflow with Orchestrate, you can:

- Create complex workflows visually
- Combine multiple AI components
- Integrate external APIs easily
- Test and iterate quickly

### Setup Steps

#### 1. Create a Connection

First, create a connection for your Langflow application:

```bash
uv run orchestrate connections add --app-id LFAPP
```

The `app-id` is a unique identifier for your Langflow connection.

#### 2. Configure the Environment

Set up the connection environment with key-value pairs:

```bash
uv run orchestrate connections configure --app-id LFAPP --environment draft --type team -k key_value
```

**Parameters:**
- `--app-id`: Your Langflow app identifier
- `--environment`: Environment name (draft, production, etc.)
- `--type`: Connection type (team, personal)
- `-k`: Configuration type (key_value for credentials)

#### 3. Set Credentials

Add any required API keys or credentials:

```bash
uv run orchestrate connections set-credentials -a LFAPP --env draft -e TAVILY_SEARCH_API_KEY=tvly-dev-key
```

You can set multiple credentials:

```bash
uv run orchestrate connections set-credentials -a LFAPP --env draft -e API_KEY_1=value1 -e API_KEY_2=value2
```

#### 4. Import the Langflow Tool

Export your workflow from Langflow as JSON, then import it:

```bash
uv run orchestrate tools import -f ArxivTool.json -r tools/langflow.txt -k langflow --app-id ArxivAgent
```

**Parameters:**
- `-f`: Path to exported Langflow JSON file
- `-r`: Requirements file for dependencies
- `-k`: Tool kind (langflow)
- `--app-id`: Application identifier for this tool

### Langflow Requirements File

Create a `langflow.txt` file with necessary dependencies:

```
langflow==1.0.0
tavily-python==0.3.0
# Add other dependencies your workflow needs
```

### Example Workflows

#### Research Assistant Workflow

A Langflow workflow that:
1. Searches academic papers using Arxiv
2. Summarizes findings
3. Returns formatted results

#### Web Scraping Workflow

A workflow that:
1. Accepts a URL
2. Extracts content
3. Processes and structures data
4. Returns clean results

### Best Practices

- **Test in Langflow first** - Ensure workflows work before importing
- **Document dependencies** - List all required packages
- **Use environment variables** - Store API keys securely
- **Version control** - Keep Langflow JSON files in Git

---

## Third-Party Models {#third-party-models}

Add AI models from providers like Anthropic, OpenAI, and others to your Orchestrate environment.

### Why Add Third-Party Models?

- Access specialized model capabilities
- Compare performance across providers
- Use models optimized for specific tasks
- Leverage latest AI advancements

### Adding Anthropic Claude

#### Step 1: Create Connection

```bash
uv run orchestrate connections add -a anthropic_credentials
```

#### Step 2: Configure Connection

```bash
uv run orchestrate connections configure -a anthropic_credentials --env draft -k key_value -t team
```

#### Step 3: Set API Key

```bash
uv run orchestrate connections set-credentials -a anthropic_credentials --env draft -e "api_key=YOUR_ANTHROPIC_API_KEY"
```

{: .important }
> Replace `YOUR_ANTHROPIC_API_KEY` with your actual Anthropic API key from [console.anthropic.com](https://console.anthropic.com)

#### Step 4: Create Model Configuration

Create a YAML file (`claude.yaml`):

```yaml
spec_version: v1
kind: model
name: anthropic/claude-haiku-4-5
display_name: Anthropic Claude Haiku 4.5
description: |
    Anthropic Claude model for safe and helpful AI interactions.
    Fast, efficient, and great for most tasks.
tags:
- anthropic
- claude
- fast
model_type: chat
provider_config: {}
```

#### Step 5: Import the Model

```bash
uv run orchestrate models import --file models/claude.yaml --app-id anthropic_credentials
```

### Adding Watsonx.ai Models

For additional Watsonx models:

```bash
# Create connection
uv run orchestrate connections add -a watsonx_credentials

# Configure connection
uv run orchestrate connections configure -a watsonx_credentials --env draft -k key_value -t team

# Set credentials
uv run orchestrate connections set-credentials -a watsonx_credentials --env draft -e "api_key=YOUR_WATSONX_API_KEY"

# Import model
uv run orchestrate models import --file models/watsonx.yaml --app-id watsonx_credentials
```

### Model Configuration Reference

```yaml
spec_version: v1           # Always v1
kind: model                # Always model
name: provider/model-name  # Format: provider/model-identifier
display_name: Human Name   # Shown in UI
description: |             # What the model is good for
    Detailed description
    Can be multi-line
tags:                      # For categorization
- provider-name
- capability
- speed-tier
model_type: chat           # chat, completion, embedding, etc.
provider_config: {}        # Provider-specific settings
```

### Supported Providers

Common providers you can integrate:

- **Anthropic** - Claude models
- **OpenAI** - GPT models
- **Watsonx.ai** - IBM models
- **Cohere** - Command models
- **Google** - Gemini models

{: .note }
> Each provider requires its own API key and connection setup.

### Using Third-Party Models

After importing, use the model in your agent configurations:

```yaml
spec_version: v1
kind: native
name: claude_agent
description: An agent powered by Claude
llm: anthropic/claude-haiku-4-5  # Use your imported model
style: react
```

---

## External Agents via A2A {#external-agents}

Import and integrate external agents using the Agent-to-Agent (A2A) protocol.

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7442052561966186496" height="284" width="504" frameborder="0" allowfullscreen="" title="Embedded post" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe></div>

### What is A2A?

Agent-to-Agent (A2A) protocol enables:
- Communication between different agent platforms
- Sharing capabilities across systems
- Building multi-agent architectures
- Leveraging specialized agents from other platforms

### Use Cases

- **Specialized Agents** - Use domain-specific agents from other platforms
- **Legacy Integration** - Connect existing agent systems
- **Multi-Platform** - Combine strengths of different platforms
- **Microservices** - Build agent-based microservice architectures

### Implementation

For detailed implementation guide, see:

**[External Agents Repository](https://github.com/nicknochnack/WatsonxOrchestrateExternalAgents)**

This repository includes:
- Complete A2A protocol implementation
- Example external agents
- Integration patterns
- Testing utilities

### Key Concepts

#### Agent Discovery

External agents can be discovered and registered:

```python
# Example agent registration
agent_config = {
    "name": "external_specialist",
    "endpoint": "https://api.example.com/agent",
    "capabilities": ["data_analysis", "visualization"],
    "protocol": "a2a"
}
```

#### Message Format

A2A uses standardized message formats:

```json
{
  "type": "request",
  "agent_id": "external_specialist",
  "task": "analyze_data",
  "parameters": {
    "data": [...],
    "analysis_type": "statistical"
  }
}
```

---

## Microsoft Copilot Integration {#copilot-integration}

Connect Microsoft Copilot agents to your Watsonx Orchestrate environment.

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7460214632683651072" height="284" width="504" frameborder="0" allowfullscreen="" title="Embedded post" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe></div>

### Why Integrate Copilot?

- **Microsoft 365 Integration** - Access Office apps and data
- **Enterprise Features** - Leverage Microsoft's enterprise capabilities
- **Existing Investments** - Use your current Copilot licenses
- **Unified Experience** - Single interface for all agents

### Implementation

For detailed implementation guide, see:

**[Copilot Integration Repository](https://github.com/nicknochnack/A2ACopilotXOrchestrate)**

This repository includes:
- Copilot-Orchestrate bridge
- Authentication setup
- Example integrations
- Best practices

### Integration Benefits

#### Unified Agent Platform

- Single chat interface for all agents
- Consistent user experience
- Centralized management
- Unified observability

#### Cross-Platform Capabilities

- Use Copilot's Microsoft 365 access
- Combine with Orchestrate's tools
- Share context between agents
- Coordinate complex workflows

### Example Use Cases

#### Document Processing

1. Copilot agent extracts data from SharePoint
2. Orchestrate agent processes and analyzes
3. Results stored back to Microsoft 365

#### Customer Service

1. Orchestrate agent handles initial inquiry
2. Copilot agent accesses customer data from Dynamics
3. Combined response with full context

---

## Connection Management

### List All Connections

```bash
uv run orchestrate connections list
```

### View Connection Details

```bash
uv run orchestrate connections get -a app_id
```

### Update Credentials

```bash
uv run orchestrate connections set-credentials -a app_id --env draft -e KEY=new_value
```

### Delete Connection

```bash
uv run orchestrate connections delete -a app_id
```

## Model Management

### List All Models

```bash
uv run orchestrate models list
```

### View Model Details

```bash
uv run orchestrate models get -n model_name
```

### Delete Model

```bash
uv run orchestrate models delete -n model_name
```

## Troubleshooting

### Connection Issues

**Cannot connect to external service:**
- Verify API keys are correct
- Check network connectivity
- Ensure service is accessible
- Review firewall settings

### Import Failures

**Tool or model import fails:**
- Validate YAML/JSON syntax
- Check all required fields are present
- Verify credentials are set
- Review server logs

### Authentication Errors

**Credential issues:**
- Regenerate API keys
- Check key permissions
- Verify environment settings
- Test credentials independently

## Best Practices

### Security

- **Rotate keys regularly** - Update API keys periodically
- **Use environment-specific keys** - Different keys for dev/prod
- **Limit permissions** - Grant minimum required access
- **Monitor usage** - Track API calls and costs

### Organization

- **Naming conventions** - Use consistent app-id naming
- **Documentation** - Document each integration
- **Version control** - Store configurations in Git
- **Testing** - Test integrations thoroughly

### Performance

- **Cache responses** - Reduce external API calls
- **Set timeouts** - Prevent hanging requests
- **Monitor latency** - Track response times
- **Optimize workflows** - Minimize external dependencies

## Next Steps

Now that you understand integrations, explore:

- **[Observability](observability)** - Monitor your integrations
- **[Creating Agents](agents)** - Build agents using integrated models
- **[Building Tools](tools)** - Create tools that use external services

---

## Quick Reference

```bash
# Connections
uv run orchestrate connections add -a app_id
uv run orchestrate connections configure -a app_id --env draft -k key_value -t team
uv run orchestrate connections set-credentials -a app_id --env draft -e KEY=value
uv run orchestrate connections list

# Models
uv run orchestrate models import --file model.yaml --app-id app_id
uv run orchestrate models list
uv run orchestrate models delete -n model_name

# Langflow Tools
uv run orchestrate tools import -f tool.json -r requirements.txt -k langflow --app-id app_id