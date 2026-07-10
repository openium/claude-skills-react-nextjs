---
name: review
description: "Revue de code React, TypeScript et Next.js pour diff git, branche, PR, fichiers ou périmètre donné. À utiliser pour relire une modification avant commit/merge, auditer une PR ou vérifier des composants/routes/hooks/services. Priorise bugs, régressions, typage, frontières Server/Client Components, hooks, sécurité, cache, data fetching, hydratation, accessibilité, performance, bundle et tests manquants."
---

# Revue de code React / Next.js

## Périmètre

Si l'utilisateur précise un fichier, dossier, commit, branche, PR ou périmètre,
analyser uniquement celui-ci.

Sinon :

- Analyser le diff staged (`git diff --cached`) s'il existe.
- À défaut, analyser le diff non staged (`git diff`).
- Si aucun diff n'existe, demander le périmètre à reviewer.

Ne jamais modifier le code pendant une review, sauf demande explicite.

## Processus

1. Lire `git status` et un résumé du diff (`git diff --stat` ou équivalent).
2. Lire le diff détaillé des fichiers concernés.
3. Inspecter les fichiers voisins uniquement si nécessaire pour prouver ou écarter un risque.
4. Lire les configs utiles si le risque dépend du projet : `package.json`, lockfile, `tsconfig.json`, `next.config.*`, lint et tests.
5. Détecter App Router, Pages Router ou architecture hybride si une route, un rendu ou un cache est concerné.
6. Remonter seulement les problèmes prouvés par le code, la configuration ou le diff.

Ne pas signaler un risque spéculatif sans chemin d'exécution crédible.

## Sévérités

- **Bloquant** : bug avéré, faille de sécurité, fuite de données, régression, hydratation cassée, crash runtime, erreur de typage masquée, rupture de contrat public.
- **Important** : risque réel et plausible, test manquant sur comportement critique, accessibilité bloquante, dégradation performance ou cache probable.
- **Suggestion** : lisibilité, convention, simplification, test additionnel ou amélioration sans risque immédiat.

## Points D'Analyse

### Bugs Et Régressions

- Changement de comportement non couvert ou non annoncé.
- Cas limite cassé : `null`, `undefined`, tableau vide, chaîne vide, valeur extrême.
- État loading/error/empty supprimé ou incohérent.
- Erreur avalée, fallback trompeur ou promesse non gérée.
- Contrat de props, payload API, route, query param ou réponse modifié sans compatibilité.

### Typage TypeScript

- `any`, assertion `as` ou non-null assertion `!` qui masque un cas réel.
- Type public modifié sans adaptation des consommateurs.
- Types serveur/client confondus : données sérialisables, `Date`, `Map`, fonctions, classes.
- Génériques ou unions qui perdent les cas d'erreur.
- Désactivation de règles TypeScript ou contournement du typechecker.

### Frontières Server / Client Components

- `"use client"` ajouté trop haut dans l'arbre.
- Hook client, API navigateur ou event handler utilisé dans un Server Component.
- Secret, token, cookie sensible ou logique serveur exposé dans un Client Component.
- Props non sérialisables passées d'un Server Component à un Client Component.
- Composant serveur rendu dépendant d'un état navigateur.

### Hooks Et Effets

- Dépendances manquantes ou inutiles dans `useEffect`, `useMemo`, `useCallback`.
- Effet utilisé pour dériver un état calculable au rendu.
- Race condition, requête concurrente non annulée ou state update après unmount.
- Hook appelé conditionnellement ou dans une boucle.
- État dupliqué entre URL, store, cache et composant sans source de vérité claire.

### Sécurité Et Exposition De Données

- Secret, token, clé API, DSN ou donnée personnelle exposée côté client.
- Variable transformée en `NEXT_PUBLIC_*` sans preuve qu'elle peut être publique.
- Lecture, affichage ou commit de `.env` ou fichier d'environnement sensible.
- HTML injecté via `dangerouslySetInnerHTML` sans sanitation prouvée.
- Route handler, API route, server action ou middleware sans contrôle d'accès attendu.
- Redirection, URL externe ou paramètre utilisateur non validé.
- Log ou analytics contenant données sensibles.

### Cache Et Data Fetching

- Stratégie `fetch` cache/revalidate incohérente avec la fraîcheur attendue.
- Invalidation manquante après mutation : tags, paths, SWR/TanStack Query.
- Données utilisateur ou tenant mises en cache trop largement.
- Requête dupliquée entre serveur et client.
- Waterfall évitable ou fetch dans un composant client alors que le serveur convient.
- Gestion d'erreur, loading ou retry incompatible avec l'expérience existante.

### Hydratation Et Rendu

- Mismatch probable : date, locale, random, timezone, `window`, taille d'écran.
- Différence de markup entre serveur et client.
- Composant client dépendant d'une valeur non stable au premier rendu.
- Usage SSR/SSG/ISR/dynamic non compatible avec les données.
- Suspense, streaming ou fallback qui cache une erreur utilisateur.

### Accessibilité

- Champ sans label accessible ou description utile.
- Bouton/lien sans nom accessible ou mauvais rôle interactif.
- Navigation clavier, focus visible ou ordre de tabulation cassé.
- Modale, menu ou popover sans gestion focus/escape/aria adaptée.
- Image informative sans alternative.
- Message d'erreur non associé au champ ou non annoncé.

### Performance Et Bundle

- Import lourd dans un Client Component ou dans le chemin critique.
- Perte de code splitting, dynamic import manquant ou bundle partagé gonflé.
- Re-render global évitable via provider, store ou props instables.
- Calcul coûteux à chaque rendu sans nécessité.
- Images non optimisées, dimensions absentes ou priorité mal utilisée.
- N+1 côté API/client, pagination absente ou payload trop volumineux.

### Tests

- Comportement critique ajouté sans test.
- Bugfix sans test de régression.
- Composant interactif sans test d'interaction ou d'état d'erreur.
- Route ou data fetching critique sans test d'intégration/E2E adapté.
- Mock qui ne reflète pas le contrat réel.
- Test supprimé, désactivé ou assoupli pour faire passer la validation.

## Ne Pas Signaler

- Une absence de test si le diff ne change pas de comportement.
- Une convention si le projet suit clairement une convention différente.
- Une amélioration stylistique sans impact quand des problèmes plus importants existent.
- Le même problème répété ligne par ligne : grouper la remarque.
- Une préférence App Router/Pages Router sans lien avec un bug réel.
- Une migration, dépendance ou refactor non demandé.

## Format De Sortie

Présenter les findings en premier, triés par sévérité décroissante.

Pour chaque finding :

- **Fichier et ligne**
- **Sévérité** : bloquant, important ou suggestion
- **Preuve** : extrait ou fait vérifié dans le code, la configuration ou le diff
- **Impact** : conséquence concrète pour l'utilisateur, la sécurité, la stabilité ou la maintenance
- **Correction** : changement recommandé sans appliquer le code
- **Test conseillé** : test ou validation qui couvrirait le problème

Après les findings, ajouter si utile :

- **Questions ouvertes** : uniquement les informations nécessaires pour conclure.
- **Limites de la revue** : tests non exécutés, périmètre incomplet, dépendance à une PR distante.

Si aucun problème n'est trouvé, le dire clairement et mentionner les éventuels
tests non exécutés ou limites de la revue.
