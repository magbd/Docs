# Notes JS / ES6

## let / const

`const` => valeur constante, non modifiable. /!\ si objet, les valeurs sont modifiables  
`let` => modifiable /!\ sa portée de limite à son scope
 
## Templates string

Concatène texte, variable, balises.
Possibilité d'écrire directement une fonction ternaire dans `${ }`

```javascript
const concatTexte = `blabla bla ${maVariable} blabla bla ${autreVarBoolen ? ‘yes’ : ‘no’ } fin.`
```
 
## … spread operator

```javascript
var eleves = [‘Marc’, ‘Toto’]
var newEleves = [ ...élèves, ‘Paul’]
// newEleves = [‘Marc’, ‘Toto’, ‘Paul’]
```

## .concat(elem)

Assemble plusieurs tableau en 1 tableau (=> … spread)  
Concatène l’elem passé en param au tableau sur lequel est appliqué la méthode (copie)

```javascript
var arr1 = [a, b, c]
var arr2 = [d, e, f]
var newArray = arr1.concat(arr2)
// newArray = [a, b, c, d, e, f]
```
 
## .includes(elem)

Vérifie présence de l’elem passé en param et retourne true/false

```javascript
[a, b, c].includes(b)   // true
[a, b, c].includes(z)	// false
```
 
## .filter( (valeurCourante, index) => callback )

Itère sur chaque elem et filtre selon la condition passée en callback.  
Prends la valeur courante et l’index en paramètres. => Possibilité de récupérer l’index.

```javascript
var nombres = [1, 2, 3, 8]
var pair = nombres.filter( elem => elem%2 == 0)
// pair = [2, 8]
```
 
## .map( (valeurCourante, index) => callback )

Itère sur chaque elem(valeurCourante). 
Prends la valeur courante et l’index en paramètres. => Possibilité de récupérer l’index.  
La fonction callback permet de modifier la valeurCourante (renvoi une valeur de retour, copie de la valeur courante modifiée) ou de la retourner (ainsi que l’index)

```javascript
var arr1 = [1, 2, 3]
var newArr = arr1.map( elem => elem + 2 )
// newArray = [3, 4, 5]
```

## .reduce((acc, current) => callback, valeurInitiale)

Applique une fonction qui est un « accumulateur » et qui traite chaque valeur d'une liste (de la gauche vers la droite) afin de la réduire à une seule valeur.

_Exemple 1:_
```javascript
const somme = [0, 1, 2, 3].reduce( (acc, current) => {
    return acc + current
}, 0)
// somme = 6
```

_Exemple 2:_
```javascript
const orders = [
    {amount: 200},
    {amount: 50},
    {amount: 100}
    ]
const totalAmount = orders.reduce( (sum, order) => sum + order.amount, 0 )
// totalAmount = 350
```

_Exemple 3:_
```javascript
const array = [11, 3, 5, 8, 5, 2, 11]
const isInArray = array.reduce( (acc, current) => {
   if ( !acc.includes(current) ) {
       acc.push(current)
		}
    return acc
} , [])
// isInArray = [11, 3, 5, 8, 2]
```
