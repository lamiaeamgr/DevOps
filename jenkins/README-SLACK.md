# Configuration Slack pour Jenkins (Backend)

Ce guide décrit la configuration des notifications Slack pour le **pipeline backend** (canal dédié backend).

Pour le **frontend**, utiliser le template dans **`frontend-standalone/`** (repo frontend séparé avec son propre Jenkinsfile et variable `SLACK_CHANNEL_FRONTEND`).

---

## Option 1 : Slack App avec Bot Token (recommandé)

### 1. Créer une app Slack

1. **https://api.slack.com/apps** → **Create New App** → **From scratch**.
2. Nom (ex. `Jenkins DevOps`), choisir le **Workspace**.
3. **OAuth & Permissions** → **Bot Token Scopes** : ajouter `chat:write` (et si besoin `channels:read`, `groups:read`).
4. **Install to Workspace** → copier le **Bot User OAuth Token** (`xoxb-...`).

### 2. Plugin Jenkins

1. **Manage Jenkins** → **Plugins** → **Available plugins**.
2. Rechercher **Slack Notification** → installer.

### 3. Configurer Jenkins (Backend)

1. **Manage Jenkins** → **Credentials** → **Add** → **Secret text**  
   - Secret : token `xoxb-...`  
   - ID : `slack-bot-token`
2. **Manage Jenkins** → **Configure System** → section **Slack**  
   - **Credential** : `slack-bot-token`  
   - **Default channel** : `#devops-backend` (ou ton canal backend)  
   - Cocher **Custom slack app bot user** si proposé  
   - **Save**

### 4. Canal backend dans le job

Le **Jenkinsfile** (backend) utilise **`SLACK_CHANNEL_BACKEND`** (défaut : `#devops-backend`).

- Laisser le défaut, ou
- Dans le job : **Configure** → **Pipeline** → **Environment** → variable **`SLACK_CHANNEL_BACKEND`** = `#ton-canal-backend`.

---

## Option 2 : Incoming Webhook

1. **https://api.slack.com/apps** → **Create New App** → **From scratch**.
2. **Incoming Webhooks** → **On** → **Add New Webhook** → choisir le canal (ex. `#devops-backend`) → copier l’URL.
3. Dans Jenkins : credential **Secret text** avec cette URL, puis dans **Configure System** → **Slack** l’associer si le plugin le permet.

---

## Vérification

Lancer un build du pipeline backend : les messages (succès / échec / instable) doivent apparaître dans le canal Slack configuré.

---

## Dépannage

| Problème | Piste |
|----------|--------|
| Aucun message | Vérifier token, credential, et invitation du bot dans le canal (`/invite @NomDuBot`). |
| "channel_not_found" / "not_in_channel" | Inviter le bot dans le canal : `/invite @NomDuBot`. |

---

*Référence : [Slack Notification Plugin](https://plugins.jenkins.io/slack/)*
