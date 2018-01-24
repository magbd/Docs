# React Router

[Doc ReactRouter](https://reacttraining.com/react-router/web/guides/philosophy)

## Accéder à l'objet `History`

On accède à l'objet `history` par le [contexte](https://facebook.github.io/react/docs/context.html).
Il faut donc importer les Prop-Types.

### 1. Composant stateless
> Le contexte vient en second argument, après les props
Avec destructuration :
```
import PropTypes from 'prop-types'

const CreativeCover = ({ creative }: Props, { router: { history } }: Object) => {

    const handleCreativeDetail = () =>
        history.push(creative.id)
    
    return (
        //...
    )
}
CreativeCover.contextTypes = {
  router: PropTypes.object
}
export default CreativeCover
```

ou sans desctructuration :

```
import PropTypes from 'prop-types'

const CreativeCover = ({ creative }: Props, context) => {

    const handleCreativeDetail = () =>
        context.router.history.push(creative.id)

    return (
        //...
    )
}
CreativeCover.contextTypes = {
  router: PropTypes.object
}
export default CreativeCover
```

### 2. Class
```
import PropTypes from 'prop-types'

export class RightToolbar extends Component<Props, State> {

  redirectToCreativeList = () =>
    this.context.router.history.replace('/creatives/list')

    render() {
        return(
            // ...
        )
    }
}

RightToolbar.contextTypes = {
  router: PropTypes.object
}

export default compose(
  // ...
)(RightToolbar)
```