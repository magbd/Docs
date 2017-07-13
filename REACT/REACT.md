# Notes React

React est une bibliothèque qui permet de créer des composants réutilisables pour construire des interfaces graphiques. Elle peut être utilisée côté client ou côté serveur. Ce n'est pas un framework. Il n'y a pas de paradigme attenant à la technologie elle-même.

## Start projet
 
[Lien doc create-react-app](https://github.com/facebookincubator/create-react-app)

``` 
npm install -g create-react-app
```

```
create-react-app my-app
cd my-app/
npm start
```

## Getter

Pr ne pas répéter ds chaque méthode une const qui récupère les props :
```jsx
const { data } = this.props 	//après on utilise direct data
```
On déclare un getter _(méthode)_ :
```jsx
get data() { 
return this.props.data 
}				//après on peut utiliser this.data dans les méthodes
```
 
## Component

### 1 Component stateless

```jsx
const componentName = (props) => {

  function renderSomething() {
    return // ...
  }

  return (
    <div>
      {renderSomething()}
    </div>
  )

}
```

### 2 Component stateful (state local et/ou store pour passer les props)

```jsx
class componentName extends React.Component {

  renderSomething() {
      return // ...
    }

  render() {
    return (
      <div>
        {this.renderSomething()}
      </div>
    )
  }

}
```

#### 2.1 class constructor
```jsx
constructor (props) {
	super (props)
	this.state = { data: [ ] }
}
```

#### 2.2 lifecycle method  

`componentWillMount()`      = insertion DOM  
`componentDidMount()`       = inséré dans le DOM  
`componentWillUnmount()`    = retire du DOM

```jsx
componentWillMount() {
	this.state = { data: [ ] }
}
```

>On peut utiliser WillUnmount pour “détruire” l’état du component, un eventListner …  
Example avec un listener qui écoute le changement de dimension de la fenêtre :

```jsx
componentWillMount() {
    this.state = {
        responsiveMenu: this.isResponsiveMenu(),
        navigationCollapse: false
    }
    window.addEventListener(“resize”, this.handleResize)
}

componentWillUnmount() {
    window.removeEventListener(this.handleResize)
}

handleResize = () => { 	//fonction fléchée pour ne pas binder le this au state
    if (window.innerWidth < 1020  && !this.state.responsiveMenu) {
	    this.setState({ responsiveMenu: false })
    }
    if (window.innerWidth > 1020  &&  this.state.responsiveMenu) {
	    this.setState({ responsiveMenu: true })
    }
}

isResponsiveMenu = () => {
    return window.innerWidth < 1020
}
```

 ## Compose

Permet de combiner des fonctions à l'export du component.
_Exemple :_ connect + onClickOutside

```jsx
import { connect } from 'react-redux'
import onClickOutside from 'react-onclickoutside'
import { compose } from 'recompose'
  ...
export default compose(
  connect(mapStateToProps, mapDispatchToProps),
  onClickOutside
)(ComponentName)
```

 ## Types

### Proptypes

```jsx
import React from 'react'
import PropTypes from 'prop-types'

const component = ({ props1, props2, props3 }) => {

  return (
    <div>
    </div>
  )
}

component.propTypes = {
   props1: PropTypes.string,
   props2: PropTypes.number,
   props3: PropTypes.boolean
}

export default DropdownMenuItem
```

### Flow

[Doc Flow](https://flow.org/)

Les types possibles :
* `?variableValue` => maybe
* `any`
* `Function`
* `string`
* `number`
* `boolean`

1. Typage direct dans le fichier


2. Typage des props dans un fichier dédié

`component.js`
```jsx
//@flow
import React from 'react'

const component = ({ props1, props2, props3 }: componentProps) => {

  return (
    <div>
    </div>
  )
}
```

`componentProps.js`
```jsx
// @flow
export type DropdownMenuItemProps = {
  props1: string,
  props2: string,
  props3: string
}
```

## Plugin react

* onclickoutside
* fontawesome
* translate
* Velocity

* Redux Form