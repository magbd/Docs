# Les composants

* [Composants fonctionnels vs class](#composants-fonctionnels-vs-class)
* [Arrow functions vs méthodes de class](#arrow-functions-vs-méthodes-de-class)
* [Déclaration des props](#déclaration-des-props)
* [Les imports](#les-imports)
* [Connexion au store Redux](#connexion-au-store-redux)
* [Conventions](#conventions)

## Composants fonctionnels vs Class

TODO: ajouter les méthodes/arrow function à utiliser selon le type de compo

### Le composant fonctionnel (*stateless*)

C'est la forme la plus simple pour créer un composant.
Il s'agit d'une fonction qui prend en argument un objet props et rend du contenu sous forme de JSX.

```jsx
// @flow
import * as React from 'react'

const ComponentName = (props) => {

  const renderSomething() {
    return // ...
  }

  return (
    <div>
      {renderSomething()}
    </div>
  )

}

export default ComponentName
```

### Le composant à état (ou *stateful*)

Il s'agit d'une classe recevant des props et possédant obligatoirement une méthode `render()` retournant du JSX.
Le composant se construit ainsi si il remplit l'une des conditions suivantes :

* Mon composant possède un état local pour générer son rendu
* Mon composant utilise des méthodes du cycle de vie

> Ce state correspond à l’état interne de notre composant, contrairement aux props venant du composant parent ou du store *Redux*. Le state est modifiable en invoquant la fonction `setState()`

[Plus d'infos sur l'utilisation du state et des cycles de vies.](https://reactjs.org/docs/state-and-lifecycle.html) 

```jsx
// @flow
import * as React from 'react'

export class ComponentName extends React.Component<Props, State> {

  state = {}

  componentDidMount() {}

  renderSomething() {
      return // ...
    }

  render() {
    return (
      <div className='ComponentName'>
        {this.renderSomething()}
      </div>
    )
  }

}

export default ComponentName
```

## Arrow functions vs méthodes de class

Pour lier simplement une fonction à une classe sans passer par `this.handleClick = this.handleClick.bind(this)` dans le constructeur, il convient d'utiliser la syntaxe suivante :

```jsx
class Button extends React.Component {

  handleClick = () => {
    // do something ...
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    )
  }
}
```

Toutefois, il ne faut pas généraliser les `arrows functions` sur toutes les méthodes de la class pour des raisons de performances. 

[Plus d'infos sur les `handling events` en React](https://reactjs.org/docs/handling-events.html)


## Déclaration des props

### Destructuration

* Composant fonctionnel

```js
const ComponentName = ({ someValue, callback, children }) => {}
```

* Class composant

```js
export class ComponentName extends React.Component<Props, *> {
  render() {
    const { someValue, callback, children } = this.props
    return // ...
  }
}
```
### Typage

* Les props sont immutables. Elles sont annotées `$ReadOnly<{}>`
* Certaines props peuvent être optionnelles `props = { name?: string }`
* Pour optimiser la couverture *Flow* les props sont groupées par origine et annotées spécifiquement.

Exemples de déclarations des props d'un composant:

```jsx
// props du parent
type PublicProps = $ReadOnly<{}>

// props du store Redux
type SelectorProps = $ReadOnly<{}>

// actions du store Redux
type DispatchProps = $ReadOnly<{}>

// union type des props
type Props = PublicProps & SelectorProps & DispatchProps
```

Exemples d'annotations des types de props:

```jsx
// props globales du composant
export const ComponentName = (props: Props) => {}
```
```jsx
// props du store Redux
const withRedux = connect(
  (state) => ({}: SelectorProps),
  dispatch => ({}: DispatchProps)
)
```
```jsx
// props attendues du parent
export default ((ComponentName): React.ComponentType<PublicProps>)
```

## Les imports

Les imports s'organisent selon l'ordre suivant:

1. Les `dépendances`
2. Les `actions`, `selectors` et `components` internes ou externes au module
3. Les méthodes utilitaires issues du dossier `helpers` ou du fichier `model`
4. Les `types`
5. Le style `css` du composant

Pour plus de lisibilité, séparer les imports de dépendances et les types en sautant une ligne.

## Connexion au store *Redux*

Pour injecter le store *Redux* dans notre composant, il convient d'utiliser le *high-order-component* [`connect()`](https://redux.js.org/recipes/writing-tests#connected-components) de la librairie [React Redux](https://github.com/reduxjs/react-redux). 

```jsx
import { connect } from 'react-redux'
```

Ce conteneur possède deux fonctions *Redux* :

* mapStateToProps : prend le state en argument et le transmet au composant sous forme de propriétés,
* mapDispatchToProps : passe des fonctions en propriétés du composant et prend en paramètre la fonction `dispatch` de *Redux* chargée d'informer le store qu'un changement d'état

[Plus d'infos sur les paramètres de `connect()`](https://github.com/reduxjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options)

```jsx
const mapStateToProps = (state) => ()

const mapDispatchToProps = (dispatch) => ()
```

La connexion du `HOC` se fait à l'`export default`, en bas du composant.

```jsx
export default connect(mapStateToProps, mapDispatchToProps)(ComponentName)
```

* importer le store seul
```jsx
export default connect(mapStateToProps)(ComponentName)
```

* dispatcher seulement les actions
```jsx
export default connect(null, mapDispatchToProps)(ComponentName)
```

### Détail d'une connexion

```jsx
const withRedux = connect(
  (state) => ({
    reduxStoreData: moduleSelector.selectorName(state)
  }: SelectorProps),
  dispatch => ({
    reduxStoreAction: () => dispatch(moduleActions.actionName()),
  }: DispatchProps)
)
```


## Conventions

* le fichier doit être nommé en *UpperCamelCase*
* les props, constantes et fonctions sont nommées en *lowerCamelCase*
* la première balise du composant contient une `className` avec son nom pour permettre de l'identifier dans le DOM
* le composant doit posséder des annotations *Flow*
* les props sont **immutables**
* les [fonctions sont pure](https://fr.wikipedia.org/wiki/Fonction_pure)
* les `actions` et `selectors` importés d'autres modules sont préfixés
* les tests unitaires doivent se trouver dans un sous-dossier du module `__tests__/` avec l'extension `.test.js`
