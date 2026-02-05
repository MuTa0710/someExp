# MCP技术
## 模型与外界数据交互的桥梁
MCP全称为（Model Context Protocol），即模型上下文协议，旨在定义模型与外界数据交互的一种规范。

官方文档的示例代码如下：

    """
    FastMCP quickstart example.

    Run from the repository root:
        uv run examples/snippets/servers/fastmcp_quickstart.py
    """

    from mcp.server.fastmcp import FastMCP

    # Create an MCP server
    mcp = FastMCP("Demo", json_response=True)


    # Add an addition tool
    @mcp.tool()
    def add(a: int, b: int) -> int:
        """Add two numbers"""
        return a + b


    # Add a dynamic greeting resource
    @mcp.resource("greeting://{name}")
    def get_greeting(name: str) -> str:
        """Get a personalized greeting"""
        return f"Hello, {name}!"


    # Add a prompt
    @mcp.prompt()
    def greet_user(name: str, style: str = "friendly") -> str:
        """Generate a greeting prompt"""
        styles = {
            "friendly": "Please write a warm, friendly greeting",
            "formal": "Please write a formal, professional greeting",
            "casual": "Please write a casual, relaxed greeting",
        }

        return f"{styles.get(style, styles['friendly'])} for someone named {name}."


    # Run with streamable HTTP transport
    if __name__ == "__main__":
        mcp.run(transport="streamable-http")


MCP其本质是一段可以运行的程序，作为大模型联系世界的工具。一个简单的工具调用流程为：

1. 用户向Agent发送需求
2. Agent得到用户的需求，获取可以用的MCPTools列表，然后一起发送给模型
3. 模型根据用户需求以及可用的工具决定使用哪一种工具，然后将使用哪一种工具以及如何使用工具发送给Agent
4. Agent查看模型返回的信息，询问用户是否使用工具（如果需要的话，比如一些网址访问的任务），执行使用工具的任务。