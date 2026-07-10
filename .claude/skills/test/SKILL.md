---
name: test
description: "Ajoute ou corrige des tests unitaires et de composants pour React, TypeScript et Next.js. Détecte Jest ou Vitest et React Testing Library, privilégie les tests orientés comportement, requêtes accessibles, userEvent, composants asynchrones, hooks, mocks limités et tests de régression."
---

# Tests React / Next.js

## Périmètre

Créer ou corriger des tests unitaires et de composants. Pour E2E, proposer une
couverture seulement si le projet utilise déjà Playwright, Cypress ou équivalent.

Ne pas ajouter une nouvelle stack de test sans confirmation explicite.

## État Des Lieux

Inspecter :

- `package.json` : scripts et dépendances test.
- Configuration Jest ou Vitest.
- React Testing Library, `@testing-library/user-event`, `jest-dom`.
- Structure `tests/`, `__tests__/`, fichiers `*.test.*`, `*.spec.*`.
- Setup global, mocks existants, MSW si présent.
- Capacité du projet à tester Server Components.

## Principes

- Tester le comportement observable, pas l'implémentation.
- Utiliser les requêtes accessibles : `getByRole`, `getByLabelText`, `getByText` si pertinent.
- Utiliser `userEvent` pour les interactions.
- Garder un test lisible avec une intention claire.
- Limiter les mocks aux frontières externes : réseau, storage, router, horloge, dépendance externe.
- Ajouter un test de régression pour un bugfix.

## Cas À Couvrir

- Rendu nominal.
- États loading, empty, error et disabled.
- Interactions utilisateur.
- Composants asynchrones et résolution de promesses.
- Hooks via composant de test ou utilitaire existant.
- Validation de formulaires et messages d'erreur.
- Server Components seulement si la stack du projet le supporte.

## Stabilité

- Éviter assertions sur détails CSS ou structure fragile.
- Contrôler timers, dates, timezone et random si nécessaires.
- Nettoyer mocks et side effects.
- Attendre l'UI avec `findBy*` ou `waitFor` quand c'est réellement asynchrone.
- Ne pas augmenter les timeouts pour masquer une race condition.

## Next.js

- Respecter les mocks existants pour `next/navigation`, `next/router`, `next/image`, `next/link`.
- Distinguer App Router et Pages Router.
- Ne pas tester une Server Action comme une simple fonction si le projet a un helper dédié.

## Ne Pas Faire

- Ne pas installer Jest, Vitest, RTL, MSW ou Playwright sans confirmation.
- Ne pas mocker tout le composant testé.
- Ne pas tester uniquement que le composant "render sans crash".
- Ne pas supprimer ou assouplir un test pour faire passer la suite.
- Ne pas utiliser `data-testid` si une requête accessible convient.

## Format De Sortie

Après modification, résumer :

- **Tests ajoutés ou modifiés**
- **Comportements couverts**
- **Mocks utilisés**
- **Commandes lancées**
- **Limites ou tests encore manquants**
