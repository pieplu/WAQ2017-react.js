# I - Présentation

React est une Bibliotheque JavaScript permettant de gérer son UI sous forme
de composant.

## Sommaire

* [Historique](#historique)
* [Pourquoi React](#pourquoi-react)
* [Constats](#constats)
* [Propositions](#propositions)

## Historique

React est une bibliothèque JavaScript crée par Jordan Walke (un dev de chez facebook) ~ 2010/2011.
Elle a été utilisé sur Facebook (2011) puis Instagram (Pete Hunt).
React a été rendu Open-Source en 2013, la version actuelle est la 15.x.
En dehors de Facebook, React est utilisé en production par : Airbnb, Netflix, Feedly, Brackets...

## Pourquoi React

React a été crée dans le but de réaliser des applications avec des données qui changent au fil du temps.

### Constats

- Maintenir une application avec des données qui changent tout le temps :
    - C'est compliqué (Codebase qui grossit à vue d'oeil, maintenabilité du code qui devient compliqué)
    - Manipuler le DOM à outrance, ça coute cher en performance.

### Propositions

- Abstraire son interface sous forme de composants
    - Un composant peut-être, un input, un bouton, un select...
    - Les composants possèdent des états (State, lifecycle)

- Virtual DOM
    - Fait une représentation virtuelle du DOM et à chaque modification d'un component un diff est fait entre l'ancienne représentation du dom et la nouvelle.
    De cette manière la manipulation du DOM est limité au strict nécéssaire (Pas besoin de modifier cinq paragraphe si seul un mot change)

- One-way data flow
- JSX
