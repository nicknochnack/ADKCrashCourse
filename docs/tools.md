---
layout: default
title: "Building Python Tools"
nav_order: 4
description: "Create custom Python tools for your agents"
---

# Building Python Tools

Extend your agents' capabilities by creating custom Python tools that can perform specific tasks.

## Overview

Python tools allow your agents to:
- Access external APIs and services
- Perform calculations and data processing
- Interact with databases
- Execute custom business logic
- Integrate with existing Python libraries

## Creating Your First Tool

### Example: Stock Price Tool

1. **Create a Python file** (`stockprice.py`):

```python
from ibm_watsonx_orchestrate.agent_builder.tools import tool, ToolPermission
import yfinance as yf

@tool(
    name='get_stock_price', 
    description='a tool that returns the last stock price given a stock ticker', 
    permission=ToolPermission.ADMIN
)
def stock_price(stock_ticker: str): 
    """
    Retrieves the most recent closing price for a given stock ticker.
    
    Args:
        stock_ticker: The stock symbol (e.g., 'AAPL', 'GOOGL')
    
    Returns:
        float: The last closing price
    """
    data = yf.Ticker(stock_ticker)
    prices = data.history(period='1mo')
    return prices['Close'].iloc[-1]
```

2. **Create a requirements file** (`requirements.txt`):

```
yfinance==1.3.0
tabulate==0.10.0
```

3. **Import the tool**:

```bash
uv run orchestrate tools import -k python -f "stockprice.py" -r "requirements.txt"
```

4. **Verify the import**:

```bash
uv run orchestrate tools list
```

## Tool Decorator Parameters

### @tool Decorator

The `@tool` decorator marks a function as an Orchestrate tool and accepts these parameters:

| Parameter | Required | Description |
|-----------|----------|-------------|
| `name` | Yes | Unique identifier for the tool |
| `description` | Yes | What the tool does (helps agent decide when to use it) |
| `permission` | Yes | Access level for the tool |

### name

The tool's unique identifier:
- Use lowercase with underscores
- Be descriptive and specific
- Examples: `get_stock_price`, `analyze_sentiment`, `fetch_weather`

### description

A clear explanation of what the tool does:

✅ **Good descriptions:**
```python
description='a tool that returns the last stock price given a stock ticker'
description='analyzes text sentiment and returns positive, negative, or neutral'
description='fetches current weather data for a given city name'
```

❌ **Avoid vague descriptions:**
```python
description='gets data'
description='does stuff with stocks'
```

{: .important }
> The description is crucial! Agents use it to decide when to call your tool. Be specific about inputs and outputs.

### permission

Controls who can use the tool:

```python
from ibm_watsonx_orchestrate.agent_builder.tools import ToolPermission

# Available permission levels
ToolPermission.ADMIN    # Admin users only
ToolPermission.USER     # All authenticated users
ToolPermission.PUBLIC   # Anyone (use with caution)
```

## Function Parameters

### Type Hints

Always use type hints for function parameters:

```python
@tool(name='calculate_sum', description='adds two numbers', permission=ToolPermission.USER)
def add_numbers(a: int, b: int) -> int:
    return a + b
```

Supported types:
- `str` - Text strings
- `int` - Integers
- `float` - Decimal numbers
- `bool` - True/False values
- `list` - Lists of items
- `dict` - Dictionaries/objects

### Docstrings

Include comprehensive docstrings:

```python
@tool(name='fetch_user_data', description='retrieves user information', permission=ToolPermission.ADMIN)
def get_user(user_id: str) -> dict:
    """
    Fetches user data from the database.
    
    Args:
        user_id: The unique identifier for the user
        
    Returns:
        dict: User information including name, email, and preferences
        
    Raises:
        ValueError: If user_id is invalid
        ConnectionError: If database is unavailable
    """
    # Implementation here
    pass
```

## Advanced Tool Examples

### Multi-Parameter Tool

```python
@tool(
    name='search_products',
    description='searches for products by category and price range',
    permission=ToolPermission.USER
)
def search_products(category: str, min_price: float, max_price: float) -> list:
    """
    Searches product catalog with filters.
    
    Args:
        category: Product category (e.g., 'electronics', 'clothing')
        min_price: Minimum price in dollars
        max_price: Maximum price in dollars
        
    Returns:
        list: List of matching products with details
    """
    # Implementation
    results = []
    # ... search logic ...
    return results
```

### Tool with Error Handling

```python
@tool(
    name='fetch_api_data',
    description='retrieves data from external API with error handling',
    permission=ToolPermission.USER
)
def fetch_data(endpoint: str) -> dict:
    """
    Fetches data from an external API.
    
    Args:
        endpoint: API endpoint path
        
    Returns:
        dict: API response data
    """
    import requests
    
    try:
        response = requests.get(f"https://api.example.com/{endpoint}", timeout=10)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        return {"error": str(e), "success": False}
```

### Tool with Complex Return Types

```python
@tool(
    name='analyze_data',
    description='performs statistical analysis on numerical data',
    permission=ToolPermission.USER
)
def analyze(data: list) -> dict:
    """
    Analyzes a list of numbers and returns statistics.
    
    Args:
        data: List of numerical values
        
    Returns:
        dict: Statistics including mean, median, std deviation
    """
    import statistics
    
    return {
        "mean": statistics.mean(data),
        "median": statistics.median(data),
        "stdev": statistics.stdev(data) if len(data) > 1 else 0,
        "min": min(data),
        "max": max(data),
        "count": len(data)
    }
```

## Managing Dependencies

### requirements.txt

List all external dependencies:

```
# Core dependencies
yfinance==1.3.0
requests==2.31.0
pandas==2.1.0

# Optional dependencies
numpy==1.24.0
tabulate==0.10.0
```

### Best Practices

1. **Pin versions** - Use exact versions for reproducibility
2. **Minimal dependencies** - Only include what you need
3. **Test compatibility** - Ensure dependencies work together
4. **Document requirements** - Add comments for clarity

## Tool Management

### List All Tools

```bash
uv run orchestrate tools list
```

### Update a Tool

Modify the Python file and re-import:

```bash
uv run orchestrate tools import -k python -f "stockprice.py" -r "requirements.txt"
```

### Delete a Tool

```bash
uv run orchestrate tools delete -n tool_name
```

### Export a Tool

```bash
uv run orchestrate tools export -n tool_name -o exported_tool.py
```

## Testing Your Tools

### Manual Testing

Test tools directly in Python before importing:

```python
# test_tool.py
from stockprice import stock_price

# Test the function
result = stock_price("AAPL")
print(f"Apple stock price: ${result:.2f}")
```

### Testing with Agents

1. Import the tool
2. Create or update an agent to use it
3. Test through the Chat UI with relevant queries
4. Monitor logs for errors: `uv run orchestrate server logs`

## Common Patterns

### API Integration Tool

```python
@tool(
    name='weather_lookup',
    description='gets current weather for a city',
    permission=ToolPermission.USER
)
def get_weather(city: str) -> dict:
    """Fetches weather data for a city."""
    import requests
    
    api_key = "your_api_key"
    url = f"https://api.weather.com/v1/current?city={city}&key={api_key}"
    
    response = requests.get(url)
    return response.json()
```

### Database Query Tool

```python
@tool(
    name='query_database',
    description='executes SQL query and returns results',
    permission=ToolPermission.ADMIN
)
def query_db(sql: str) -> list:
    """Executes a SQL query safely."""
    import sqlite3
    
    # Use parameterized queries in production!
    conn = sqlite3.connect('database.db')
    cursor = conn.cursor()
    cursor.execute(sql)
    results = cursor.fetchall()
    conn.close()
    
    return results
```

### File Processing Tool

```python
@tool(
    name='process_csv',
    description='reads and analyzes CSV file data',
    permission=ToolPermission.USER
)
def process_csv(filepath: str) -> dict:
    """Processes a CSV file and returns summary statistics."""
    import pandas as pd
    
    df = pd.read_csv(filepath)
    
    return {
        "rows": len(df),
        "columns": list(df.columns),
        "summary": df.describe().to_dict()
    }
```

## Best Practices

### Security

- **Validate inputs** - Check parameters before processing
- **Use permissions wisely** - Restrict sensitive operations to ADMIN
- **Sanitize data** - Clean user inputs to prevent injection attacks
- **Handle secrets** - Use environment variables for API keys

### Performance

- **Add timeouts** - Prevent tools from hanging
- **Cache results** - Store frequently accessed data
- **Limit data size** - Set reasonable limits on inputs/outputs
- **Async operations** - Use async for I/O-bound tasks when possible

### Error Handling

```python
@tool(name='safe_tool', description='tool with proper error handling', permission=ToolPermission.USER)
def safe_operation(param: str) -> dict:
    """Tool with comprehensive error handling."""
    try:
        # Main logic
        result = perform_operation(param)
        return {"success": True, "data": result}
    except ValueError as e:
        return {"success": False, "error": f"Invalid input: {str(e)}"}
    except Exception as e:
        return {"success": False, "error": f"Unexpected error: {str(e)}"}
```

### Documentation

- Write clear docstrings
- Include usage examples
- Document expected inputs/outputs
- Note any limitations or requirements

## Troubleshooting

### Import Errors

**Tool not appearing after import:**
- Check Python syntax errors
- Verify the @tool decorator is present
- Ensure requirements.txt includes all dependencies
- Review server logs: `uv run orchestrate server logs`

### Runtime Errors

**Tool fails when called by agent:**
- Test the function independently first
- Check for missing dependencies
- Verify input types match expectations
- Add error handling and logging

### Permission Issues

**Agent can't access tool:**
- Verify tool permission level
- Check agent has appropriate access
- Review user permissions in Orchestrate

## Next Steps

Now that you can create Python tools, explore:

- **[Integrations](integrations)** - Add Langflow workflows and third-party models
- **[Observability](observability)** - Monitor tool usage and performance
- **[Creating Agents](agents)** - Build agents that use your tools

---

## Quick Reference

```bash
# Tool management commands
uv run orchestrate tools list                                    # List all tools
uv run orchestrate tools import -k python -f tool.py -r req.txt  # Import tool
uv run orchestrate tools delete -n tool_name                     # Delete tool
uv run orchestrate tools export -n tool_name                     # Export tool
```

## Tool Template

```python
from ibm_watsonx_orchestrate.agent_builder.tools import tool, ToolPermission

@tool(
    name='your_tool_name',
    description='clear description of what this tool does',
    permission=ToolPermission.USER
)
def your_function(param1: str, param2: int) -> dict:
    """
    Detailed docstring explaining the function.
    
    Args:
        param1: Description of first parameter
        param2: Description of second parameter
        
    Returns:
        dict: Description of return value
    """
    # Your implementation here
    result = {"status": "success", "data": "your data"}
    return result