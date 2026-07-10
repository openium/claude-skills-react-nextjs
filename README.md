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

### Planification Et Onboarding

| Skill | Description |
|-------|-------------|
| [`/plan`](.claude/skills/plan/SKILL.md) | Plan d'implémentation React/Next.js avant code : étapes, fichiers, tests, validations, risques. |

## Exemples D'Invocation

| Besoin | Skill | Exemple |
|--------|-------|---------|
| Relire un diff avant commit | `/review` | `/review analyse le diff staged avant commit` |
| Planifier une feature ou un bugfix | `/plan` | `/plan prépare l'implémentation de <feature-ou-bugfix>` |

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
