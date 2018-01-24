```
const getItems = (routeName: string) => {
  const current: CrumbRoute = crumbs[routeName]
  return current.parent ? [ ...getItems(current.parent), current] : [current]
}
```

```
const getItems = (routeName: string) => {
  const current: CrumbRoute = crumbs[routeName]
  return current.parent.cata({
    Nothing: () => [current],
    Just: name => [ ...getItems(name), current]
  })
}
```