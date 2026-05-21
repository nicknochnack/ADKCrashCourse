---
title: "Watsonx Orchestrate ADK Course"
layout: default
nav_order: 1
description: "Building Agents with Watsonx Orchestrate ADK - A comprehensive crash course"
permalink: /
---

# Building Agents with Watsonx Orchestrate ADK

Welcome to the ultimate crash course on building AI agents with Watsonx Orchestrate! This comprehensive course will take you from zero to hero in building powerful agents using the Watsonx Orchestrate Agent Development Kit (ADK).

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7346742234089738240" height="284" width="504" frameborder="0" allowfullscreen="" title="Embedded post" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe></div>

## Course Overview

This course provides hands-on experience with building AI agents using Watsonx Orchestrate's powerful Agent Development Kit. Whether you're just getting started or looking to build production-ready agents, this course covers everything you need to know.

## What You'll Learn

By the end of this course, you'll be able to:
- Set up and configure Watsonx Orchestrate locally
- Create agents programmatically using YAML configurations
- Build custom Python tools for your agents
- Integrate Langflow workflows into your agents
- Add third-party AI models (Anthropic Claude, etc.)
- Configure observability with Langfuse
- Import external agents via A2A protocol
- Integrate Microsoft Copilot agents

## Getting Started

### Prerequisites

Before you begin, make sure you have:
- Python 3.11+ installed
- UV package manager
- IBM Cloud account with Watsonx access
- Entitlement key from [IBM Container Library](https://myibm.ibm.com/products-services/containerlibrary)

### Initial Setup 🚀

1. **Create UV project**:
   ```bash
   uv init
   ```

2. **Install Python library**:
   ```bash
   uv add ibm-watsonx-orchestrate
   ```

3. **Verify installation**:
   ```bash
   orchestrate --version
   ```

4. **Get your entitlement key** from [IBM Container Library](https://myibm.ibm.com/products-services/containerlibrary)

5. **Configure environment**:
   Create a `.env` file with:
   ```
   WO_DEVELOPER_EDITION_SOURCE=myibm
   WO_ENTITLEMENT_KEY=your_key_here
   WATSONX_APIKEY=your_api_key
   WATSONX_SPACE_ID=your_space_id
   ```

6. **Install Orchestrate server**:
   ```bash
   uv run orchestrate server start --env-file='.env'
   ```

7. **Activate local tenant**:
   ```bash
   uv run orchestrate env activate local
   ```

8. **Start the Chat UI**:
   ```bash
   uv run orchestrate chat start
   ```

## Course Sections

### [1. Creating Agents Programmatically](orchestrate#creating-agents)

Learn how to define and import agents using YAML configuration files. Build agents with custom descriptions, LLM models, and reasoning styles.

### [2. Building Python Tools](orchestrate#python-tools)

Create custom Python tools that your agents can use. Learn to build tools for stock prices, data analysis, and more with proper permissions and dependencies.

### [3. Langflow Integration](orchestrate#langflow)

Integrate visual workflows from Langflow into your Orchestrate agents. Configure connections, set credentials, and import tools seamlessly.

### [4. Third-Party Models](orchestrate#third-party-models)

Add models from providers like Anthropic Claude and other AI services. Configure credentials and import models for enhanced agent capabilities.

### [5. Observability with Langfuse](orchestrate#langfuse)

Set up monitoring and observability for your agents using Langfuse. Track performance, debug issues, and optimize your agent workflows.

### [6. Advanced Integrations](orchestrate#advanced)

Explore advanced topics including:
- External agent integration via A2A protocol
- Microsoft Copilot agent integration
- Server management and maintenance

## Quick Reference

### Server Management

**Stop the server**:
```bash
uv run orchestrate server stop
```

**Reset the server**:
```bash
uv run orchestrate server reset
```

**View server logs**:
```bash
uv run orchestrate server logs
```

### Common Commands

**List agents**:
```bash
uv run orchestrate agents list
```

**List tools**:
```bash
uv run orchestrate tools list
```

**List connections**:
```bash
uv run orchestrate connections list
```

## Video Resources

Throughout this course, you'll find embedded LinkedIn videos demonstrating key concepts:

- **Introduction to Watsonx Orchestrate** - Getting started overview
- **Langflow Integration** - Visual workflow development
- **External Agents via A2A** - Agent-to-agent communication
- **Copilot Integration** - Microsoft Copilot agents in Orchestrate

## Repository Structure

```
22-05-2026-ADKCrashCourse/
├── agents/              # Agent configuration files
│   └── agent.yaml
├── models/              # Model configuration files
│   └── claude.yaml
├── tools/               # Python tools and dependencies
│   ├── stockprice.py
│   └── stocks.txt
└── docs/               # Course documentation (this site)
```

## Support & Resources

- [Official IBM Documentation](https://www.ibm.com/docs/en/watsonx/watson-orchestrate)
- [Agent Development Kit](https://developer.watson-orchestrate.ibm.com/)
- [GitHub Repository](https://github.com/nicknochnack/WatsonxAgenticCrashCourse)

## Next Steps

Ready to dive in? Head over to the [Orchestrate ADK Guide](orchestrate) to start building your first agent!

---

**Instructor**: Nick Renotte  
**Version**: 1.0  
**License**: MIT License