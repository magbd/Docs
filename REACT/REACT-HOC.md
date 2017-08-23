# React : Higher-Order Components

[Doc](https://facebook.github.io/react/docs/higher-order-components.html)

## Partager un héritage

**helpers/authenticate.jsx**
```
import React, { Component } from 'react'

type StatelessComponent<P, C> = (props: P, context: C) => ?React$Element<any>
type StatefulComponent<D, P, S> = Class<React$Component<D, P, S>>
type ReactGenericComponent<F, S, T> = StatelessComponent<F, S> | StatefulComponent<F, S, T>


export default function requireAuth<F, S, T>(WrappedComponent: ReactGenericComponent<F, S, T>) {
  return class AuthenticatedRoute extends Component {

    componentWillMount() {
      const { auth, location, history } = this.props
      auth.parseHash(location.hash, history)
      if (!auth.isLoggedIn()) {
        history.push('/login')
      }
    }

    render() {
      return <WrappedComponent { ...this.props }/>
    }
  }
}
```
Et après utiliser dans les components :  
`import authenticate from 'helpers/authenticate'`
`export default authenticate(MyComponent)`


## Créer un pattern

**helpers/withTransition.jsx**
```
import React from 'react'
import TransitionGroup from 'react-transition-group/TransitionGroup'

type StatelessComponent<P, C> = (props: P, context: C) => ?React$Element<any>
type StatefulComponent<D, P, S> = Class<React$Component<D, P, S>>
type ReactGenericComponent<F, S, T> = StatelessComponent<F, S> | StatefulComponent<F, S, T>

export default function withTransition<F, S, T>(WrappedComponent: ReactGenericComponent<F, S, T>) {
  return class extends React.Component {
    render() {
      return (
        <TransitionGroup>
          { this.props.isOpen ? <WrappedComponent {...this.props}/> : null }
        </TransitionGroup>
      )
    }
  }
}
```
Et après utiliser dans les components :    
`import authenticate from 'helpers/withTransition'`
`export default withTransition(MyComponent)`  

****

_fonction simplifiée_

```
const withTransition = () => {
  return class extends React.Component {
    render() {
      return (
        <TransitionGroup>
          { this.props.isOpen ? <WrappedComponent {...this.props}/> : null }
        </TransitionGroup>
      )
    }
  }
}
export default withTransition(MyComponent)
```