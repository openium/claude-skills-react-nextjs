---
name: security
description: "Audite la sécurité d'un projet React et Next.js. Couvre XSS, validation serveur, authentification, autorisation, IDOR, CSRF selon architecture, redirections ouvertes, injection, uploads, cookies, headers, Server Actions, routes API, NEXT_PUBLIC_*, secrets dans le bundle, dépendances vulnérables et logs sensibles."
---

# Sécurité React / Next.js

## Périmètre

Par défaut, analyser les fichiers modifiés. Si l'utilisateur précise un fichier,
dossier, route, PR ou flux, limiter l'audit à ce périmètre.

Ne jamais lire, afficher ou recopier `.env` ni les fichiers d'environnement
sensibles. Ne jamais afficher de secret pendant l'audit.

## Contexte À Inspecter

- `package.json` et lockfile pour dépendances.
- `next.config.*`, middleware, headers, redirects.
- Route Handlers, API routes, Server Actions, services auth.
- Composants client exposant des données.
- Logs, analytics, observabilité.
- Uploads, formulaires, validations runtime.

## Critères D'Analyse

### XSS

- `dangerouslySetInnerHTML` sans sanitation prouvée.
- Injection dans URL, HTML, Markdown, script, analytics ou attribut.
- Données utilisateur rendues dans un contexte non échappé.

### Validation Serveur

- Validation uniquement côté client.
- Payload externe traité sans validation runtime.
- TypeScript utilisé comme unique barrière à l'entrée runtime.

### Authentification, Autorisation, IDOR

- Route API, Server Action ou page sensible sans contrôle d'accès.
- Accès ressource par id sans vérifier propriétaire, tenant ou rôle.
- Données utilisateur trop larges retournées au client.

### CSRF, Cookies Et Headers

- Mutation cookie/session sans protection adaptée à l'architecture.
- Cookie sensible sans `HttpOnly`, `Secure`, `SameSite` approprié.
- Headers sécurité absents ou affaiblis si le projet les gère.

### Redirections Et Injection

- Redirection ouverte via paramètre non validé.
- URL externe, commande, regex ou requête construite avec entrée utilisateur non validée.

### Uploads

- Type MIME, extension, taille ou nom de fichier non validés côté serveur.
- Upload dans chemin public dangereux.
- Fichier traité sans antivirus/sandbox si le contexte l'exige.

### Next.js Et Bundle

- Secret importé dans un Client Component.
- Variable passée en `NEXT_PUBLIC_*` sans preuve qu'elle peut être publique.
- Token, cookie, DSN ou clé API dans le bundle.
- Server Action exposant une opération sans validation ou autorisation.

### Dépendances Et Logs

- Dépendance vulnérable ou lockfile absent.
- Données personnelles, tokens, cookies ou Authorization dans logs/analytics.
- Source maps publiques exposant des informations sensibles si cela concerne le projet.

## Ne Pas Faire

- Ne pas afficher de secret pour prouver une fuite.
- Ne pas lire `.env`.
- Ne pas corriger automatiquement sans demande.
- Ne pas recommander une dépendance de sécurité sans justification et validation.
- Ne pas ignorer une validation serveur parce qu'une validation client existe.

## Format De Sortie

Pour chaque vulnérabilité :

- **Fichier et ligne**
- **Catégorie**
- **Sévérité** : bloquant, important ou suggestion
- **Preuve vérifiée** sans recopier de secret
- **Scénario d'exploitation réaliste**
- **Correction proposée**
- **Test ou vérification de non-régression**
