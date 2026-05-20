# Microsoft Agent Framework - NOTES

[Microsoft Agent Framework](https://learn.microsoft.com/en-us/agent-framework/overview/?pivots=programming-language-python)

Offers two main categories of capabilities: **Agents** and **Workflows**

```bash
pip install agent-framework
```

## Baseline Agent

An agent created using the `Agent` class and a selected chat inference provider (aka LLM) has these capabilities:
- function calling
- multi-turn conversations with chat history management
- custom service-provided tools (e.g. MCP, Code Execution)
- structured outputs
- streaming responses

Supported chat providers:
- foundry agent
- azure openai chat completion
- azure openai responses
- openai chat completion
- openai responses
- anthropic claude
- amazon bedrock
- github copilot
- ollama (openai-compatible)
- any other chatclient (... that implements the SupportsChatGetResponse protocol)

[Provider Comparison](https://learn.microsoft.com/en-us/agent-framework/agents/providers/?pivots=programming-language-python#provider-comparison)

All major providers support calling function tools.

These providers support MCP tools:
- Azure OpenAI
- OpenAI
- Microsoft Foundry
- Anthropic
- GitHub Copilot

All of those except GitHub Copilot can also use a Code Interpreter.

## Using Claude Agent SDK with Microsoft Agent Framework

[Build AI Agents with Claude Agent SDK and Microsoft Agent Framework](https://devblogs.microsoft.com/agent-framework/build-ai-agents-with-claude-agent-sdk-and-microsoft-agent-framework/)

```bash
pip install agent-framework-claude --pre
```

Create an agent using the `ClaudeAgent` class from `agent-framework-claude`


## Multi-Turn Conversations

No need to maintain a message history list.

```python
from agent_framework_claude import ClaudeAgent

async def main():
    async with ClaudeAgent(
        instructions="You are a helpful assistant. Keep your answers short.",
    ) as agent:
        thread = agent.get_new_thread()

        # First turn
        await agent.run("My name is Alice.", thread=thread)

        # Second turn - agent remembers the context
        response = await agent.run("What is my name?", thread=thread)
        print(response.text)  # Should mention "Alice"
```

## Connecting to MCP Servers

Register Tools in hosted MCP servers by calling `get_mcp_tool()` with the MCP Server name and URL.


## Built-in Tools

The Claude Agent SDK provides these built-in tools:
- Read
- Write
- Bash
- Glob
- (others?)

## Custom Tools

Define custom tools simply by passing the Python function in the list of tools when initialising the agent.

## What about Skills?

It doesn't look like the MAF gives access to Skills through a specific file tree structure in the way that Claude Code and Claude Cowork do.

See this blog post for an example of how to "build your own skills framework": https://devblogs.microsoft.com/foundry/dotnet-ai-skills-executor-azure-openai-mcp/

