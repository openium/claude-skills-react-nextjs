---
name: onboard
description: "Produit un guide d'onboarding technique à partir d'un projet React/Next.js existant. Analyse architecture, versions, installation, commandes, routing, composants, données, authentification, tests, conventions, CI/CD, observabilité et pièges connus sans lire les fichiers d'environnement sensibles."
---

# Onboarding Technique React / Next.js

## Périmètre

Par défaut, produire un guide pour le projet entier. Si l'utilisateur précise un
module, dossier ou objectif, limiter l'onboarding à ce périmètre.

Ne pas modifier le repository, sauf demande explicite.

## Sources À Inspecter

Lire selon leur présence :

- `README*`, `docs/`
- `package.json`, lockfile
- `.nvmrc`, `.node-version`, `Dockerfile`, `docker-compose*`
- `next.config.*`, `tsconfig.json`, lint, Prettier
- `app/`, `pages/`, `src/`, `components/`, `lib/`, `services/`, `hooks/`
- `public/`, styles, design system
- tests, Storybook, Playwright/Cypress
- `.github/workflows/`, `.gitlab-ci.yml`, config déploiement
- `.env.example` ou documentation d'environnement si non sensible

Ne jamais lire ni recopier `.env` ou fichiers d'environnement sensibles.

## Règles

- Documenter seulement ce qui est vérifiable dans le repo.
- Distinguer faits vérifiés et points à confirmer.
- Citer les fichiers sources consultés quand utile.
- Ne pas inventer de commandes : utiliser scripts `package.json`, Makefile ou CI existants.
- Si README, Docker, scripts et CI se contredisent, signaler l'écart.
- Documenter les variables d'environnement par nom et rôle seulement si source non sensible.
- Si un secret semble committé, le signaler sans le recopier.

## Sections Du Guide

### Vue D'Ensemble

Objectif du projet, type d'application, domaines principaux, services externes.

### Stack Et Versions

Node.js, package manager, React, Next.js, TypeScript, styles, tests, outils.

### Installation Et Commandes

Prérequis, installation, démarrage local, build, tests, lint, storybook si présent.

### Architecture

Structure des dossiers, conventions, séparation UI/data/domain, providers.

### Routing

App Router, Pages Router ou hybride, routes principales, API routes, middleware.

### Composants Et Design System

Organisation des composants, primitives UI, styles, responsive, accessibilité.

### Données Et Authentification

Data fetching, cache, mutations, auth/session, clients API, données sensibles.

### Tests Et Qualité

Jest/Vitest, Testing Library, E2E, lint, typecheck, conventions de test.

### CI/CD Et Déploiement

Pipelines, variables, build, preview, production, rollback si vérifiable.

### Observabilité

Logs, Sentry/APM, analytics, source maps, alertes si présents.

### Pièges Connus

TODO/FIXME, documentation contradictoire, zones legacy, limites de tests.

## Format De Sortie

Produire un document Markdown factuel et concis avec :

- commandes utiles en blocs `sh` ;
- références aux fichiers consultés quand pertinent ;
- section **À confirmer** ;
- section **Écarts ou manques de documentation** si nécessaire.

Ne pas inclure de secrets ni de contenu de fichiers d'environnement sensibles.
