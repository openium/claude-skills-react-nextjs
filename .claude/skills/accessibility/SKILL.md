---
name: accessibility
description: "Audite ou améliore l'accessibilité d'une interface React/Next.js. Couvre HTML sémantique, clavier, focus, labels, formulaires, modales, menus, contraste, alternatives textuelles, annonces dynamiques, responsive/zoom, tests automatisés et vérification manuelle."
---

# Accessibilité React / Next.js

## Périmètre

Analyser un diff, une page, un composant ou un parcours utilisateur. Si aucun
périmètre n'est donné, demander la page ou le composant à auditer.

## Sévérités

- **Bloquant** : parcours impossible au clavier, champ sans nom, focus perdu, action inaccessible, contenu critique non annoncé.
- **Important** : expérience dégradée pour lecteur d'écran, contraste insuffisant, erreur de formulaire mal associée.
- **Suggestion** : amélioration de confort, libellé plus clair, structure plus robuste.

## Points D'Analyse

- HTML sémantique : boutons, liens, headings, listes, landmarks.
- Navigation clavier : tab order, activation Entrée/Espace, Escape, raccourcis.
- Focus : visible, initial, retour après fermeture, pièges de focus.
- Labels et descriptions : nom accessible, `aria-describedby`, aide contextuelle.
- Formulaires : erreurs associées, résumé d'erreurs, `autoComplete`, required.
- Modales et menus : role adapté, focus trap, fermeture, état ouvert/fermé.
- Contraste : texte, icônes informatives, focus ring, états disabled.
- Alternatives textuelles : images, icônes, loaders, graphiques.
- Annonces dynamiques : loading, succès, erreurs, changements de contenu.
- Responsive et zoom : 200%, mobile, orientation, texte non tronqué.

## Tests Et Vérifications

- Utiliser Testing Library avec requêtes accessibles.
- Lancer axe, jest-axe, Storybook a11y ou Playwright a11y seulement si présents.
- Prévoir une vérification manuelle clavier et lecteur d'écran pour les patterns complexes.
- Ne pas se limiter à un score automatisé : vérifier le parcours réel.

## Ne Pas Faire

- Ne pas ajouter ARIA pour compenser un mauvais élément HTML si un élément natif convient.
- Ne pas supprimer outline/focus visible.
- Ne pas masquer une erreur seulement visuellement.
- Ne pas rendre un `div` cliquable sans rôle, tabIndex et gestion clavier.
- Ne pas introduire une librairie a11y sans confirmation.

## Format De Sortie

Présenter les findings par sévérité :

- **Fichier et ligne**
- **Sévérité**
- **Preuve**
- **Impact utilisateur**
- **Correction proposée**
- **Test ou vérification conseillé**
