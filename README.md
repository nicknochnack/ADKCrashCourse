# Building Agents with Watsonx Orchestrate ADK
How to build your own agents with Watsonx Orchestrate. Shown on a bunch of live stages globally in Sydney. 

# Startup (local instances)🚀
1. Create uv project: `uv init`
2. Install python library: `uv add ibm-watsonx-orchestrate`
3. Double check orchestrate version: `orchestrate --version`
4. Get entitlement key from https://myibm.ibm.com/products-services/containerlibrary
5. Rename sample.env file to `.env` and fill out this stuff 
   - WO_DEVELOPER_EDITION_SOURCE=myibm
   - WO_ENTITLEMENT_KEY=
   - WATSONX_APIKEY=
   - WATSONX_SPACE_ID=
6. Install orchestrate: `uv run orchestrate server start --env-file='.env'`
7. Activate local tenant: `uv run orchestrate env activate local`
8. Start the Chat UI: `uv run orchestrate chat start`

**📺 [Watch: Introduction to Watsonx Orchestrate](https://www.linkedin.com/feed/update/urn:li:ugcPost:7346742234089738240)**

---

## Programmatically Creating Agents 
1. Create a new yaml file. Example below: 
```yaml
# agent.yaml
spec_version: v1 
kind: native 
name: nicks_stock_agent
description: a stock research agent that can find stock prices, stock info and income details
llm: watsonx/meta-llama/llama-4-maverick-17b-128e-instruct-fp8
style: react
```
2. Import it into watsonx orchestrate `uv run orchestrate agents import -f agent.yaml`
3. View the agent by listing out agents: `uv run orchestrate agents list`

## Building Python Tools 
1. Create a workflow in Python, this gets stock prices:
```python
# stockprices.py
from ibm_watsonx_orchestrate.agent_builder.tools import tool, ToolPermission
import yfinance as yf

@tool(name='get_stock_price', description='a tool that returns the last stock price given a stock ticker', permission=ToolPermission.ADMIN)
def stock_price(stock_ticker: str): 
    data = yf.Ticker(stock_ticker)
    prices = data.history(period='1mo')
    return prices['Close'].iloc[-1]
```

2. You also need an associated requirements file. Here I've got just two dependencies:   
```
# requirements.txt
yfinance==1.3.0
tabulate==0.10.0
```

3. Import a tool: `uv run orchestrate tools import -k python -f "stockprices.py" -r "requirements.txt"`
4. View the tool: `uv run orchestrate tools list`

## Adding in Langflow

**📺 [Watch: Langflow Integration](https://www.linkedin.com/feed/update/urn:li:ugcPost:7394963536256516096)**

1. Create a connection: `uv run orchestrate connections add --app-id LFAPP`
2. Configure an environment and key value pairs for mcp servers: `uv run orchestrate connections configure --app-id LFAPP --environment draft --type team -k key_value`
3. Set the credentials: `uv run orchestrate connections set-credentials -a LFAPP --env draft -e TAVILY_SEARCH_API_KEY=tvly-dev-key`
4. Import the exported tool from Langflow: `uv run orchestrate tools import -f ArxivTool.json -r tools/langflow.txt -k langflow --app-id ArxivAgent`

## Adding in Third Party Models 
1. Create a new connection: `uv run orchestrate connections add -a anthropic_credentials`
2. Specify the credentials placeholder: `uv run orchestrate connections configure -a anthropic_credentials --env draft -k key_value -t team`
3. Set the credentials - replace with your api key here: `uv run orchestrate connections set-credentials -a anthropic_credentials --env draft -e "api_key=YOURAPIKEYHERE"`
4. Configure the model you want to import inside a yaml file:
```yaml
# model.yaml
spec_version: v1
kind: model
name: anthropic/claude-haiku-4-5
display_name: Anthropic Claude Haiku 4.5
description: |
    Anthropic Claude model for safe and helpful AI interactions.
tags:
- anthropic
- claude
model_type: chat
provider_config: {}
```
5. Import the model: `uv run orchestrate models import --file models/claude.yaml --app-id anthropic_credentials`

### Adding Other Providers (e.g., watsonx.ai)
```bash
uv run orchestrate connections add -a watsonx_credentials
uv run orchestrate connections configure -a watsonx_credentials --env draft -k key_value -t team
uv run orchestrate connections set-credentials -a watsonx_credentials --env draft -e "api_key=3y5RO9NvHBoALqHSfsrJ43ke0dMPS99z1iQv3SWr3ybg"
uv run orchestrate models import --file models/watsonx.yaml --app-id watsonx_credentials
```

## Configuring Langfuse 
1. Get your Langfuse apikey and public key from the instance. Once you create a project you'll have these provided. 
2. Replace the api key and public key in this command:
```bash
uv run orchestrate settings observability langfuse configure --url "https://cloud.langfuse.com/api/public/otel" --api-key "YOURAPIKEYHERE" --health-uri "https://cloud.langfuse.com" --config-json '{"public_key": "YOURPUBLICKEYHERE"}'
```

## Cleaning up the Server and Upgrading
- **Stopping the server and resetting:**
  ```bash
  uv run orchestrate server stop
  uv run orchestrate server reset
  ```
- **Viewing server logs:**
  ```bash
  uv run orchestrate server logs
  ```

---

# More Stuff

## Importing External Agents via A2A

**📺 [Watch: External Agents via A2A](https://www.linkedin.com/feed/update/urn:li:ugcPost:7442052561966186496)**

**Code:** https://github.com/nicknochnack/WatsonxOrchestrateExternalAgents 

## Importing Copilot Agents into Watsonx Orchestrate 

**📺 [Watch: Copilot Agents Integration](https://www.linkedin.com/feed/update/urn:li:ugcPost:7460214632683651072)**

**Code:** https://github.com/nicknochnack/A2ACopilotXOrchestrate