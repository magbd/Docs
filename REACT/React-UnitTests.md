# React - Unit Testing

## Start
install Jest
install Enzyme

## Example
```jsx
import React from 'react'
import { shallow } from 'enzyme'

import { DropdownMenu } from 'modules/routing'

describe('DropdownMenu tests', () => {
    test('DropdownMenu render', () => {
        const wrapper = shallow(<DropdownMenu/>)
        expect(wrapper.find('.DropdownMenu--flexContainer')).toHaveLength(1)
        
    })
})
```

## Debug  

Affiche dans le terminal le DOM virtuel qui est monté par le component wrappé

```jsx
console.log(wrapper.debug())
```