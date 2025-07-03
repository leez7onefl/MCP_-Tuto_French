# MCP-RAG : Model Context Protocol avec RAG
Une implémentation puissante et efficace de RAG (Retrieval-Augmented Generation) utilisant GroundX et OpenAI, construite avec le Modern Context Processing (MCP).

## Fonctionnalités
- **Implémentation avancée de RAG** : Utilise GroundX pour une récupération de documents de haute précision
- **Model Context Protocol** : Intégration transparente avec MCP pour une gestion améliorée du contexte
- **Sécurité des types** : Construit avec Pydantic pour une vérification et une validation robustes des types
- **Configuration flexible** : Paramètres faciles à personnaliser via des variables d'environnement
- **Ingestion de documents** : Support pour l'ingestion et le traitement de documents PDF
- **Recherche intelligente** : Capacités de recherche sémantique avec notation

## Prérequis
- Python 3.12 ou supérieur
- Clé API OpenAI
- Clé API GroundX
- Outils CLI MCP

## Installation

1. Configurer les variables d'environnement :

```env
GROUNDX_API_KEY="your-groundx-api-key"
OPENAI_API_KEY="your-openai-api-key"
BUCKET_ID="your-bucket-id"
```

## Démarrage

1. Démarrer le serveur de developpement :
```bash
mcp dev server.py
```

2. Démarrer le serveur : 

```bash
mcp server.py
```

## Charger les documents

```python
from server import ingest_documents

result = ingest_documents("path/to/your/document.pdf")
print(result)
```

## Effectuer une recherche

1. Requête basique : 

```python
from server import process_search_query

response = process_search_query("your search query here")
print(f"Query: {response.query}")
print(f"Score: {response.score}")
print(f"Result: {response.result}")
```

2. Requête avec configuration spécifiques :

```python
from server import process_search_query, SearchConfig

config = SearchConfig(
    completion_model="gpt-4",
    bucket_id="custom-bucket-id"
)
response = process_search_query("your query", config)
```

## Librairies
- `groundx` (≥2.3.0) : Fonctionnalité principale RAG
- `openai` (≥1.75.0) : Intégration de l'API OpenAI
- `mcp[cli]` (≥1.6.0) : Outils de traitement de contexte modernes
- `ipykernel` (≥6.29.5) : Support des notebooks Jupyter