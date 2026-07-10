---
name: observability
description: "Audite ou améliore l'observabilité d'un projet React/Next.js. Couvre erreurs serveur et navigateur, logs structurés, corrélation des requêtes, traces, métriques, source maps, Sentry ou outil existant, alertes, sampling, protection des tokens/cookies/données personnelles et distinction erreurs attendues/inattendues."
---

# Observabilité React / Next.js

## Périmètre

Déterminer si l'utilisateur demande :

- Audit des logs, erreurs, traces ou métriques.
- Ajout de logs autour d'un flux, route, Server Action, API route ou client externe.
- Diagnostic d'un manque d'information en production.
- Intégration ou correction Sentry, OpenTelemetry, Datadog ou outil existant.
- Réduction de bruit ou fuite de données sensibles.

Ne pas lire ni afficher `.env`, DSN, tokens, cookies ou données personnelles.

## État Des Lieux

Inspecter selon le projet :

- `package.json` : Sentry, OpenTelemetry, Datadog, logger, analytics.
- `next.config.*`, instrumentation, middleware, source maps.
- Route Handlers, API routes, Server Actions, clients externes.
- Error boundaries, `error.tsx`, `global-error.tsx`, logs navigateur.
- CI/CD et configuration de déploiement si visible.

## Objectifs

Une bonne observabilité doit permettre de savoir :

- Qu'est-ce qui a échoué ?
- Côté serveur, navigateur, edge ou dépendance externe ?
- Pour quelle requête, route, tenant, ressource ou action, sans donnée sensible ?
- Erreur attendue, métier, transitoire ou incident ?
- Comment corréler logs, traces, métriques et événement frontend ?

## Logs Et Erreurs

- Utiliser logs structurés avec contexte stable.
- Ne pas logger tokens, cookies, Authorization, payload complet sensible ou PII inutile.
- Distinguer erreurs attendues et inattendues.
- Ne pas avaler une exception critique après log.
- Côté client, éviter de capturer des données de formulaire sensibles.

## Traces, Métriques, Alertes

- Propager request id ou correlation id si le projet le fait.
- Instrumenter frontières : route, action serveur, client externe, tâche async.
- Mesurer taux d'erreur, latence, timeouts, échecs critiques.
- Garder cardinalité maîtrisée : pas d'email, UUID utilisateur massif ou payload dynamique en label.
- Prévoir alertes actionnables et sampling adapté.

## Source Maps Et Outils

- Utiliser Sentry ou outil existant plutôt que changer d'outil.
- Vérifier upload de source maps sans exposer inutilement les sources publiques.
- Garder environnements, release et commit SHA cohérents si déjà utilisés.

## Tests Et Validation

- Tester qu'une erreur attendue n'est pas reportée comme incident si possible.
- Tester qu'une erreur critique remonte ou est loggée avec contexte utile.
- Vérifier que données sensibles ne sont pas incluses dans logs/events.
- Ne pas déclencher alertes production ou appels externes réels sans confirmation.

## Ne Pas Faire

- Ne pas ajouter une dépendance d'observabilité sans demande explicite.
- Ne pas logger des secrets pour debugger.
- Ne pas remplacer une exception par un simple log.
- Ne pas envoyer données personnelles à un outil tiers sans filtrage.
- Ne pas créer métriques ou tags à cardinalité incontrôlée.

## Format De Sortie

Présenter :

- **Findings** par sévérité, avec fichier/ligne, preuve, impact, correction.
- **Contexte à ajouter ou retirer**
- **Données sensibles protégées**
- **Tests ou vérifications conseillés**
- **Limites restantes**
