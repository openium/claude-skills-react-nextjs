---
name: debug
description: "Diagnostique une erreur frontend, React, TypeScript ou Next.js sans appliquer automatiquement une correction. À utiliser pour build cassé, erreur serveur ou navigateur, hydration mismatch, boucle de rendu, routing, cache obsolète, Server/Client Components, test instable ou écart dev/prod."
---

# Débogage React / Next.js

## Périmètre

Si l'utilisateur fournit une erreur, stacktrace, log, test en échec, URL, capture
ou fichier, commencer par ce signal.

Sinon :

- Lire `git status` pour repérer les changements récents.
- Identifier les scripts dans `package.json`.
- Inspecter la stack : lockfile, `tsconfig.json`, `next.config.*`, tests.
- Demander le symptôme exact si aucun signal exploitable n'est présent.

Ne pas appliquer automatiquement une correction. Proposer la correction après
avoir établi la cause racine, sauf demande explicite de modifier le code.

## Démarche Reproductible

Toujours structurer l'enquête :

1. **Symptôme** : message, commande, page, environnement, navigateur, test.
2. **Reproduction** : plus petite commande ou suite d'actions qui déclenche le bug.
3. **Hypothèses** : causes possibles, triées par probabilité.
4. **Preuves** : fichiers, lignes, logs, diff, config ou comportement observé.
5. **Cause racine** : cause minimale qui explique le symptôme.
6. **Correction proposée** : changement ciblé, sans refactor opportuniste.
7. **Test de non-régression** : test ou validation qui prouve la correction.

Si la reproduction est impossible, dire ce qui manque et quelle donnée collecter.

## Points D'Analyse

### Build Et TypeScript

- Erreur `next build`, `tsc`, ESLint ou bundler.
- Type public modifié, `any`, assertion `as`, non-null assertion ou config TS contournée.
- Différence entre `next dev` et `next build`.
- Import incompatible serveur/client ou module absent dans l'environnement cible.

### Erreurs Serveur Et Navigateur

- Lire la première frame applicative.
- Distinguer erreur Node.js, Route Handler, Server Action, middleware, navigateur.
- Vérifier que les secrets ne sont pas affichés dans les logs ou la réponse.
- Comparer environnement local, CI, preview et production si le bug dépend du contexte.

### Hydration Mismatch

- Chercher date, random, locale, timezone, `window`, media query, storage, taille d'écran.
- Vérifier markup serveur/client et valeurs non stables au premier rendu.
- Identifier si le composant doit rester serveur ou devenir client localement.

### Hooks Et Boucles De Rendu

- Dépendances `useEffect`, `useMemo`, `useCallback`.
- State update pendant le rendu ou effet qui réécrit sa propre dépendance.
- Hook conditionnel, race condition, requête non annulée.
- État dérivé stocké inutilement.

### Routing

- App Router : layouts, pages, route handlers, dynamic segments, `notFound`, `redirect`.
- Pages Router : `getServerSideProps`, `getStaticProps`, API routes, query params.
- Architecture hybride : vérifier la zone réellement concernée.
- Middleware, redirects, trailing slash, basePath, i18n si configurés.

### Cache Et Données Obsolètes

- `fetch` cache, `revalidate`, tags, `revalidatePath`, `revalidateTag`.
- SWR/TanStack Query si déjà utilisés.
- Cache navigateur, CDN, edge, ISR, données par utilisateur ou tenant.
- Invalidation après mutation.

### Server / Client Components

- API navigateur ou hooks client dans un Server Component.
- Secret ou logique serveur importé côté client.
- Props non sérialisables entre serveur et client.
- `"use client"` placé trop haut pour masquer le problème.

### Tests Instables

- Asynchronisme non attendu, timers, race, ordre des tests, mock global.
- Requête Testing Library non accessible ou assertion trop liée à l'implémentation.
- Écart entre environnement test et runtime Next.js.

## Commandes Utiles

Adapter au lockfile et aux scripts existants :

- `<package-manager> run build`
- `<package-manager> run typecheck`
- `<package-manager> run lint`
- `<package-manager> run test -- <filtre>`
- `<package-manager> run test:e2e -- <filtre>`
- `next build` seulement si le projet l'utilise directement

Ne pas lancer de commande destructive, migration, reset de données ou nettoyage
global sans confirmation explicite.

## Ne Pas Faire

- Ne pas masquer une erreur par un fallback silencieux.
- Ne pas supprimer ou assouplir un test sans preuve qu'il est obsolète.
- Ne pas désactiver TypeScript, ESLint, hydration warnings ou checks de build.
- Ne pas vider le cache comme solution durable sans cause prouvée.
- Ne pas exposer secrets, cookies, tokens ou données personnelles.
- Ne pas transformer une variable en `NEXT_PUBLIC_*` sans preuve qu'elle est publique.

## Format De Sortie

Répondre avec :

1. **Symptôme** : erreur ou comportement observé.
2. **Reproduction** : commande ou scénario minimal.
3. **Hypothèses** : triées par probabilité si la cause n'est pas prouvée.
4. **Preuves** : fichiers, lignes, logs ou résultats de commandes.
5. **Cause racine** : diagnostic en une phrase avec niveau de confiance.
6. **Correction proposée** : changement ciblé à appliquer.
7. **Test de non-régression** : test ou validation à ajouter/lancer.
