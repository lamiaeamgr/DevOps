# Configuration Webhook GitHub → Jenkins

## 1. Plugin Jenkins

- Installer le plugin **GitHub Plugin** (et **Git Plugin** si besoin).
- Dans **Manage Jenkins** → **Plugins** : chercher "GitHub", installer "GitHub Plugin".

## 2. Créer un Pipeline

- **New Item** → **Pipeline** → nom : `miniprojet-devops`.
- **Pipeline** → Definition : **Pipeline script from SCM**.
- SCM : **Git**.
- Repository URL : `https://github.com/lamiaeamgr/DevOps.git` (ou votre dépôt).
- Credentials : ajouter un **Personal Access Token** GitHub (scope `repo`).
- Branch : `*/main` (ou `*/master`).
- **Build Triggers** : cocher **GitHub hook trigger for GITScm polling**.

## 3. Webhook sur GitHub

- Sur le dépôt GitHub : **Settings** → **Webhooks** → **Add webhook**.
- **Payload URL** : `https://testy-brandy-parenterally.ngrok-free.dev/github-webhook/`
- **Content type** : `application/json`.
- **Events** : "Just the push event" (ou "Let me select" → Push events).
- **Add webhook**.

À chaque push sur la branche configurée, Jenkins déclenche le pipeline automatiquement.
