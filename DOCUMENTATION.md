# Mini Projet DevOps — Documentation technique (Backend)

Ce dépôt est dédié au **backend** uniquement. Frontend et backend ont chacun leur propre Jenkinsfile, docker-compose et .gitignore (template frontend dans `frontend-standalone/`).

## 1. Architecture globale (Backend)

| Composant   | Technologie        | Rôle                  | Port |
|------------|--------------------|------------------------|------|
| **Backend** | Node.js (Express)  | API REST (CRUD items)  | 3001 |
| **Base de données** | PostgreSQL 16 | Persistance            | 5432 (interne) |

### Schéma

```
  Client (HTTP)  →  Backend (Express)  →  PostgreSQL
       │                    │                     │
       └── :3001            └── DB_HOST=db       └── volume postgres_data
```

- Orchestration : **Docker Compose** (`docker-compose.yml` à la racine).
- Conteneurs : `db`, `backend`.

---

## 2. Schéma du pipeline CI/CD (Backend)

```
  GitHub (code)     →     GitHub Actions (CI)     →     Jenkins (CI/CD)     →     Déploiement
       │                           │                           │
       │  push/PR                  │  Lint + Tests backend      │  Checkout
       ├──────────────────────────►│  Build image Docker        │  Backend: lint, test
       │                           │                           │  SonarQube, Quality Gate
       │  Webhook                  │                           │  Build Docker Compose
       ├──────────────────────────────────────────────────────►│  Deploy (main)
       │                                                       │  Slack (succès/échec)
```

---

## 3. Rôle des outils (Backend)

| Outil | Rôle |
|-------|------|
| **GitHub** | Code source, webhook vers Jenkins. |
| **GitHub Actions** | CI : lint, tests, build image backend. |
| **Jenkins** | Pipeline : tests, SonarQube, Quality Gate, build, déploiement, Slack. |
| **Docker / Docker Compose** | Conteneurisation backend + PostgreSQL. |
| **SonarQube** | Qualité et couverture du code backend. |
| **Slack** | Notifications du pipeline (canal backend). |

---

## 4. Structure du dépôt (Backend)

```
MiniProjectDevOps/
├── .github/workflows/ci.yml   # CI backend
├── backend/
│   ├── src/
│   ├── Dockerfile
│   ├── package.json
│   ├── sonar-project.properties
│   └── ...
├── frontend-standalone/       # Template pour repo frontend (Jenkinsfile, docker-compose, .gitignore, etc.)
├── jenkins/
│   ├── README-WEBHOOK.md
│   └── README-SLACK.md
├── docker-compose.yml        # Backend + db uniquement
├── Jenkinsfile               # Pipeline backend
├── .gitignore
├── DOCUMENTATION.md
└── README.md
```

---

## 5. Procédures d’exécution

### 5.1 Prérequis

- Docker et Docker Compose.
- En local : Node.js 20, npm (pour tests dans `backend/`).

### 5.2 Lancer l’application (Backend)

```bash
git clone https://github.com/VOTRE_ORG/MiniProjectDevOps.git
cd MiniProjectDevOps
docker compose up -d --build
```

- API : http://localhost:3001  
- Health : http://localhost:3001/health  

### 5.3 Tests et lint en local

```bash
cd backend
npm ci
npm run lint
npm run test
```

### 5.4 GitHub Actions

- Déclenchement : push/PR sur `main` ou `develop`.
- Workflow : `.github/workflows/ci.yml` (lint, tests, build image backend).

### 5.5 Jenkins

- Job Pipeline, SCM = ce dépôt, Jenkinsfile à la racine.
- Webhook GitHub → Jenkins : voir `jenkins/README-WEBHOOK.md`.
- Slack : voir `jenkins/README-SLACK.md` (canal backend).

### 5.6 Frontend (repo séparé)

- Template : copier le contenu de **`frontend-standalone/`** à la racine du repo frontend.
- Voir **`frontend-standalone/README.md`**.

---

## 6. Livrables (Backend)

| Livrable | Emplacement |
|----------|-------------|
| Repo GitHub (backend) | Racine du projet |
| Jenkinsfile | Racine |
| docker-compose.yml | Racine (backend + db) |
| Workflow GitHub Actions | `.github/workflows/ci.yml` |
| Documentation | `DOCUMENTATION.md`, `README.md`, `jenkins/*.md` |
| Template frontend | `frontend-standalone/` |

---

*Mini Projet DevOps — Pr. KOUISSI Mohamed*
