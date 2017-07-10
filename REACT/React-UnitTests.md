# React - Unit Testing

## Start
`npm i --save-dev jest`     [Doc](https://facebook.github.io/jest/docs/en/getting-started.html)  
`npm i --save-dev enzyme`   [Doc](http://airbnb.io/enzyme/index.html)

* importer React et la méthode shallow de Enzyme
* importer le module à tester
* `describe` la méthode prend 2 paramètres : nom du le module testé puis fonction anonyme qui exécute le/les tests
* `test` _(ou it ...)_ la méthode prend 2 paramètres : nom du test (render du module, test function, ...) puis fonction anonyme
* déclarer `const wrapper` qui applique la méthode shallow au component
* vérifier les résultats attendus
  * `expect(myVar).toHaveLength(...)`     => test la présence
  * `expect(maVar).toBe(...)`             => comparaison stricte
  * `expect(myVar).toEqual(...)`          => test d'équivalence
  * `expect(myVar).toBeDefined()`         => variable définie (contraire: toBeUndefined)
  * ...


## 1. Tester la présence des éléments

Tester la présence des classes permet de vérifier que les éléments attendus sont bien montés par le component.

```jsx
import React from 'react'
import { shallow } from 'enzyme'

import { Component } from 'modules/component'
import { SubComponent } from 'modules/component'

describe('Component tests', () => {
    test('Component render', () => {
        const wrapper = shallow(<Component/>)
        expect(wrapper.find('.Component')).toHaveLength(1) //test class
        expect(wrapper.find(SubComponent)).toHaveLength(2) // test présence d'un autre component intégré
    })
})
```

## 2. Tester les props

```jsx
import { Component } from 'modules/component'
import FontAwesome from 'react-fontawesome'

describe('Component tests', () => {

  test('Component has props', () => {
    const props = {
      label: 'Label',
      subLabel: 'SubLabel',
      icon: 'Icon'
    }
    const wrapper = shallow(<Component {...props} />)
    expect(wrapper.find('.Component--label').text()).toEqual(props.label) // => <div className="DropdownMenuItem--label" >{label}</div>
    expect(wrapper.find('.Component--subLabel').text()).toEqual(props.subLabel)
    expect(wrapper.find(FontAwesome).prop('name')).toEqual(props.icon) // => <FontAwesome className="DropdownMenuItem--icon" name={icon} />
  })

})
```

## 3. Tester les fonctions



## Debug  

Affiche dans le terminal le DOM virtuel qui est monté par le component wrappé

```jsx
console.log(wrapper.debug())
```