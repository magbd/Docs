# React - Architecture projet

## Vue d'ensemble

```
src/
├── components/ (common = DUMB)  
|   ├── SomeComponent.css
|   ├── SomeComponent.js
|   └── ...
├── helpers/ (UTILITIES méthodes utilitaires)
|   ├── someHelper.js
|   └── ...
├── media/
|   ├── someMedia.png
|   └── ...
├── modules/ (FEATURES) 
|   ├── someFeature/
|   |   ├── components/
|   |   |   ├── SomeFeatureComponent.css
|   |   |   ├── SomeFeatureComponent.js
|   |   |   └── ...
|   |   ├── actions.js
|   |   ├── index.js
|   |   ├── models.js
|   |   ├── reducer.js
|   |   ├── sagas.js
|   |   ├── selectors.js
|   |   └── types.js
|   └── ...
├── services/ (fonctionnel bas niveau, “tuyauterie” => Auth.js, api.js ...)
|   ├── someService.js
|   └── ...
├── types/
|   └── types.js
├── App.jsx
├── configureStore.js
├── index.css
└── index.js
```

* `components/` regroupe les composants partagés sans logique métier (button, card...)
* `helpers/` regroupe des méthodes utilitaires sans logique métier (object, array...)
* `media/` regroupe des fichiers media divers (essentiellement des images)
* `modules/` est découpé en [sous-dossiers](#anatomie-dun-module) regroupant chacun les composants et la logique métier propres à une fonctionnalité
* `services/` regroupe les services permettants d'effectuer des effets de bords (http, storage...)
* `types/types.js` déclare et exporte quelques types communs à toute l'application
* `App.jsx` déclare le composant applicatif racine
* `configureStore.js` déclare et exporte la configuration du [store](http://redux.js.org/docs/basics/Store.html)
* `index.css|js` servent de points d'entrées de l'application

## Components
core ou common = modules bas niveau  
=> vue uniquement.  
Pas de reducers  
Aucune logique métier contenue (ex: button)
>_Conventions :_
>* les composants ne doivent rien importer en provenance des dossiers `modules/` et `services/`
>* les fichiers `.js` et `.css` d'un composant doivent être disposés à plat à la racine du dossier
>* les tests unitaires doivent se trouver dans un sous-dossier `__tests__/` avec l'extension `.test.js`

## Modules
/!\ dépendances cycliques (sortie erreur console => import undefined)  
Un module ne doit pas dépendre d’un autre module.
>_Conventions :_
>* doivent impérativement posséder un fichier `index.js` servant de point d'entrée du module
>* ne doivent pas importer les fonctionnalités d'autres modules sans passer par le fichier `index.js` de ces modules
>* peuvent posséder un dossier `components/` regroupant tous les composants liés à la fonctionnalité du module
>* peuvent être en charge d'un domaine de l'état Redux et doivent à ce titre posséder au minimum les fichiers `reducer.js` et `types.js` (optionnellement  les fichiers `actions.js`, `actionTypes.js`, `model.js`, `saga.js` et `selectors.js`)

### Anatomie d'un module

```
someFeature/
├── components/
|   ├── SomeFeatureComponent.css
|   ├── SomeFeatureComponent.js
|   └── ...
├── actions.js
├── index.js
├── models.js
├── reducer.js
├── sagas.js
├── selectors.js
└── types.js
```

* `components/` (optionnel) regroupe les composants
* `actions.js` (optionnel) déclare et exporte les [action creators] (http://redux.js.org/docs/basics/Actions.html) ainsi que les valeurs possibles pour le  champ `type`
* `index.js` (obligatoire) sert de point d'entrée du module
* `models.js` (optionnel) déclare et exporte des méthodes utilitaire permettant de manipuler les données
* `reducer.js` (optionnel) déclare et exporte le [reducer](http://redux.js.org/docs/basics/Reducers.html)
* `sagas.js` (optionnel) déclare et exporte les [sagas](https://redux-saga.js.org/)
* `selectors.js` (optionnel) déclare et exporte les [selectors](http://redux.js.org/docs/recipes/ComputingDerivedData.html)
* `types.js` (optionnel) déclare et exporte des types

## Model.js
=> méthodes utilitaires (services)   
Fonctions pures.

## Selectors.js
=> calcul des valeurs depuis l’état.  
Envoi la donnée traitée triée au component.

```
src/
├── components/
|   ├── SomeComponent.css
|   ├── SomeComponent.js
|   └── ...
└── helpers/
|   ├── someHelper.js
|   └── ...
└── media/
|   ├── someMedia.png
|   └── ...
└── modules/ 
|   └── someFeature/
|   |   ├── components/
|   |   |   ├── SomeFeatureComponent.css
|   |   |   ├── SomeFeatureComponent.js
|   |   |   └── ...
|   |   ├── actions.js
|   |   ├── constants.js
|   |   ├── index.js
|   |   ├── model.js
|   |   ├── reducer.js
|   |   ├── saga.js
|   |   ├── selectors.js
|   |   └── types.js
|   └── ...
└── services/
|   ├── someService.js
|   └── ...
├── index.css
├── index.js
├── store.js
├── routes.js
└── types.js
```

## Module en détail

* le dossier `components/` _(optionnel)_ regroupe les composants
* le fichier `actions.js` _(optionnel)_ déclare et exporte les action creators Redux ansi que les valeurs possibles pour le  champ type
* le fichier `constants.js` _(optionnel)_ déclare et exporte des constantes
* le fichier `index.js` _(obligatoire)_ sert de point d'entré du module
* le fichier `model.js` _(optionnel)_ déclare et exporte des méthodes utilitaire permettant de manipuler les données
* le fichier `reducer.js` _(optionnel)_ déclare et exporte le reducer  Redux
* le fichier `saga.js` _(optionnel)_ déclare et exporte la root saga Redux
* le fichier `selectors.js` _(optionnel)_ déclare et exporte les selectors Redux
* le fichier `types.js` _(optionnel)_ déclare et exporte des types

## Exemple de fichier `index.js` :

```jsx
// @flow

import * as actions from './actions'
import * as actionTypes from './actionTypes'
import reducer from './reducer'
import SomeComponent from './components/SomeComponent'

export default {
  actions,
  actionTypes,
  reducer,
  SomeComponent
}
```

1. Importer les actions, reducers, types, components ...  
2. Exporter chaque import pour les rendre accessibles  
3. /!\ Les exports seront accessibles tels que :   

```jsx
import { SomeComponent } from './modules/ModuleFeature'
```
