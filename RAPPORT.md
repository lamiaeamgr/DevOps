# Rapport — Mini Projet DevOps (Backend)

## Contexte et objectifs

Ce dépôt met en place le **pipeline CI/CD du backend** du Mini Projet DevOps : automatisation des tests, analyse de qualité (SonarQube), construction d’artefacts et déploiement via Docker Compose, avec notifications Slack.

**Frontend et backend sont séparés** : chacun dispose de son propre Jenkinsfile, docker-compose et .gitignore. Ce repo = **backend uniquement**. Un template pour le frontend est fourni dans **`frontend-standalone/`**.

**Objectifs pédagogiques atteints :**

- Pipeline CI/CD automatisé (GitHub Actions + Jenkins) pour le backend.
- Intégration Docker, SonarQube, Slack.
- Webhooks GitHub ↔ Jenkins.
- Notifications Slack (canal backend).

---

## Application (Backend)

- **Backend** : Node.js (Express), API REST (CRUD items).
- **Base de données** : PostgreSQL 16.
- Conteneurisation : Docker ; orchestration : Docker Compose (backend + db).

---

## Technologies et outils (Backend)

| Outil | Usage |
|-------|--------|
| **GitHub** | Dépôt source, webhook. |
| **GitHub Actions** | CI : lint, tests, build image. |
| **Jenkins** | Pipeline (tests, SonarQube, build, déploiement, Slack). |
| **Docker / Docker Compose** | Backend + PostgreSQL. |
| **SonarQube** | Qualité et couverture du code. |
| **Slack** | Notifications du pipeline (backend). |

---

## Livrables (Backend)

| Livrable | Détail |
|----------|--------|
| **Repository GitHub** | Backend uniquement : `backend/`, `.github/workflows/`, `jenkins/`, `docker-compose.yml`, `Jenkinsfile`, `frontend-standalone/` (template). |
| **Jenkinsfile** | À la racine (backend). |
| **docker-compose.yml** | À la racine (backend + db). |
| **.gitignore** | À la racine (backend). |
| **Workflow GitHub Actions** | `.github/workflows/ci.yml` (backend). |
| **Documentation** | `DOCUMENTATION.md`, `README.md`, `jenkins/README-WEBHOOK.md`, `jenkins/README-SLACK.md`. |
| **Template frontend** | `frontend-standalone/` (Jenkinsfile, docker-compose, .gitignore, Dockerfile, nginx.conf, README). |

---

## Documentation technique

- **[DOCUMENTATION.md](DOCUMENTATION.md)** : architecture, pipeline, procédures (backend).

---

*Mini Projet DevOps — Pr. KOUISSI Mohamed*
