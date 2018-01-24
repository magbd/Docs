````
const getInitials = (name) => name.split(" ").map((n)=>n[0]).join('').toUpperCase()
getInitials('my name')
```
Version ramda :  
```
const getInitials = compose(toUpper, join, map(head), split(" "))
getInitials('my name')
```