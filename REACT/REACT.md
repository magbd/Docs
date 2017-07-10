# Notes React

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

_Dans un component class_  
Pr ne pas répéter ds chaque méthode une const qui récupère les props :
```javascript
const { data } = this.props 	//après on utilise direct data
```
On déclare un getter _(méthode)_ :
```javascript
get data() { 
return this.props.data 
}				//après on peut utiliser this.data dans les méthodes
```
 
## Déclarer un state local ds un component

### - class constructor
```javascript
constructor (props) {
	super (props)
	this.state = { data: [ ] }
}
```

### - lifecycle method  

`componentWillMount()`      = insertion DOM  
`componentDidMount()`       = inséré dans le DOM  
`componentWillUnmount()`    = retire du DOM

```javascript
componentWillMount() {
	this.state = { data: [ ] }
}
```

>On peut utiliser WillUnmount pour “détruire” l’état du component, un eventListner …  
Example avec un listener qui écoute le changement de dimension de la fenêtre :

```javascript
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