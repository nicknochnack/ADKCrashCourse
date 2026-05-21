---
title: "Watsonx Orchestrate ADK Course"
layout: default
nav_order: 1
description: "Building Agents with Watsonx Orchestrate ADK"
permalink: /
---

# Building Agents with Watsonx Orchestrate ADK

Learn to build AI agents with Watsonx Orchestrate's Agent Development Kit (ADK).

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7346742234089738240" height="284" width="504" frameborder="0" allowfullscreen="" title="Embedded post" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe></div>

## Course Pages

### [Setup & Installation](setup)
Get your Watsonx Orchestrate development environment running. Install the ADK, configure credentials, and start the local server.

### [Creating Agents](agents)
Learn to build agents programmatically using YAML configuration files. Define agent behavior, select models, and manage the agent lifecycle.

### [Building Python Tools](tools)
Extend your agents with custom Python tools. Create functions that agents can call to perform specific tasks like fetching data or processing information.

### [Integrations](integrations)
Connect external services to your agents. Integrate Langflow workflows, add third-party AI models (Anthropic, OpenAI), and import external agents via A2A protocol.

### [Observability](observability)
Monitor and optimize your agents with Langfuse. Track performance, debug issues, and analyze agent behavior in production.

---

## Quick Start

```bash
# Install
uv init
uv add ibm-watsonx-orchestrate

# Configure (create .env file)
WO_DEVELOPER_EDITION_SOURCE=myibm
WO_ENTITLEMENT_KEY=your_key
WATSONX_APIKEY=your_api_key
WATSONX_SPACE_ID=your_space_id

# Start
uv run orchestrate server start --env-file='.env'
uv run orchestrate env activate local
uv run orchestrate chat start
```

## Resources

- [Official IBM Documentation](https://www.ibm.com/docs/en/watsonx/watson-orchestrate)
- [Agent Development Kit](https://developer.watson-orchestrate.ibm.com/)
- [GitHub Repository](https://github.com/nicknochnack/22-05-2026-ADKCrashCourse)