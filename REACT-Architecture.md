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

# Components
core ou common = modules bas niveau  
=> vue uniquement. Pas de reducers  
Aucune logique métier contenue (ex: button)

# Modules
/!\ dépendances cycliques (sortie erreur console => import undefined)  
Un module ne doit pas dépendre d’un autre module.

# Model.js
=> méthodes utilitaires (services)   
Fonctions pures.

# Selectors.js
=> calcul des valeurs depuis l’état.  
Envoi la donnée traitée triée au component.

````
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

