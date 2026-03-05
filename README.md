# Mini Projet DevOps — Backend

Ce dépôt contient **uniquement le backend** du Mini Projet DevOps : API REST (Node.js + Express) + PostgreSQL, avec pipeline CI/CD (GitHub Actions, Jenkins, SonarQube, Slack).

Chaque composant (backend / frontend) a son propre **Jenkinsfile**, **docker-compose.yml** et **.gitignore**. Le template pour le frontend est dans le dossier **`frontend-standalone/`** (à copier dans un repo frontend séparé).

## Démarrage rapide

```bash
git clone https://github.com/lamiaeamgr/MiniProjectDevOps.git
cd MiniProjectDevOps
docker compose up -d --build
```

- **API** : http://localhost:3001  
- **Health** : http://localhost:3001/health  

## Stack (ce repo)

| Composant | Technologie |
|-----------|-------------|
| Backend   | Node.js (Express) |
| Base de données | PostgreSQL 16 |
| CI        | GitHub Actions |
| CI/CD     | Jenkins |
| Qualité   | SonarQube |
| Notifications | Slack |

## Structure

- **`backend/`** : code de l’API (src, tests, Dockerfile, sonar-project.properties).
- **Racine** : `Jenkinsfile`, `docker-compose.yml`, `.gitignore` (backend).
- **`frontend-standalone/`** : template pour le repo frontend (Jenkinsfile, docker-compose, .gitignore, Dockerfile, nginx.conf à copier à la racine du repo frontend).

## Documentation

- **[DOCUMENTATION.md](DOCUMENTATION.md)** : architecture, pipeline, procédures.
- **[jenkins/README-WEBHOOK.md](jenkins/README-WEBHOOK.md)** : webhook GitHub ↔ Jenkins.
- **[jenkins/README-SLACK.md](jenkins/README-SLACK.md)** : configuration Slack (backend).
- **[frontend-standalone/README.md](frontend-standalone/README.md)** : utilisation du template frontend.

## Commandes utiles

```bash
# Backend : tests et lint
cd backend && npm ci && npm run lint && npm run test

# Arrêter les conteneurs
docker compose down
```

---

*Projet pédagogique — Pr. KOUISSI Mohamed*
