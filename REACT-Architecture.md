# React - Architecture projet

```
src  
|__ components/ (common = DUMB)  
|__ modules/ (FEATURES)  
|__ helpers/ (UTILITIES méthodes utilitaires)  
|__ services/ (fonctionnel bas niveau, “tuyauterie” => Auth.js, api.js ...)
|
├── index.js
├── store.js
├── routes.js   
```

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

## Exemple de fichier `index.js` (complet) :

```jsx
// @flow

import * as actions from './actions'
import * as actionTypes from './actionTypes'
import * as constants from './constants'
import * as model from './model'
import reducer from './reducer'
import saga from './saga'
import * as selectors from './selectors'

import MyFeatureComponent from './components/SomeFeatureComponent'

export default {
  actions,
  actionTypes,
  constants,
  components: { SomeFeatureComponent },
  model,
  reducer,
  saga,
  selectors
}
```