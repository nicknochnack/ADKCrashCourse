---
layout: default
title: "Setup & Installation"
nav_order: 2
description: "Getting started with Watsonx Orchestrate ADK"
parent: "Watsonx Orchestrate ADK"
---

# Setup & Installation

Get your Watsonx Orchestrate development environment up and running in minutes.

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7346742234089738240" height="284" width="504" frameborder="0" allowfullscreen="" title="Embedded post" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe></div>

## Prerequisites

Before you begin, ensure you have:
- **Python 3.11 or higher** installed on your system
- **UV package manager** for Python dependency management
- **IBM Cloud account** with Watsonx access
- **Entitlement key** from [IBM Container Library](https://myibm.ibm.com/products-services/containerlibrary)

## Installation Steps

### 1. Create a UV Project

Initialize a new UV project in your working directory:

```bash
uv init
```

This creates the basic project structure for managing Python dependencies.

### 2. Install Watsonx Orchestrate Library

Add the IBM Watsonx Orchestrate library to your project:

```bash
uv add ibm-watsonx-orchestrate
```

### 3. Verify Installation

Check that the Orchestrate CLI is properly installed:

```bash
orchestrate --version
```

You should see the version number displayed, confirming successful installation.

### 4. Get Your Entitlement Key

1. Visit [IBM Container Library](https://myibm.ibm.com/products-services/containerlibrary)
2. Sign in with your IBM Cloud credentials
3. Navigate to the entitlement keys section
4. Generate and copy your entitlement key

{: .important }
> Keep your entitlement key secure! It provides access to IBM's container registry and should be treated like a password.

### 5. Configure Environment Variables

Create a `.env` file in your project root with the following configuration:

```bash
WO_DEVELOPER_EDITION_SOURCE=myibm
WO_ENTITLEMENT_KEY=your_entitlement_key_here
WATSONX_APIKEY=your_watsonx_api_key
WATSONX_SPACE_ID=your_space_id
```

**Where to find these values:**
- **WO_ENTITLEMENT_KEY**: From step 4 above
- **WATSONX_APIKEY**: From your IBM Cloud Watsonx instance
- **WATSONX_SPACE_ID**: From your Watsonx deployment space

### 6. Start the Orchestrate Server

Install and start the Orchestrate server using your environment configuration:

```bash
uv run orchestrate server start --env-file='.env'
```

This command will:
- Download necessary container images
- Initialize the local Orchestrate environment
- Start the server on default ports

{: .note }
> The first startup may take several minutes as it downloads required images.

### 7. Activate Local Tenant

Activate the local development tenant:

```bash
uv run orchestrate env activate local
```

This sets up your local environment for development and testing.

### 8. Launch the Chat UI

Start the interactive chat interface:

```bash
uv run orchestrate chat start
```

The Chat UI will open in your default browser, allowing you to interact with your agents.

## Verification

To verify your setup is complete:

1. **Check server status**:
   ```bash
   uv run orchestrate server logs
   ```

2. **List environments**:
   ```bash
   uv run orchestrate env list
   ```

3. **Access the Chat UI** at `http://localhost:8080` (or the port shown in your terminal)

## Troubleshooting

### Port Already in Use

If you see port conflict errors:

```bash
# Stop any running Orchestrate instances
uv run orchestrate server stop

# Then restart
uv run orchestrate server start --env-file='.env'
```

### Authentication Errors

If you encounter authentication issues:

1. Verify your entitlement key is correct
2. Check that your Watsonx API key is valid
3. Ensure your Space ID matches your Watsonx deployment

### Container Download Issues

If container downloads fail:

1. Check your internet connection
2. Verify your entitlement key has proper permissions
3. Try clearing the cache and restarting:
   ```bash
   uv run orchestrate server reset
   uv run orchestrate server start --env-file='.env'
   ```

## Server Management Commands

### Stop the Server

```bash
uv run orchestrate server stop
```

### View Server Logs

```bash
uv run orchestrate server logs
```

### Reset the Server

{: .warning }
> This will delete all data and configurations!

```bash
uv run orchestrate server reset
```

### Restart the Server

```bash
uv run orchestrate server stop
uv run orchestrate server start --env-file='.env'
```

## Next Steps

Now that your environment is set up, you're ready to:

- **[Create your first agent](agents)** - Learn to build agents programmatically
- **[Build Python tools](tools)** - Create custom tools for your agents
- **[Explore integrations](integrations)** - Connect Langflow and third-party models

---

## Quick Reference

```bash
# Essential commands
uv run orchestrate --version              # Check version
uv run orchestrate server start           # Start server
uv run orchestrate server stop            # Stop server
uv run orchestrate server logs            # View logs
uv run orchestrate env activate local     # Activate environment
uv run orchestrate chat start             # Launch Chat UI
```

## Additional Resources

- [Official IBM Documentation](https://www.ibm.com/docs/en/watsonx/watson-orchestrate)
- [Agent Development Kit](https://developer.watson-orchestrate.ibm.com/)
- [IBM Cloud Console](https://cloud.ibm.com/)