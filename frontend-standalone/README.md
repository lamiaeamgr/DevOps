# Template Frontend (repo séparé)

Ce dossier contient les fichiers à **copier à la racine de ton repo frontend** pour avoir son propre pipeline Jenkins, Docker Compose et .gitignore.

## Fichiers à copier dans ton repo frontend

- **Jenkinsfile** → à la racine du repo frontend  
- **docker-compose.yml** → à la racine  
- **.gitignore** → à la racine (fusionne avec l’existant si besoin)  
- **Dockerfile** → à la racine  
- **nginx.conf** → à la racine  

## Structure attendue du repo frontend

À la racine du repo frontend, tu dois avoir ton app React (ou Vue/Angular) avec au minimum :

- `package.json` / `package-lock.json`
- `src/` (et `public/` si React)
- Scripts : `lint`, `test`, `build`

Le **Jenkinsfile** suppose que les commandes `npm run lint`, `npm run test`, `npm run build` existent.

## Utilisation

1. Crée un nouveau dépôt GitHub pour le frontend.
2. Copie le code de l’app frontend (React, etc.) à la racine.
3. Copie les fichiers de ce dossier (`frontend-standalone/`) à la racine du repo frontend.
4. Configure Jenkins : nouveau job Pipeline, SCM = ce repo frontend, Jenkinsfile à la racine.
5. Configure Slack : variable d’environnement `SLACK_CHANNEL_FRONTEND` = `#devops-frontend` (ou ton canal).

## Variables

- **REACT_APP_API_URL** : URL de l’API backend (ex. `https://api.example.com` ou `/api` si proxy). À définir en build ou dans `docker-compose` via `args`.
