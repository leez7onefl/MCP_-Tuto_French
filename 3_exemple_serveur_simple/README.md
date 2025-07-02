## Partie 3 : Configuration Simple d'un Serveur avec le SDK Python

### Construction de Votre Premier Serveur MCP

Créons un serveur de démonstration simple avec un outil :

```python
# server.py
from mcp.server.fastmcp import FastMCP

# Create an MCP server
mcp = FastMCP("DemoServer")

# Simple tool
@mcp.tool()
def say_hello(name: str) -> str:
    """Say hello to someone

    Args:
        name: The person's name to greet
    """
    return f"Hello, {name}! Nice to meet you."

# Run the server
if __name__ == "__main__":
    mcp.run()
```


### Exécution du Serveur

Il existe plusieurs façons d'exécuter votre serveur MCP :

##### 1. Mode Développement avec MCP Inspector

La manière la plus simple de tester votre serveur est d'utiliser MCP Inspector :


```bash
mcp dev server.py
```

Cela exécute votre serveur localement et le connecte à l'Inspecteur MCP, un outil basé sur le web qui vous permet d'interagir directement avec les outils et les ressources de votre serveur. C'est idéal pour les tests.

#### 2. Intégration avec Claude Desktop
Si vous avez Claude Desktop installé, vous pouvez installer votre serveur pour l'utiliser avec Claude :

```bash
mcp install server.py
```

Cela ajoutera votre serveur à la configuration de Claude Desktop, le rendant disponible pour Claude.

#### 3. Exécution directe (nécessaire uniquement pour SSE)
Vous pouvez également exécuter le serveur directement :

```bash
# Method 1: Running as a Python script
python server.py

# Method 2: Using UV (recommended)
uv run server.py
```

### Que se passe-t-il lorsque vous exécutez un serveur MCP ?

Lorsque vous exécutez un serveur MCP :

1. Le serveur s'initialise avec les capacités que vous avez définies (outils, ressources, etc.)
2. Il commence à écouter les connexions sur un transport spécifique

Par défaut, les serveurs MCP n'utilisent pas un port de serveur web traditionnel. Au lieu de cela, ils utilisent soit :

- **transport stdio** : Le serveur communique via l'entrée et la sortie standard (par défaut pour `mcp run` et l'intégration avec Claude Desktop)
- **transport SSE** : Pour la communication basée sur HTTP (utilisé lorsqu'il est explicitement configuré)

Si vous souhaitez exposer votre serveur via HTTP avec un port spécifique, vous devez modifier votre serveur pour utiliser le transport SSE.

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("MyServer", host="127.0.0.1", port=8050)

# Add your tools and resources here...

if __name__ == "__main__":
    # Run with SSE transport on port 8000
    mcp.run(transport="sse")
```

avec ensuite :

```bash
python server.py
```

Cela démarrera votre serveur à l'adresse `http://127.0.0.1:8050`.

### Implémentation Côté Client (avec I/O Standard)

Maintenant, voyons comment créer un client qui utilise notre serveur :

```python
import asyncio
import nest_asyncio
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def main():
    # Define server parameters
    server_params = StdioServerParameters(
        command="python",  # The command to run your server
        args=["server.py"],  # Arguments to the command
    )

    # Connect to the server
    async with stdio_client(server_params) as (read_stream, write_stream):
        async with ClientSession(read_stream, write_stream) as session:
            # Initialize the connection
            await session.initialize()

            # List available tools
            tools_result = await session.list_tools()
            print("Available tools:")
            for tool in tools_result.tools:
                print(f"  - {tool.name}: {tool.description}")

            # Call our calculator tool
            result = await session.call_tool("add", arguments={"a": 2, "b": 3})
            print(f"2 + 3 = {result.content[0].text}")


if __name__ == "__main__":
    asyncio.run(main())
```

Ce client :
1. Crée une connexion à notre serveur via stdio
2. Établit une session MCP
3. Liste les outils disponibles
4. Appelle l'outil `add` avec des arguments

### Implémentation côté client (avec Server-Sent Events)

Voici comment se connecter à votre serveur avec SSE :


```python
import asyncio
import nest_asyncio
from mcp import ClientSession
from mcp.client.sse import sse_client

async def main():
    # Connect to the server using SSE
    async with sse_client("http://localhost:8050/sse") as (read_stream, write_stream):
        async with ClientSession(read_stream, write_stream) as session:
            # Initialize the connection
            await session.initialize()

            # List available tools
            tools_result = await session.list_tools()
            print("Available tools:")
            for tool in tools_result.tools:
                print(f"  - {tool.name}: {tool.description}")

            # Call our calculator tool
            result = await session.call_tool("add", arguments={"a": 2, "b": 3})
            print(f"2 + 3 = {result.content[0].text}")


if __name__ == "__main__":
    asyncio.run(main())
```

### Quelle Approche Devez-Vous Choisir ?
- **Utilisez stdio** si votre client et votre serveur vont s'exécuter dans le même processus ou si vous démarrez le processus serveur directement depuis votre client.
- **Utilisez HTTP** si votre serveur va s'exécuter séparément de votre client, éventuellement sur des machines différentes ou dans des conteneurs différents.

Pour la plupart des intégrations backend de production, l'approche HTTP offre une meilleure séparation et une meilleure évolutivité, tandis que l'approche stdio peut être plus simple pour le développement ou les systèmes étroitement couplés.
