# Cours accéléré sur le MCP pour les développeurs Python

Le Model Context Protocol (MCP) est un cadre normalisé qui permet aux développeurs de créer des applications d'IA avec des LLMs en fournissant une manière standardisée de connecter les modèles avec des sources de données externes et des outils. Ce contenu à pour but d'enseigner les fondamentaux du MCP, de la compréhension de ses concepts de base à la mise en œuvre de serveurs et de clients.

## Table des matières
1. [Introduction et contexte](./1_Introduction_et_contexte/README.md)
2. [Comprendre le MCP](./2_comprendre_le_protocole_MCP/README.md)
3. [Configuration simple du serveur avec le SDK Python](./3_exemple_serveur_simple/README.md)
4. [Intégration d'un LLM](./4_integration_LLM/README.md)
5. [MCP vs Appel de fonction](./5_MCP_vs_fonctions/README.md)
6. [Exécution avec Docker](./6_intégration_avec_docker/README.md)
7. [Gestion du cycle de vie](./7_cycle_de_vie_MCP/README.md)

## Configuration de votre environnement de développement

Le [SDK Python MCP](https://github.com/modelcontextprotocol/python-sdk) fournit de quoi construire à la fois serveurs et clients.

```bash
pip install -r requirements.txt
```
 
Les outils CLI MCP fournissent des utilitaires utiles pour le développement et les tests :

```bash
# Tester un serveur avec l'Inspecteur MCP
mcp dev server.py
# Exécuter un serveur directement
mcp run server.py
```

Ressources utiles :

- [Model Context Protocol documentation](https://modelcontextprotocol.io)
- [Model Context Protocol specification](https://spec.modelcontextprotocol.io)
- [Python SDK GitHub repository](https://github.com/modelcontextprotocol/python-sdk)
- [Officially supported servers](https://github.com/modelcontextprotocol/servers)
- [MCP Core Architecture](https://modelcontextprotocol.io/docs/concepts/architecture)