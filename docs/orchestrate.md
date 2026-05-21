---
layout: default
title: "Watsonx Orchestrate ADK"
nav_order: 2
description: "Complete guide to building agents with Watsonx Orchestrate Agent Development Kit"
has_children: true
---

# Watsonx Orchestrate Agent Development Kit

Welcome to the comprehensive guide for building AI agents with Watsonx Orchestrate ADK!

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7346742234089738240" height="284" width="504" frameborder="0" allowfullscreen="" title="Embedded post" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe></div>

## What is Watsonx Orchestrate ADK?

The Watsonx Orchestrate Agent Development Kit (ADK) is a powerful platform for building, deploying, and managing AI agents. It provides:

- **Programmatic agent creation** using YAML configurations
- **Custom Python tools** for extending agent capabilities
- **Visual workflow integration** with Langflow
- **Third-party model support** from providers like Anthropic and OpenAI
- **Production-ready observability** with Langfuse
- **Enterprise features** for security, governance, and scale

## Course Structure

This guide is organized into focused sections that build upon each other:

### [Setup & Installation](setup)

Get your development environment up and running:
- Install Watsonx Orchestrate
- Configure credentials
- Start the local server
- Launch the Chat UI

**Start here if you're new to Orchestrate!**

### [Creating Agents](agents)

Learn to build agents programmatically:
- Define agents with YAML
- Configure language models
- Set reasoning styles
- Manage agent lifecycle

**Perfect for understanding agent fundamentals.**

### [Building Python Tools](tools)

Extend your agents with custom capabilities:
- Create Python tools
- Define tool parameters
- Handle dependencies
- Implement best practices

**Essential for adding custom functionality.**

### [Integrations](integrations)

Connect external services and platforms:
- Integrate Langflow workflows
- Add third-party AI models
- Import external agents via A2A
- Connect Microsoft Copilot

**Expand your agent ecosystem.**

### [Observability](observability)

Monitor and optimize your agents:
- Configure Langfuse
- Track performance metrics
- Debug issues
- Optimize costs

**Critical for production deployments.**

## Learning Paths

Choose your path based on your goals:

### Quick Start (30 minutes)

1. [Setup & Installation](setup) - Get Orchestrate running
2. [Creating Agents](agents) - Build your first agent
3. Test in Chat UI

**Goal**: Have a working agent in 30 minutes

### Developer Path (2-3 hours)

1. [Setup & Installation](setup)
2. [Creating Agents](agents)
3. [Building Python Tools](tools)
4. [Integrations](integrations) - Langflow section

**Goal**: Build agents with custom tools and workflows

### Production Path (Full course)

1. Complete all sections in order
2. Focus on [Observability](observability)
3. Implement monitoring and optimization

**Goal**: Deploy production-ready agents with full observability

## Key Features

### Programmatic Agent Creation

Define agents as code for version control and automation:

```yaml
spec_version: v1
kind: native
name: my_agent
description: A powerful AI agent
llm: watsonx/meta-llama/llama-4-maverick-17b-128e-instruct-fp8
style: react
```

### Custom Python Tools

Extend agents with any Python capability:

```python
@tool(name='my_tool', description='does something useful', permission=ToolPermission.USER)
def my_function(param: str) -> dict:
    return {"result": "success"}
```

### Visual Workflows

Integrate Langflow's drag-and-drop interface for complex logic without coding.

### Multi-Model Support

Use models from multiple providers in a single environment:
- Watsonx models (Llama, Granite)
- Anthropic Claude
- OpenAI GPT
- And more...

### Enterprise Ready

Built for production with:
- Observability and monitoring
- Security and governance
- Scalability features
- Team collaboration tools

## Prerequisites

Before starting, ensure you have:

- **Python 3.11+** installed
- **UV package manager** for dependencies
- **IBM Cloud account** with Watsonx access
- **Entitlement key** from IBM Container Library

See [Setup & Installation](setup) for detailed requirements.

## Common Use Cases

### Customer Support Agents

Build agents that:
- Answer customer questions
- Look up order information
- Escalate complex issues
- Provide 24/7 support

### Research Assistants

Create agents that:
- Search academic papers
- Summarize findings
- Analyze data
- Generate reports

### Data Analysis Agents

Develop agents that:
- Process datasets
- Generate insights
- Create visualizations
- Automate reporting

### Code Assistants

Build agents that:
- Review code
- Suggest improvements
- Generate documentation
- Debug issues

## Quick Reference

### Essential Commands

```bash
# Server management
uv run orchestrate server start --env-file='.env'
uv run orchestrate server stop
uv run orchestrate server logs

# Agent management
uv run orchestrate agents list
uv run orchestrate agents import -f agent.yaml

# Tool management
uv run orchestrate tools list
uv run orchestrate tools import -k python -f tool.py -r requirements.txt

# Environment
uv run orchestrate env activate local
uv run orchestrate chat start
```

### Project Structure

```
your-project/
├── .env                    # Environment configuration
├── agents/                 # Agent YAML files
│   └── my_agent.yaml
├── tools/                  # Python tools
│   ├── tool1.py
│   └── requirements.txt
└── models/                 # Model configurations
    └── claude.yaml
```

## Getting Help

### Documentation

- [Official IBM Docs](https://www.ibm.com/docs/en/watsonx/watson-orchestrate)
- [Agent Development Kit](https://developer.watson-orchestrate.ibm.com/)
- This course documentation

### Community

- GitHub Issues
- IBM Community Forums
- Stack Overflow (tag: watsonx-orchestrate)

### Video Resources

Throughout this course, you'll find embedded LinkedIn videos demonstrating key concepts:
- Introduction to Watsonx Orchestrate
- Langflow Integration
- External Agents via A2A
- Microsoft Copilot Integration

## Next Steps

Ready to get started? Head to [Setup & Installation](setup) to begin your journey!

Already set up? Jump to:
- [Creating Agents](agents) - Build your first agent
- [Building Tools](tools) - Add custom capabilities
- [Integrations](integrations) - Connect external services
- [Observability](observability) - Monitor and optimize

---

## About This Course

This course is designed to take you from zero to production-ready agents. Each section builds on the previous one, but you can also jump to specific topics as needed.

**Estimated Time**: 4-6 hours for complete course  
**Level**: Beginner to Advanced  
**Prerequisites**: Basic Python knowledge helpful but not required

**Let's build something amazing!** 🚀