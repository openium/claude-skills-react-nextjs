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
