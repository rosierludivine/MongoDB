# Docker et mongoDB

## Installation de Docker

Pour telecharger l'image Docker mongoDB, il suffit de lancer la commande suivante:

```bash
docker pull mongo
```

Pour lancer un container mongoDB, il suffit de lancer la commande suivante:

```bash
docker run -p 27017:27017 -d --name mongoRPI mongo
```

Pour lister les containers en cours d'execution, il suffit de lancer la commande suivante:

```bash
docker ps


// exemple de resultat
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
f3e3e3e3e3e3        mongo               "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes
```

Pour le stopper le conteneur ci-dessus il suffit de lancer la commande suivante:

```bash
docker stop f3e3e3e3e3e3
```

A partir du fichier docker compose fournit, lancer la commande suivante pour lancer le container mongoDB:

```bash
docker-compose up -d
```
