---
name: performance
description: "Audite et optimise la performance React/Next.js avec mesures avant optimisation. Couvre rendu React, hydratation, Server/Client Components, bundle JavaScript, imports, images, fonts, cache, requêtes en cascade, Core Web Vitals, listes volumineuses, memoization justifiée et build de production."
---

# Performance React / Next.js

## Périmètre

Si l'utilisateur précise une page, composant, route, diff ou métrique, analyser
uniquement ce périmètre. Sinon demander le symptôme : lenteur, bundle, LCP, CLS,
INP, hydratation, requêtes, build.

Ne pas modifier le code pendant un audit sauf demande explicite.

## Mesurer Avant D'Optimiser

Chercher une preuve :

- Core Web Vitals : LCP, CLS, INP, TTFB.
- Taille JS par route ou chunk.
- Nombre de rendus et coût React.
- Durée d'hydratation.
- Requêtes réseau et waterfalls.
- Cache hit/miss ou revalidation.
- Taille et dimensions images/fonts.
- Temps `next build`.

Si la mesure directe est impossible, séparer clairement hypothèse et preuve.

## Points D'Analyse

- Rendu React : re-renders globaux, props instables, providers trop hauts.
- Hydratation : surface client excessive, mismatch, composants lourds.
- Server/Client Components : logique client inutile, imports serveur/client.
- Bundle : dépendances lourdes, imports barrel, dynamic import, tree shaking.
- Images et fonts : `next/image`, dimensions, formats, priority, preload, font display.
- Cache : `fetch` cache, ISR, CDN, SWR/TanStack Query, invalidation.
- Requêtes en cascade : waterfalls, duplication serveur/client, absence de parallélisation.
- Listes volumineuses : pagination, virtualisation si déjà acceptée, rendu borné.
- Memoization : `memo`, `useMemo`, `useCallback` seulement avec cause prouvée.
- Build production : analyser `next build`, analyzer si déjà configuré.

## Corrections Attendues

- Réduire la surface client avant de memoizer partout.
- Déplacer au serveur ce qui n'a pas besoin du navigateur.
- Diviser les imports lourds ou charger à la demande.
- Ajouter cache avec stratégie d'invalidation claire.
- Optimiser images/fonts selon les primitives existantes.
- Supprimer requêtes dupliquées ou waterfalls prouvés.

## Ne Pas Faire

- Ne pas optimiser sans preuve ou hypothèse explicitement annoncée.
- Ne pas ajouter memoization spéculative.
- Ne pas ajouter cache sans invalidation ou fraîcheur définie.
- Ne pas dégrader accessibilité ou comportement métier pour gagner du temps.
- Ne pas ajouter de dépendance d'analyse ou runtime sans confirmation.

## Format De Sortie

Présenter les findings par sévérité :

- **Fichier et ligne**
- **Type** : rendu, hydratation, bundle, image, cache, réseau, Core Web Vitals
- **Preuve ou mesure**
- **Impact**
- **Cause racine**
- **Correction proposée**
- **Validation conseillée**
- **Gain attendu** si estimable
