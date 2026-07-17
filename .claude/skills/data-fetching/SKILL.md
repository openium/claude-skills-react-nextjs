---
name: data-fetching
description: "Conçoit ou corrige le data fetching dans un projet React/Next.js. Couvre chargement serveur ou client, stratégie existante, cache, revalidation, invalidation, erreurs, annulation, concurrence, pagination, déduplication, données sensibles et états loading/empty."
---

# Data Fetching React / Next.js

## Périmètre

Traiter les chargements de données côté serveur ou client selon l'architecture
existante du projet.

Ne pas introduire TanStack Query, SWR, GraphQL client ou autre dépendance si le
projet ne les utilise pas déjà, sauf confirmation explicite.

## État Des Lieux

Inspecter :

- Router utilisé : App Router, Pages Router ou hybride.
- Patterns existants : `fetch`, Server Components, Route Handlers, API routes, Server Actions.
- Bibliothèques déjà présentes : TanStack Query, SWR, Apollo, urql, client custom.
- Stratégie de cache : `revalidate`, tags, cache client, CDN, ISR.
- Gestion d'erreurs et états loading/empty existants.
- Types de données et présence de données sensibles.

## Choix Serveur Ou Client

- Préférer le chargement serveur pour données initiales, SEO, sécurité et réduction du bundle.
- Préférer le client pour interactions fréquentes, données très dynamiques ou état utilisateur local.
- Ne pas dupliquer la même requête serveur et client sans raison.
- Ne pas exposer token, secret ou donnée sensible au navigateur.

## Cache, Revalidation Et Invalidation

- Définir fraîcheur attendue, portée utilisateur/tenant et invalidation.
- Pour App Router, choisir explicitement cache, `no-store`, `revalidate`, tags ou invalidation par path/tag.
- Pour SWR/TanStack Query, respecter query keys, stale time et invalidation existantes.
- Éviter de cacher globalement des données personnalisées.

## Erreurs, Concurrence Et Annulation

- Gérer erreurs attendues et inattendues.
- Prévoir retry seulement si le projet le fait et si l'opération est idempotente.
- Annuler ou ignorer les réponses obsolètes côté client.
- Éviter les race conditions entre filtres, pagination et mutation.

## Pagination Et Déduplication

- Ajouter pagination, limite ou infinite loading si le volume peut grandir.
- Dédupliquer requêtes identiques quand la stack le permet.
- Éviter les waterfalls serveur/client.
- Garder les query params comme source de vérité si le projet l'utilise.

## États UI

Prévoir :

- loading initial ;
- refresh/revalidation ;
- empty ;
- error ;
- pagination terminée ;
- disabled pendant mutation si nécessaire.

## Chargement Scindé Par Section

- Ne pas bloquer toute une page sur un unique `Promise.all` avec un loader
  plein écran.
- Scinder les appels indépendants en sous-composants, chacun avec son propre
  `Suspense` et son skeleton, pour afficher chaque bloc dès que sa donnée arrive
  (streaming) plutôt que d'attendre la requête la plus lente.
- Réserver le loader global au cas où toutes les données sont réellement
  interdépendantes.

## Tests Et Validation

- Tester succès, erreur, empty et pagination si applicables.
- Mocker seulement la frontière externe : `fetch`, client API, MSW si déjà utilisé.
- Vérifier typecheck, tests composants et build.

## Ne Pas Faire

- Ne pas ajouter TanStack Query ou SWR si absent sans confirmation.
- Ne pas rendre publique une variable ou un secret pour appeler une API côté client.
- Ne pas cacher des données utilisateur dans un cache partagé.
- Ne pas masquer les erreurs par un tableau vide.
- Ne pas faire de refactor de couche data hors périmètre.

## Format De Sortie

Présenter :

- **Stratégie détectée**
- **Choix serveur/client**
- **Cache et invalidation**
- **États et erreurs**
- **Fichiers à modifier**
- **Tests et validations**
- **Risques ou décisions à valider**
