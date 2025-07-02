## Partie 1 : Introduction & Contexte

### Ce qu'on dit du MCP vs. la Réalité

Le Model Context Protocol (MCP) a suscité beaucoup d'engouement dans la communauté de l'IA récemment, et pour de bonnes raisons. Il représente une manière standardisée pour les LLMs d'interagir avec des outils et services externes. Malgré cet enthousiasme, il est important de voir au-delà et de comprendre ce qu'est vraiment MCP.

Le MCP n'est pas une nouvelle technologie - c'est un standard (potentiellement) révolutionnaire. Si vous avez travaillé avec des systèmes ou agents d'IA pendant un certain temps, vous avez déjà mis en œuvre le concept de base : donner aux LLMs accès à des outils via des appels de fonction. Ce qui est différent, c'est que MCP fournit un protocole standardisé pour ces interactions.


Il existe une distinction claire entre :

1. **L'utilisation personnelle de MCP** - Intégration de serveurs avec Claude Desktop, Cursor, ou d'autres assistants IA personnels
2. **L'intégration backend** - Intégration de MCP dans vos applications Python et systèmes d'agents


Ce repository à pour but de permettre de :

- Comprendre l'architecture technique de MCP
- Construire des serveurs MCP personnalisés en utilisant le SDK Python
- Intégrer ces serveurs dans des applications Python