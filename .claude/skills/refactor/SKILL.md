---
name: refactor
description: "Refactorise du code React, TypeScript ou Next.js sans changer le comportement observable. À utiliser pour simplifier composants, hooks, types, duplication, état dérivé, effets inutiles ou frontières Server/Client Components, avec diff minimal et tests de caractérisation."
---

# Refactoring React / Next.js

## Périmètre

Si l'utilisateur précise un fichier, composant, hook, dossier ou diff, limiter le
refactor à ce périmètre.

Si le périmètre est ambigu ou trop large, demander une cible avant de modifier.

## Objectif

Améliorer la structure sans changer le comportement observable :

- mêmes props publiques ;
- même rendu utile ;
- mêmes interactions ;
- mêmes effets de bord ;
- mêmes routes, payloads, erreurs et états utilisateur.

## Processus

1. Lire le code cible et les tests existants.
2. Rechercher les usages entrants du composant, hook ou service.
3. Identifier les conventions locales avant d'appliquer une règle générale.
4. Ajouter ou proposer un test de caractérisation si le comportement n'est pas couvert.
5. Appliquer le plus petit refactor cohérent.
6. Lancer les tests ciblés si possible.

## Refactors Couverts

- Composant trop complexe : extraire sous-composants privés ou fonctions pures.
- Hook trop chargé : séparer état, effets, data fetching et handlers.
- Duplication : extraire seulement si au moins deux usages réels existent.
- Types : clarifier props, unions, types dérivés, sans affaiblir le typage.
- État dérivé : remplacer state inutile par calcul au rendu ou memo justifié.
- Effets inutiles : supprimer effets de synchronisation évitables.
- Server/Client Components : réduire la surface client sans changer l'UX.
- Tests : préserver le comportement via tests existants ou caractérisation.

## Frontières Server / Client

- Ne pas ajouter `"use client"` à un arbre complet pour simplifier.
- Ne pas déplacer une logique serveur dans un Client Component.
- Ne pas exposer secret, cookie, token ou client interne côté navigateur.
- Préserver la sérialisabilité des props passées aux Client Components.

## Tests Et Validation

- Lancer tests unitaires ou composants proches.
- Lancer typecheck si signatures, props ou types changent.
- Lancer lint si imports, hooks ou conventions sont touchés.
- Ajouter un test de caractérisation quand le refactor touche un comportement non couvert.

## Ne Pas Faire

- Ne pas migrer de framework, router ou architecture.
- Ne pas changer de dépendance ou ajouter une librairie.
- Ne pas réécrire largement un module par préférence.
- Ne pas mélanger refactor et feature.
- Ne pas modifier comportement, payload, accessibilité ou route sans demande explicite.
- Ne pas faire d'améliorations opportunistes hors périmètre.

## Format De Sortie

Résumer :

- **Fichiers modifiés**
- **Refactors appliqués** et raison courte
- **Comportement préservé**
- **Tests ou validations lancés**
- **Risques ou limites restantes**
