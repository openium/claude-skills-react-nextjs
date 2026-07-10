---
name: forms
description: "Crée ou modifie des formulaires React/TypeScript/Next.js selon l'architecture existante. Couvre contrôlé/non contrôlé, validation client et serveur, validation runtime, Server Actions ou API, erreurs, focus, double soumission, loading/disabled, accessibilité, données sensibles et tests."
---

# Formulaires React / Next.js

## Périmètre

Créer, corriger ou améliorer un formulaire sans supposer React Hook Form, Zod,
Yup ou une autre bibliothèque si elle n'est pas déjà utilisée.

## État Des Lieux

Inspecter :

- Formulaires voisins et conventions de validation.
- Présence de React Hook Form, Formik, Zod, Yup, Valibot ou validators custom.
- Architecture de soumission : Server Actions, Route Handlers, API routes, client API.
- Gestion existante des erreurs, loading, disabled et focus.
- Tests de formulaires existants.

## Modèle De Formulaire

- Utiliser contrôlé ou non contrôlé selon la convention du projet.
- Éviter de stocker deux sources de vérité pour la même valeur.
- Préserver types TypeScript et contrats de payload.
- Ne pas exposer données sensibles côté client plus que nécessaire.

## Validation

- Garder validation serveur obligatoire pour données utilisateur.
- Ajouter validation client seulement comme aide UX.
- Utiliser une validation runtime si le projet en a déjà une ou si le boundary l'exige.
- Retourner des erreurs par champ et erreurs globales.
- Ne pas faire confiance aux types TypeScript pour valider une entrée runtime.

## Soumission Et États

- Empêcher double soumission si l'action n'est pas idempotente.
- Gérer loading et disabled sans bloquer la navigation clavier inutilement.
- Préserver les valeurs utiles après erreur.
- Distinguer erreur validation, erreur réseau et erreur serveur inattendue.
- Gérer succès, reset et redirection selon le parcours existant.

## Accessibilité Et Focus

- Associer label, description et message d'erreur au champ.
- Utiliser `aria-invalid`, `aria-describedby` si utile.
- Mettre le focus sur la première erreur ou fournir un résumé accessible.
- Ne pas remplacer bouton submit natif par un élément non sémantique.

## Next.js

- Server Actions si le projet les utilise déjà et que le formulaire s'y prête.
- API route ou Route Handler si c'est la convention existante.
- Ne pas migrer Pages Router vers App Router pour un formulaire.
- Ne pas déplacer secret ou validation sensible dans un Client Component.

## Tests

- Tester saisie utilisateur avec `userEvent`.
- Tester validation client si présente, erreurs serveur, loading et double submit.
- Tester le payload envoyé ou la Server Action via la stratégie existante.
- Ajouter test de régression pour bugfix.

## Ne Pas Faire

- Ne pas supposer React Hook Form ou une librairie de schémas.
- Ne pas valider seulement côté client.
- Ne pas exposer token, secret ou donnée sensible dans le DOM ou les logs.
- Ne pas désactiver un bouton sans message ou état perceptible.
- Ne pas supprimer les attributs natifs utiles (`type`, `required`, `autoComplete`) sans raison.

## Format De Sortie

Résumer :

- **Architecture du formulaire**
- **Validation client/serveur**
- **Gestion des erreurs et focus**
- **États loading/disabled**
- **Tests ajoutés ou proposés**
- **Risques ou décisions à valider**
