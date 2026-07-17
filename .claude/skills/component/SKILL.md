---
name: component
description: "Crée ou modifie un composant React/TypeScript/Next.js conforme au design system existant. À utiliser pour composants UI, API de props, variants, composition, responsive, états loading/empty/error/disabled, accessibilité, focus, tests et Storybook si déjà installé."
---

# Composants React / Next.js

## Périmètre

Créer ou modifier un composant en respectant le design system, les patterns et
les dépendances déjà présents.

Ne pas introduire une nouvelle bibliothèque UI sans confirmation explicite.

## État Des Lieux

Avant d'écrire :

- Chercher les composants voisins et usages similaires.
- Identifier style system : CSS Modules, Tailwind, Sass, styled-components, MUI, shadcn/ui, autre.
- Vérifier conventions de dossier, exports, tests et Storybook.
- Détecter si le composant doit être Server Component ou Client Component.
- Lire les tokens, variants ou primitives UI existants.

## API Du Composant

- Définir des props minimales, typées et stables.
- Préférer composition (`children`, slots existants) à une API trop large.
- Gérer variants seulement s'ils correspondent au design system.
- Nommer les props selon les composants existants.
- Éviter d'exposer des détails internes de style ou d'état.

## États UI

Prévoir selon le besoin :

- loading ;
- empty ;
- error ;
- disabled ;
- selected/active ;
- focus/hover ;
- responsive.

Ne pas inventer d'états si le design existant ne les prévoit pas, mais signaler
les manques qui bloquent l'usage.

## Accessibilité Et Focus

- Utiliser HTML sémantique avant ARIA.
- Fournir label, nom accessible et description si nécessaire.
- Gérer focus visible, ordre clavier, `aria-*`, `role` seulement quand utile.
- Pour modales, menus, popovers : focus initial, retour focus, Escape, navigation clavier.
- Ne pas remplacer un bouton par un `div` cliquable.

## Next.js

- Garder Server Component par défaut si aucune API client n'est nécessaire.
- Ajouter `"use client"` uniquement au fichier qui utilise état, effets, événements ou API navigateur.
- Ne pas importer un module serveur dans un composant client.
- Utiliser `next/image`, `next/link` ou primitives existantes selon le projet.

## Galerie De Composants

Si le projet dispose d'une page galerie (démarrage depuis un boilerplate),
référencer chaque composant réutilisable créé ou modifié avec tous ses états
visibles (loading, empty, error, disabled, variants). Cette page sert de
vérification visuelle. Ne pas la créer si le projet n'en a pas et si ce n'est
pas demandé.

## Tests Et Storybook

- Ajouter ou adapter un test de composant si le projet utilise Jest/Vitest et Testing Library.
- Tester comportement et accessibilité observable, pas l'implémentation interne.
- Utiliser `userEvent` pour interactions.
- Ajouter une story seulement si Storybook est déjà installé et utilisé.

## Ne Pas Faire

- Ne pas ajouter de nouvelle librairie UI sans confirmation.
- Ne pas créer un design parallèle au design system.
- Ne pas ajouter `"use client"` à toute une arborescence.
- Ne pas ignorer loading, error, disabled ou accessibilité quand le composant est interactif.
- Ne pas modifier les composants voisins sans lien direct.

## Format De Sortie

Résumer :

- **Composant créé ou modifié**
- **API de props**
- **États couverts**
- **Accessibilité et focus**
- **Tests/Storybook**
- **Limites ou décisions à valider**
