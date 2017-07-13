# React - Unit Testing

## Start
`npm i --save-dev jest`     [Doc](https://facebook.github.io/jest/docs/en/getting-started.html)  
`npm i --save-dev enzyme`   [Doc](http://airbnb.io/enzyme/index.html)

Par défaut, Jest cherche les tests dans un dossier nommé __tests

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
Accéder à une classe particulière du DOM qui se répète ( [cf Doc enzyme](http://airbnb.io/enzyme/docs/api/ShallowWrapper/childAt.html) ) :
* `.at(0index)`
* `.childAt(index)`
* `first`
* ...

## 2. Tester les props

_Exemple :_
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
    expect(wrapper.find('.Component--label').text()).toEqual(props.label) 
    // => <div className="DropdownMenuItem--label" >{label}</div>
    expect(wrapper.find('.Component--subLabel').text()).toEqual(props.subLabel)
    expect(wrapper.find(FontAwesome).prop('name')).toEqual(props.icon) 
    // => <FontAwesome className="DropdownMenuItem--icon" name={icon} />
  })

})
```

## 3. Tester un changement d'état, simuler une action/event

`.simulate(event)` permet de simuler le déclenchement d'un event
* `onClick` => `.simulate('click')`
* `onChange` => `.simulate('change')`

_Exemple :_
```javascript
test('Toggle Dropdown', () => {
    const wrapper = shallow(<RightToolbar {...props}/>)
    wrapper.find('.RightToolbar--dropdown').first().simulate('click') // simule le click
    expect(
      wrapper.find('.RightToolbar--dropdown').first().prop('className')
    ).toEqual('RightToolbar--dropdown open') // vérifie que la classe se modifie (classeName dynamique relié au state)
    expect(
      wrapper.state('openDropdown')
    ).toBeTruthy() // vérifie que le changement du state est bien passé à true
    wrapper.find('.RightToolbar--dropdown').first().simulate('click')
    expect(
      wrapper.find('.RightToolbar--dropdown').first().prop('className')
    ).toEqual('RightToolbar--dropdown close')
    expect(
      wrapper.state('openDropdown')
    ).toBeFalsy() // vérifie que le changement du state est bien passé à false
  })
```

## 4. Tester le store

Importer le reducer à tester avec son INITIAL_STATE, et les actions qui permettent d'interagir avec le reducer

```jsx
import userReducer, { INITIAL_STATE } from './user.reducer'
import {
  FETCH_USER_REQUEST,
  FETCH_USER_SUCCESS,
  FETCH_USER_FAILURE,
  SELECT_PLATFORM
} from './user.actions'
```

Tester le chaque état lors du chargement de la data (REQUEST, SUCCESS, FAILURE)
```jsx
describe('User reducer test', () => {

  test('Fetch user request', () => {
    expect(userReducer(INITIAL_STATE, { type: FETCH_USER_REQUEST })).toEqual({
      ...INITIAL_STATE,
      isFetching: true
    })
  })

  test('Fetch user success', () => {
    const user = {
      id: 0,
      platforms: []
    }
    const action = { type: FETCH_USER_SUCCESS, payload: user }
    expect(userReducer(INITIAL_STATE, action)).toEqual({
      ...INITIAL_STATE,
      userData: user
    })
  })

  test('Fetch user failure', () => {
    const action = {type: FETCH_USER_FAILURE, payload: 'ERROR' }
    expect(userReducer(INITIAL_STATE, action)).toEqual({
      ...INITIAL_STATE,
      error: 'ERROR'
    })
  })

})
```

Tester le résultats des autres actions

```jsx
test('Select platform', () => {
    const state = {
      ...INITIAL_STATE,
      platforms: [
        { id: 1, name: 'tabmo', organizations: [ { id: 11, name: 'orga' } ] },
        { id: 2, name: 'tabmo-test', organizations: [ { id: 22, name: 'orga' } ] }
      ]
    }
    const action = { type: SELECT_PLATFORM, payload: 1 }
    expect(platform(state, action)).toEqual({
      ...state,
      selectedPlatform: { id: 1, name: 'tabmo', organizations: [ { id: 11, name: 'orga' } ] },
      selectedOrganization: { id: 11, name: 'orga' }
    })
  })

  test('Select organization', () => {
    const state = {
      ...INITIAL_STATE,
      selectedPlatform: {
        id: 1,
        name: 'tabmo',
         organizations: [ { id: 11, name: 'orga' }, { id: 12, name: 'agro' } ] }
    }
    const action = { type: SELECT_ORGANIZATION, payload: 12 }
    expect(platform(state, action)).toEqual({
      ...state,
      selectedOrganization: { id: 12, name: 'agro' }
    })
  })
```
## Debug  

Affiche dans le terminal le DOM virtuel qui est monté par le component wrappé

```jsx
console.log(wrapper.debug())
```