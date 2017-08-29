# Fetch API

Fetch API est une méthode de ECMAScript6 => tous les navigateurs ne le supportent pas encore.
Pour l'utiliser il faut utiliser un _'polyfill'_ :  
> _"A polyfill, or polyfiller, is a piece of code (or plugin) that provides the technology that you, the developer, expect the browser to provide natively."_

Pour utiliser fetch API il faut inclure deux polyfills différents : un pour la méthode fetch et un pour les promises

[node-fetch npm](https://www.npmjs.com/package/node-fetch)  
install `npm install node-fetch --save`  
import `var fetch = require('node-fetch')` 

Fetch parameters : url (required), options (optional)

```
const fetch = require('node-fetch')

const url = 'https://swapi.co/api/people/1/'

fetch(url)
.then(response => response.json())
```

La méthode Fetch retourne un objet de promise, qui peuvent être utilisés avec des générateurs pour former des bases d'opérations asynchrones complexes. 
Chaque opération est asynchrone, et dépend de la valeur renvoyée par la précédente.  


## Promise

Promise => asynchrone = non bloquant !  

```
const url = 'https://swapi.co/api/people/1/'

fetch(url)
    .then(response => response.json)
    .then(data => console.log(data))
    .catch(error => console.log('ERROR', error))
```

La promsesse `fetch` va s'exécuter jusqu'au bout avant d'exécuter les `.then`
Dans le code suivant, les log sortiront dans l'ordre :
1. before fetch
1. after fetc
1. updateName call

**html body**
```
<div id="name">Loading...</div>
```
**script js**
```
const updateName = name => {
    document.getElementById('#name').innerText(name)
    console.log('updateName call')
}

console.log('before fetch')
fetch(url)
    .then(response => response.json())
    .then(data => updateName(data.name))
    .catch(error =>
        console.log('ERROR', error)
    )
console.log('after fetch')
```

On peut chainer les actions :
```
const processName = data => {
    return data.name.toUpperCase()
}

const updateName = name => {
    document.getElementById('#name').innerText(name)
}

fetch(url)
    .then(response => response.json())
    .then(data => processName(data))
    .then(name => updateName(name))
    .catch(error =>
        console.log('ERROR', error)
    )
```

Pour accéder aux données (les push dans un tableau) une fois que la promesse fetch a été exécutée :
```
const store = []

const success = data => store.push(data)

fetch(url)
    .then(response => response.json())
    .then(data => success(data))
    .catch(error =>
        console.log('ERROR', error)
    )
```

## Generator
[Itérateurs et générateurs - MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Guide/iterateurs_et_generateurs)
[ECMAScript 2015: Generators and Iterators](https://www.sitepoint.com/ecmascript-2015-generators-and-iterators/)
[Going Async With ES6 Generators](https://davidwalsh.name/async-generators)
[Asynchronous APIs Using the Fetch API and ES6 Generators](https://www.sitepoint.com/asynchronous-apis-using-fetch-api-es6-generators/)  


Les générateurs peuvent être utilisés pour continuer à donner les réponses jusqu'à ce que la réponse contienne des données.

install co `npm install co`
```
const co = require('co');
const fetch = require('node-fetch')

const url = 'https://swapi.co/api/people/1/'

const toJSON = function(response) {
  if (response.ok) {
    return response.json();
  } else {
    throw new Error(response.status);
  }
};

co(function*() {
  var data;
  data = (yield fetch(url, {}).then(toJSON));
  return console.log(data);
})["catch"](function(err) {
  return console.error(err);
});
```

## Observable RxJS
`
`import { Observable } from 'rxjs/Observable`

```
const url = 'https://swapi.co/api/people/1/'

const promise = fetch(url)

const source = Rx.Observable.fromPromise(promise)
                          .map(res => res.json())
                          .map(data => data.name.toUpperCase())
                          .subscribe(console.log)

```

```
const promise = fetch(url)

const source = Rx.Observable.fromPromise(promise)

const subscription = source.subscribe(
    function onNext(response) {
        console.dir(response.json())
        console.log('Response ', name)
    },
    function onError(error) {
        console.error('error ', error)
    },
    function onCompleted() {
        console.log('Completed !')
    }
)
```