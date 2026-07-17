# CLAUDE.md

Instructions projet pour Claude Code.

Ce fichier doit rester court, concret et maintenu avec le projet. Il complète
les skills spécialisés disponibles dans `.claude/skills/`.

## Contexte Du Projet

- Nom du projet : `<nom-du-projet>`
- Domaine métier : `<description courte>`
- Type d'application : `<site vitrine, SaaS, back-office, e-commerce, app mobile web, librairie>`
- Environnement principal : `<local Node.js, Docker, CI uniquement, plateforme cloud>`
- Niveau de criticité : `<faible, moyen, élevé>`

## Stack Technique

- Node.js : `<version détectée dans .nvmrc, package.json, Dockerfile ou CI>`
- Gestionnaire de paquets : `<npm, pnpm ou Yarn selon le lockfile présent>`
- React : `<version>`
- Next.js : `<version>`
- Router : `<App Router, Pages Router ou architecture hybride>`
- TypeScript : `<strict, non strict, configuration particulière>`
- Styles et design system : `<CSS Modules, Tailwind CSS, Sass, styled-components, MUI, shadcn/ui, autre>`
- Gestion d'état : `<React state, Context, Redux, Zustand, Jotai, TanStack Query, autre>`
- Data fetching : `<Server Components, fetch, API routes, Route Handlers, SWR, TanStack Query, GraphQL, autre>`
- Tests unitaires, composants et E2E : `<Vitest, Jest, Testing Library, Storybook, Playwright, Cypress, autre>`
- Observabilité : `<Sentry, Datadog, OpenTelemetry, logs plateforme, analytics, aucun>`

## Règles Générales De Modification

- Répondre en français technique, concis et actionnable.
- Lire le code, les scripts et les fichiers de configuration avant de modifier.
- Préserver les conventions et versions réellement détectées dans le projet.
- Utiliser le gestionnaire de paquets correspondant au lockfile.
- Ne pas supposer App Router, Pages Router, React Server Components ou une version récente sans preuve.
- Ne pas migrer automatiquement du Pages Router vers l'App Router.
- Ne pas ajouter `"use client"` à un arbre complet par facilité.
- Ne pas désactiver TypeScript, ESLint ou les tests pour faire passer une validation.
- Ne pas faire de refactor opportuniste sans lien direct avec la demande.

## Conventions De Génération Au Démarrage De Projet

Ces règles s'appliquent quand du code neuf est généré à partir du boilerplate
(phase de démarrage, avant reprise par les développeurs). Elles sont
prescriptives : les appliquer par défaut, sans attendre qu'un existant les
impose. Objectif : livrer un code que les développeurs peuvent intégrer sans
tout renommer ni tout retraduire.

### Langue du code contre langue de l'interface

- Tout le **code** est en anglais, sans exception : noms de fichiers, dossiers,
  routes, fonctions, composants, hooks, variables, types et **propriétés
  d'objets**.
- Le **français** n'existe que dans les fichiers de traduction. Jamais dans un
  identifiant, un nom de route ou une clé d'objet.
- Un libellé métier français dans la demande ne devient pas un nom de code
  français : le traduire en anglais pour nommer le code, garder le français
  pour le texte affiché.

### Schéma de données — question à poser en premier

Avant de générer types, routes ou services de données, poser la question :

> « Existe-t-il un schéma de base de données ou un contrat d'API back sur lequel
> je dois m'aligner ? »

- Si oui : les types et propriétés front reprennent **exactement** les noms du
  back (noms inclus). Ne jamais mapper ni transformer la réponse API pour
  « traduire » ou « réarranger » les champs — c'est ce qui force à tout
  réécrire ensuite. Une conversion UI ↔ API réellement nécessaire (ex. toggle
  booléen vers enum) se fait dans la couche service, pas dans le composant.
- Si non : nommer selon les conventions REST/API anglaises usuelles et signaler
  que ces noms devront être confirmés à l'arrivée du vrai back.

### Traductions dès la génération

- Aucun texte visible en dur. Tout texte affiché passe par `next-intl` dès
  l'écriture du composant.
  - Server Components : `const t = await getTranslations('namespace')`
  - Client Components : `const t = useTranslations('namespace')`
- Chaque clé est ajoutée dans `messages/fr.json` **et** `messages/en.json` en
  même temps, sous un namespace correspondant à la section.
- Dans le JSX : guillemets simples et accolades — `{t('cle')}`, pas
  `t("cle")` ni `{t("cle")}`. Pour injecter du JSX dans une chaîne, utiliser
  `t.rich('cle', { ... })`.

### `generateMetadata` sur chaque page

Toute page exporte `generateMetadata`, alimentée par une clé de traduction
(`<namespace>.titleMetadata`) — pas de page sans titre d'onglet.

```ts
import type { Metadata } from 'next';
import { getTranslations } from 'next-intl/server';

export async function generateMetadata(props: {
  params: Promise<{ lng: string }>;
}): Promise<Metadata> {
  const { lng } = await props.params;
  const t = await getTranslations({ locale: lng, namespace: 'myNamespace' });
  return { title: t('titleMetadata') };
}
```

La clé `titleMetadata` est ajoutée dans `messages/fr.json` et `messages/en.json`.

### Zéro duplication

Dès que des blocs de JSX ou de logique sont identiques ou quasi identiques,
extraire un composant ou un helper réutilisable plutôt que copier-coller.

### Chargement scindé, pas de loader global

- Ne pas bloquer une page entière sur un unique `Promise.all` avec un loader
  plein écran.
- Scinder les appels de données indépendants en sous-composants, chacun avec
  son propre `Suspense` et son skeleton, pour afficher chaque bloc dès que sa
  donnée arrive (streaming) plutôt que d'attendre la donnée la plus lente.

### Galerie de composants

À chaque ajout ou modification d'un composant réutilisable, le référencer dans
une page galerie qui montre tous ses états (loading, empty, error, disabled,
variants). Cette page sert de vérification visuelle pour la designer et les
développeurs.

## Sécurité Et Données Sensibles

- Ne jamais lire, afficher ou committer `.env` ni les autres fichiers d'environnement sensibles.
- Ne jamais exposer secrets, tokens, mots de passe, cookies, clés API, DSN ou données personnelles sensibles.
- Ne pas transformer une variable en `NEXT_PUBLIC_*` sans vérifier qu'elle peut être publique côté navigateur.
- Ne pas ajouter de credentials réalistes dans le code, les tests, les fixtures ou la documentation.

## Workflow Avant, Pendant Et Après Modification

Avant de modifier :

1. Identifier le besoin exact.
2. Lire les fichiers concernés et les conventions locales.
3. Vérifier la stack détectée, le router Next.js utilisé et le lockfile.
4. Proposer un plan court si la modification touche plusieurs zones.

Pendant la modification :

- Garder le diff minimal.
- Respecter les frontières serveur/client de Next.js.
- Ajouter ou adapter les tests quand le comportement change.
- Conserver les noms, patterns et abstractions existants.

Après la modification :

- Résumer les fichiers modifiés.
- Indiquer les commandes lancées.
- Signaler les tests non lancés ou impossibles à lancer.
- Mentionner les risques résiduels si nécessaire.

## Commandes Du Projet

Adapter cette section au projet et au gestionnaire de paquets détecté.

```bash
# Installer les dépendances
<npm install | pnpm install | yarn install>

# Lancer le serveur de développement
<npm run dev | pnpm dev | yarn dev>

# Vérifier le typage
<npm run typecheck | pnpm typecheck | yarn typecheck>

# Linter
<npm run lint | pnpm lint | yarn lint>

# Tests unitaires et composants
<npm run test | pnpm test | yarn test>

# Tests E2E
<npm run test:e2e | pnpm test:e2e | yarn test:e2e>

# Build de production
<npm run build | pnpm build | yarn build>
```

## Skills Disponibles

Adapter cette liste aux skills réellement installés dans `.claude/skills/`.

- `<skill-review>` : `<usage>`
- `<skill-debug>` : `<usage>`
- `<skill-test>` : `<usage>`
- `<skill-nextjs>` : `<usage>`
- `<skill-performance>` : `<usage>`
- `<skill-accessibility>` : `<usage>`

## Conventions React, TypeScript Et Next.js

- Composants : `<organisation, conventions de nommage, co-location>`
- Server Components : privilégier le rendu serveur quand il est compatible avec le besoin.
- Client Components : limiter `"use client"` aux composants qui ont besoin d'état navigateur, d'effets ou d'API client.
- Routing : respecter l'architecture existante App Router, Pages Router ou hybride.
- Data fetching : suivre les patterns déjà utilisés pour cache, revalidation, erreurs et loading states.
- TypeScript : éviter `any`; utiliser les types existants et expliciter les contrats publics.
- Formulaires : `<React Hook Form, actions serveur, validation Zod, autre>`
- Accessibilité : conserver labels, états focus, navigation clavier et textes alternatifs.
- Performance : éviter les bundles client inutiles, imports lourds côté client et recalculs non nécessaires.

## Tests

- Unitaires : `<emplacement et commande>`
- Composants : `<emplacement et commande>`
- E2E : `<emplacement et commande>`
- Fixtures et mocks : `<conventions>`

Quand un bug est corrigé, ajouter un test de régression si le projet le permet.
Ne pas supprimer ou assouplir un test pour masquer une régression.

## Git, Branches, Commits, Pull Requests Et Déploiements

- Ne pas modifier les fichiers sans rapport avec la demande.
- Ne pas revert des changements utilisateur sans demande explicite.
- Ne pas inclure fichiers IDE, caches, logs, artefacts générés ou fichiers d'environnement.
- Ne pas lancer de commande Git ou de données destructive sans confirmation explicite.
- Vérifier les diffs staged et non staged avant un commit.
- Documenter dans la pull request les commandes de vérification lancées et les risques connus.
- Ne pas créer ou pousser de tag, branche ou déploiement sans demande explicite.

### Branches

- Créer les branches de travail depuis la branche d'intégration du projet.
- Utiliser le numéro de ticket associé dans le nom de branche.
- Utiliser un nom court en kebab-case après le ticket.

Format recommandé :

```text
<type>/<ticket>_<description-courte>
```

Exemples :

```text
feature/US-123_login-form
fix/TICKET-456_query-params
hotfix/INC-789_checkout-error
chore/TECH-321_update-next-config
```

### Commits

- Utiliser la convention Conventional Commits.
- Utiliser le numéro de ticket associé comme scope.
- Si aucun ticket n'existe, demander le scope attendu ou utiliser un scope technique court validé par le projet.

Format de commit recommandé :

```text
<type>(<scope>): <description courte>
```

Exemples :

```text
feat(US-123): add login form
fix(TICKET-456): preserve query params
test(TICKET-456): add router regression test
docs(TECH-321): document local setup
```

## Notes Propres Au Projet

Ajouter ici les décisions locales importantes :

- `<decision 1>`
- `<decision 2>`
- `<decision 3>`
