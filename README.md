# Claude Skills React/Next.js

Ce dépôt contient des skills Claude Code pour React, TypeScript et Next.js.

Les skills sont installés dans le dossier `.claude/skills/`. Chaque skill
possède un fichier `SKILL.md` qui décrit son usage, ses règles et ses
conventions.

Pour utiliser un skill dans un projet, copiez son dossier dans
`.claude/skills/` à la racine du projet cible. Pour installer l'ensemble des
skills, copiez tout le contenu de ce dépôt vers le dossier `.claude/skills/` du
projet cible.

Les conventions détaillées seront ajoutées progressivement.

## Skills Disponibles

### Qualité De Code

| Skill | Description |
|-------|-------------|
| [`/review`](.claude/skills/review/SKILL.md) | Revue de code React/Next.js : bugs, typage, Server/Client Components, sécurité, accessibilité, performance, tests. |
| [`/refactor`](.claude/skills/refactor/SKILL.md) | Refactoring sûr de composants, hooks et code TypeScript sans changement de comportement observable. |

### UI, Données Et Tests

| Skill | Description |
|-------|-------------|
| [`/component`](.claude/skills/component/SKILL.md) | Création ou modification de composants conformes au design system existant. |
| [`/data-fetching`](.claude/skills/data-fetching/SKILL.md) | Stratégie de chargement, cache, revalidation, pagination et erreurs côté serveur ou client. |
| [`/forms`](.claude/skills/forms/SKILL.md) | Formulaires React/Next.js : validation, soumission, erreurs, accessibilité et tests. |
| [`/test`](.claude/skills/test/SKILL.md) | Tests unitaires et composants avec Jest/Vitest et React Testing Library selon le projet. |

### Sécurité, Performance Et Observabilité

| Skill | Description |
|-------|-------------|
| [`/accessibility`](.claude/skills/accessibility/SKILL.md) | Audit accessibilité : sémantique, clavier, focus, formulaires, contraste et vérifications. |
| [`/security`](.claude/skills/security/SKILL.md) | Audit sécurité React/Next.js : XSS, auth, IDOR, CSRF, secrets, routes API et dépendances. |
| [`/performance`](.claude/skills/performance/SKILL.md) | Audit performance : rendu, hydratation, bundle, cache, images, fonts et Core Web Vitals. |
| [`/observability`](.claude/skills/observability/SKILL.md) | Logs, erreurs, traces, métriques, Sentry/APM, alertes et protection des données sensibles. |

### Planification Et Onboarding

| Skill | Description |
|-------|-------------|
| [`/plan`](.claude/skills/plan/SKILL.md) | Plan d'implémentation React/Next.js avant code : étapes, fichiers, tests, validations, risques. |
| [`/debug`](.claude/skills/debug/SKILL.md) | Diagnostic reproductible d'erreurs frontend ou Next.js sans correction automatique. |
| [`/upgrade`](.claude/skills/upgrade/SKILL.md) | Plan d'upgrade Node.js, React, Next.js, TypeScript et dépendances frontend. |
| [`/onboard`](.claude/skills/onboard/SKILL.md) | Guide d'onboarding technique pour un projet React/Next.js existant. |

## Exemples D'Invocation

| Besoin | Skill | Exemple |
|--------|-------|---------|
| Relire un diff avant commit | `/review` | `/review analyse le diff staged avant commit` |
| Diagnostiquer une erreur | `/debug` | `/debug analyse cette erreur d'hydratation Next.js` |
| Simplifier un composant | `/refactor` | `/refactor simplifie components/ProductCard.tsx sans changer le comportement` |
| Créer un composant | `/component` | `/component crée un composant Modal conforme au design system` |
| Corriger un chargement de données | `/data-fetching` | `/data-fetching corrige la revalidation de la liste produits` |
| Ajouter un formulaire | `/forms` | `/forms crée le formulaire de contact avec validation serveur` |
| Ajouter des tests | `/test` | `/test ajoute les tests du composant ProductCard` |
| Auditer l'accessibilité | `/accessibility` | `/accessibility audite le formulaire de checkout` |
| Auditer la sécurité | `/security` | `/security audite les routes API modifiées` |
| Diagnostiquer une lenteur | `/performance` | `/performance analyse le bundle de la page dashboard` |
| Améliorer les logs | `/observability` | `/observability propose les logs utiles autour de la Server Action de paiement` |
| Planifier une feature ou un bugfix | `/plan` | `/plan prépare l'implémentation de <feature-ou-bugfix>` |
| Préparer un upgrade | `/upgrade` | `/upgrade prépare le passage à Next.js <version>` |
| Documenter un projet | `/onboard` | `/onboard génère un guide pour un nouveau développeur` |

## Convention De Structure

Chaque skill est un dossier dans `.claude/skills/<nom>/` avec un fichier
obligatoire `SKILL.md`.

```text
.claude/
└── skills/
    └── nom-du-skill/
        └── SKILL.md
```

Le nom du dossier et le champ `name` doivent être identiques.

```yaml
---
name: nom-du-skill
description: "Description courte et précise du moment où utiliser ce skill."
---
```

## Template projet

Un template de `CLAUDE.md` est disponible dans `templates/CLAUDE.md`. Il sert de
base pour créer des instructions projet adaptées à une application React,
TypeScript et Next.js existante.

Prompt court :

```text
À partir du template templates/CLAUDE.md, génère un CLAUDE.md adapté à ce projet.
Inspecte d'abord la stack, le lockfile, le router Next.js, les scripts, les tests
et les conventions existantes. Garde le fichier concis et ne lis pas `.env` ni
les fichiers d'environnement sensibles.
```
