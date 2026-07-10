---
name: plan
description: "Planifie l'implémentation d'une feature, bugfix, refactor, migration frontend ou upgrade dans un projet React, TypeScript et Next.js. Analyse la stack, le router, les composants, routes, hooks, services, tests, impacts cache/rendu/hydratation/accessibilité/bundle, puis produit un plan ordonné sans modifier le code."
---

# Planification d'implémentation React / Next.js

## Périmètre

Déterminer si l'utilisateur demande un plan pour :

- Feature UI ou parcours utilisateur
- Bugfix React, TypeScript, Next.js, rendu ou hydratation
- Refactor ciblé de composants, hooks, services ou routes
- Migration frontend limitée
- Upgrade React, Next.js, TypeScript, tooling ou dépendance
- Correction performance, accessibilité, bundle ou data fetching
- Ajout ou évolution de tests unitaires, composants ou E2E

Si l'utilisateur demande seulement un plan, ne jamais modifier le code.

Si le périmètre est ambigu, poser uniquement les questions bloquantes. Pour les
points non bloquants, formuler des hypothèses explicites.

## Objectif

Produire un plan exécutable, ordonné et adapté au projet.

Le plan doit :

- Respecter les conventions et versions réellement détectées.
- Préserver l'architecture App Router, Pages Router ou hybride existante.
- Séparer composants, routes, hooks, services, styles, tests et configuration.
- Identifier les impacts sur cache, rendu, hydratation, accessibilité et bundle.
- Garder le diff futur minimal.
- Éviter migrations, dépendances et refactors non demandés.
- Donner les validations utiles sans lancer d'action destructive.

## État Des Lieux À Inspecter

Inspecter selon le sujet :

- `package.json` : scripts, dépendances React/Next.js/TypeScript, tooling.
- Lockfile : `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock` ou `bun.lockb`.
- TypeScript : `tsconfig.json` et configurations étendues.
- Next.js : `next.config.js`, `next.config.mjs`, `next.config.ts`, middleware.
- Lint/format : `eslint.config.*`, `.eslintrc*`, Prettier, Stylelint.
- Tests : Vitest, Jest, Testing Library, Storybook, Playwright, Cypress.
- Router : présence de `app/`, `pages/`, `src/app/`, `src/pages/`.
- Routes : layouts, pages, route handlers, API routes, middleware, dynamic routes.
- UI : composants, styles, design system, accessibilité existante.
- Logique : hooks, services, clients API, stores, providers, validators.
- Data fetching : `fetch`, Server Components, actions serveur, SWR, TanStack Query, GraphQL.
- Observabilité : Sentry, Datadog, OpenTelemetry, analytics, logs plateforme.

Ne pas lire ni afficher `.env` ou les fichiers d'environnement sensibles.

## Analyse Next.js

Détecter et expliciter :

- App Router : `app/`, layouts, Server Components, Client Components, route handlers.
- Pages Router : `pages/`, `getServerSideProps`, `getStaticProps`, API routes.
- Architecture hybride : coexistence `app/` et `pages/`, responsabilités de chaque zone.
- Frontières serveur/client : composants qui nécessitent `"use client"` et ceux qui doivent rester serveur.
- Stratégie de rendu : static, dynamic, SSR, ISR, streaming, client-side rendering.
- Cache : `fetch` cache, `revalidate`, tags, SWR/TanStack Query, invalidation.
- Hydratation : sources possibles de mismatch, dépendance au navigateur, dates, random, locale.
- Bundle : imports côté client, librairies lourdes, découpage, dynamic imports.

Ne pas migrer automatiquement Pages Router vers App Router. Ne pas ajouter
`"use client"` à un arbre complet par facilité.

## Questions Ouvertes

Lister uniquement :

- Les décisions produit nécessaires pour éviter une mauvaise implémentation.
- Les choix techniques qui changent fortement l'architecture, le coût ou le risque.
- Les informations sans lesquelles le plan serait trompeur.

Pour le reste, indiquer les hypothèses :

- Router supposé si plusieurs architectures coexistent.
- Gestionnaire de paquets retenu selon le lockfile.
- Stratégie de rendu et cache supposée.
- Niveau de compatibilité navigateur ou accessibilité attendu.
- Couverture de tests réaliste.

## Découpage Des Étapes

Ordonner les étapes pour réduire les risques :

1. Lire les configs et détecter stack, router, scripts et conventions.
2. Identifier composants, routes, hooks, services, styles et tests concernés.
3. Définir les changements de données, état, rendu et cache.
4. Définir les changements UI, accessibilité et design system.
5. Définir les impacts Server Components / Client Components.
6. Prévoir les tests unitaires, composants et E2E utiles.
7. Prévoir les validations de typecheck, lint, test et build.
8. Lister les risques, mitigations et décisions à confirmer.

## Risques À Analyser

- **Rendu** : SSR, SSG, ISR, streaming, dynamic rendering non anticipé.
- **Hydratation** : mismatch serveur/client, usage navigateur côté serveur.
- **Cache** : données obsolètes, invalidation manquante, revalidation trop large.
- **Bundle** : dépendance lourde côté client, mauvais import, perte de code splitting.
- **Accessibilité** : labels, focus, clavier, contrastes, textes alternatifs.
- **TypeScript** : contrats faibles, `any`, types publics cassés.
- **Routing** : dynamic routes, query params, redirects, middleware, API routes.
- **État** : stores globaux, providers, synchronisation URL/cache/UI.
- **Tests** : comportement critique non couvert, mocks fragiles, E2E trop large.
- **Upgrade** : breaking changes React/Next.js/tooling, plugins incompatibles.

## Stratégie De Tests

Prévoir selon le changement :

- Unitaires : fonctions pures, services, hooks isolables, validators.
- Composants : rendu, interactions, états loading/error/empty, accessibilité.
- Next.js : routes, loaders, server actions, route handlers, metadata si pertinent.
- E2E : parcours critique, navigation, auth, formulaires, régression bugfix.
- Régression : test qui reproduit le bug avant correction si possible.

Ne pas supprimer, désactiver ou assouplir un test pour masquer une régression.

## Validation Technique

Proposer uniquement les commandes adaptées au projet et au lockfile :

- `<package-manager> run typecheck`
- `<package-manager> run lint`
- `<package-manager> run test`
- `<package-manager> run test -- <filtre>`
- `<package-manager> run test:e2e`
- `<package-manager> run build`
- Commande Storybook ou accessibility si présente

Ne pas proposer de migration, upgrade de dépendance ou commande destructive non
demandée.

## Règles

- Ne jamais modifier le code quand l'utilisateur demande seulement un plan.
- Ne pas ajouter de dépendance sans justification explicite et validation.
- Ne pas inclure de refactor large sauf demande explicite.
- Ne pas changer de router Next.js sans demande explicite.
- Ne pas contourner TypeScript, ESLint, tests ou accessibilité.
- Respecter le gestionnaire de paquets imposé par le lockfile.
- Signaler les opportunités de refactor hors périmètre sans les intégrer au plan.
- Ne pas masquer une incertitude importante : la lister comme décision à valider.

## Format De Sortie

Présenter le plan avec :

1. **Résumé** : besoin reformulé en une phrase.
2. **Hypothèses** : choix supposés si non confirmés.
3. **Contexte détecté** : stack, router, conventions, fichiers et contraintes observés.
4. **Étapes** : ordre, fichier ou zone, changement prévu, raison, dépendances.
5. **Fichiers** : fichiers à créer ou modifier, responsabilité et risque.
6. **Tests** : tests à écrire ou adapter, mocks/fixtures nécessaires.
7. **Validations** : commandes à lancer et objectif de chaque commande.
8. **Risques** : impacts possibles et mitigations.
9. **Complexité** : simple, modéré ou complexe, avec justification courte.
10. **Décisions à valider** : uniquement les points nécessaires avant implémentation.
