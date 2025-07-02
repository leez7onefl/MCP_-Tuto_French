# Serveur MCP avec Docker

Ce projet démontre comment exécuter un serveur MCP (Model Control Protocol) en utilisant Docker. Le serveur fournit un outil de calculatrice simple qui peut être accessible par un client.

## Prérequis

- Docker installé sur votre système
- Git (pour cloner le dépôt)

## Structure du projet

- `server.py` : L'implémentation du serveur MCP avec un outil de calculatrice simple
- `client.py` : Un client qui se connecte au serveur et appelle l'outil de calculatrice
- `Dockerfile` : Instructions pour construire l'image Docker
- `requirements.txt` : Dépendances Python pour le projet

## Exécution avec Docker

### Étape 1 : Construire l'image Docker

```bash
docker build -t mcp-server .
```

### Étape 2 : run le conteneur Docker

```bash
docker run -p 8050:8050 mcp-server
```

Cela démarrera le serveur MCP à l'intérieur d'un conteneur Docker et l'exposera sur le port 8050.

## Exécution du Client

Une fois le serveur en cours d'exécution, vous pouvez exécuter le client dans un terminal séparé :

```bash
python client.py
```

Le client se connectera au serveur, listera les outils disponibles et appellera l'outil calculatrice pour additionner 2 et 3.

## Dépannage

Si vous rencontrez des problèmes de connexion :

1. **Vérifiez si le serveur est en cours d'exécution** : Assurez-vous que le conteneur Docker est en cours d'exécution avec `docker ps`.
2. **Vérifiez le mappage de port** : Assurez-vous que le port est correctement mappé avec `docker ps` ou en vérifiant la sortie de la commande `docker run`.
3. **Consultez les logs du serveur** : Voir les logs du serveur avec `docker logs <container_id>` pour voir s'il y a des erreurs.
4. **Liaison d'hôte** : Le serveur est configuré pour se lier à `0.0.0.0` au lieu de `127.0.0.1` pour le rendre accessible depuis l'extérieur du conteneur. Si vous avez toujours des problèmes, vous devrez peut-être vérifier les paramètres de votre pare-feu.
5. **Problèmes de réseau** : Si vous exécutez Docker sur une machine distante, assurez-vous que le port est accessible depuis votre machine cliente.

## Notes

- Le serveur est configuré pour utiliser le transport SSE (Server-Sent Events) et écoute sur le port 8050.
- Le client se connecte au serveur à l'adresse `http://localhost:8050/sse`.
- Assurez-vous que le serveur est en cours d'exécution avant de démarrer le client.