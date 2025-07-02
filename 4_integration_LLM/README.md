# Intégration d'OpenAI avec MCP

Cette section démontre comment intégrer le Model Context Protocol (MCP) avec l'API d'OpenAI pour créer un système où OpenAI peut accéder et utiliser des outils fournis par votre serveur MCP.

## Aperçu

Cet exemple montre comment :

1. Créer un serveur MCP qui expose un outil de base de connaissances
2. Connecter OpenAI à ce serveur MCP
3. Permettre à OpenAI d'utiliser dynamiquement les outils lors de la réponse aux requêtes des utilisateurs

## Méthodes de Connexion

Cet exemple utilise le **transport stdio** pour la communication entre le client et le serveur, ce qui signifie :

- Le client et le serveur s'exécutent dans le même processus
- Le client lance directement le serveur en tant que sous-processus
- Aucun processus serveur séparé n'est nécessaire

Si vous souhaitez diviser votre client et votre serveur en applications distinctes (par exemple, exécuter le serveur sur une machine différente), vous devrez utiliser le **transport SSE (Server-Sent Events)**. Pour plus de détails sur la configuration d'une connexion SSE, voir la section [Configuration Simple du Serveur](../3-simple-server-setup).

### Explication du Flux de Données

1. **Requête Utilisateur** : L'utilisateur envoie une requête au système (par exemple, "Quelle est la politique de vacances de notre entreprise ?")
2. **API OpenAI** : OpenAI reçoit la requête et les outils disponibles du serveur MCP
3. **Sélection d'Outil** : OpenAI décide quels outils utiliser en fonction de la requête
4. **Client MCP** : Le client reçoit la demande d'appel d'outil d'OpenAI et la transmet au serveur MCP
5. **Serveur MCP** : Le serveur exécute l'outil demandé (par exemple, récupérer des données de la base de connaissances)
6. **Flux de Réponse** : Le résultat de l'outil revient via le client MCP vers OpenAI
7. **Réponse Finale** : OpenAI génère une réponse finale incorporant les données de l'outil

## Comment OpenAI Exécute les Outils

Le mécanisme d'appel de fonction d'OpenAI fonctionne avec les outils MCP à travers ces étapes :

1. **Enregistrement d'Outil** : Le client MCP convertit les outils MCP au format de fonction d'OpenAI
2. **Choix d'Outil** : OpenAI décide quels outils utiliser en fonction de la requête de l'utilisateur
3. **Exécution d'Outil** : Le client MCP exécute les outils sélectionnés et retourne les résultats
4. **Intégration de Contexte** : OpenAI incorpore les résultats des outils dans sa réponse

## Le Rôle de MCP

MCP sert de pont standardisé entre les modèles d'IA et vos systèmes backend :

- **Standardisation** : MCP fournit une interface cohérente pour que les modèles d'IA interagissent avec les outils
- **Abstraction** : MCP abstrait la complexité de vos systèmes backend
- **Sécurité** : MCP vous permet de contrôler exactement quels outils et données sont exposés aux modèles d'IA
- **Flexibilité** : Vous pouvez changer votre implémentation backend sans modifier l'intégration de l'IA

## Détails d'Implémentation

### Serveur (`server.py`)

Le serveur MCP expose un outil `get_knowledge_base` qui récupère des paires de questions-réponses à partir d'un fichier JSON.

### Client (`client.py`)

Le client :

1. Se connecte au serveur MCP
2. Convertit les outils MCP au format de fonction d'OpenAI
3. Gère la communication entre OpenAI et le serveur MCP
4. Traite les résultats des outils et génère les réponses finales

### Base de Connaissances (`data/kb.json`)

Contient des paires de questions-réponses sur les politiques de l'entreprise qui peuvent être interrogées via le serveur MCP.

## Exécution de l'Exemple

1. Assurez-vous d'avoir les dépendances requises installées
2. Configurez votre clé API OpenAI dans le fichier `.env`
3. Exécutez le client : `python client.py`

Note : Avec le transport stdio utilisé dans cet exemple, vous n'avez pas besoin d'exécuter le serveur séparément car le client le démarrera automatiquement.
