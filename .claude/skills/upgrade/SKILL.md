---
name: upgrade
description: "Planifie ou accompagne les upgrades Node.js, React, Next.js, TypeScript et dépendances frontend. Analyse versions, release notes officielles, breaking changes, peer dependencies, codemods, lockfile, tests, build, rollback, App Router et Pages Router. Ne lance pas l'upgrade sans validation explicite du plan."
---

# Upgrade React / Next.js

## Périmètre

Déterminer si l'utilisateur demande :

- Diagnostic ou plan d'upgrade : ne pas modifier le code.
- Application d'un upgrade validé : procéder par étapes vérifiables.
- Cible précise : Node.js, React, Next.js, TypeScript, dépendance ou tooling.

Si la cible n'est pas donnée, analyser l'état actuel et demander la cible avant
de modifier.

Ne pas lancer l'upgrade sans validation explicite du plan.

## État Des Lieux

Inspecter :

- `package.json`, lockfile, package manager.
- Versions Node.js : `.nvmrc`, `.node-version`, Dockerfile, CI, `engines`.
- React, Next.js, TypeScript, ESLint, test runner, bundler.
- App Router, Pages Router ou hybride.
- `next.config.*`, `tsconfig.json`, lint, tests, CI.
- Dépendances avec peer dependencies sensibles.

## Sources

- Lire release notes et guides officiels pour la cible.
- Vérifier breaking changes et migrations documentées.
- Vérifier codemods officiels si disponibles.
- Utiliser sources primaires quand une information peut avoir changé.

## Stratégie

- Préférer upgrade incrémental.
- Séparer Node.js, Next.js, React, TypeScript et dépendances risquées si possible.
- Respecter le lockfile et le package manager existants.
- Ne pas modifier le lockfile à la main.
- Prévoir stratégie de rollback.
- Tester App Router et Pages Router selon l'architecture réelle.

## Points D'Attention

- Breaking changes Next.js : config, routing, cache, image, middleware, server actions.
- React : rendu, strict mode, hooks, hydratation, types.
- TypeScript : options plus strictes, types React/Node.
- Peer dependencies : eslint, testing-library, storybook, bundler.
- Codemods : les lancer en dry-run si possible, relire le diff.
- CI/CD : version Node, cache dependencies, build command.

## Commandes Utiles

Adapter au projet :

- `<package-manager> outdated`
- `<package-manager> why <package>`
- `<package-manager> install --lockfile-only` si validé
- Codemod officiel en dry-run si disponible
- `<package-manager> run typecheck`
- `<package-manager> run lint`
- `<package-manager> run test`
- `<package-manager> run build`

Ne pas lancer commande de mise à jour effective sans validation explicite.

## Ne Pas Faire

- Ne pas faire un saut majeur non demandé.
- Ne pas mélanger upgrade et refactor large.
- Ne pas changer de router Next.js.
- Ne pas ajouter ou supprimer une dépendance sans preuve et validation.
- Ne pas ignorer peer dependencies ou tests cassés.
- Ne pas modifier `.env` ou secrets.

## Format De Sortie

Pour un diagnostic :

- **État actuel**
- **Cible demandée ou recommandée**
- **Breaking changes**
- **Peer dependencies et blockers**
- **Fichiers impactés**
- **Plan d'exécution ordonné**
- **Tests/build**
- **Rollback**
- **Décisions à valider**

Après application validée :

- **Fichiers modifiés**
- **Dépendances mises à jour**
- **Corrections appliquées**
- **Commandes lancées**
- **Risques ou étapes restantes**
