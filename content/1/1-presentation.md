## Point rapide ES6

---

### ES6

* Nouvelle version de language JavaScript. <!-- .element: class="fragment" -->
* Sortie en Juin 2015. <!-- .element: class="fragment" -->
* Ajoute pas mal de nouveautés : Classes, Modules, Generateurs, fonctions flécheés... <!-- .element: class="fragment" -->

---

### Let et Const

---

#### Const

* Const ne peut être assigné qu'une seule fois. <!-- .element: class="fragment" -->
* Il a pour portée le bloc. <!-- .element: class="fragment" -->

---

##### Exemples :

```javascript
const toto = 'hello';

function sayHello() {
    toto = 'titi';
    console.log(toto);
}

sayHello();

// => Uncaught TypeError: Assignment to constant variable.
```

Note: Normal, on veut ré-assigner la valeur de toto dans la function sayHello

---

##### Exemples :

```javascript
const toto = 'hello';

function sayHello() {
    const toto = 'titi';
    console.log(toto);
}

console.log(toto);
sayHello();

// => 'hello'
// => 'titi'
```

Note: Dans ce cas on a ré-assigner const toto mais dans le scope de la function.

---

#### Let

* Let c'est un peu le nouveau var, sauf qu'il est scopé au bloc. <!-- .element: class="fragment" -->
* Il fait sensiblement la même chose que const mais sa valeur peu lui être ré-assignée. <!-- .element: class="fragment" -->

---

##### Exemples :

```javascript
let toto = 'Hello';
var toto2 = 'Hello again';

if (true) {
    let toto = 'new hello';
    // Ici la nouvelle assignation va écraser toto2 défini au dessus
    var toto2 = 'new hello again';

    console.log(toto);
    console.log(toto2);
}

console.log(toto);
console.log(toto2);

// => new hello
// => new hello again
// => Hello
// => new hello again
```

---

### Les classes

* En fait ça ajoute aucune fonctionnalité par rapport a ES5. <!-- .element: class="fragment" -->
* Par contre la syntaxe est bien plus facile à lire <!-- .element: class="fragment" -->

---

##### Classe es5 :

```javascript
// ES5

function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}

Person.prototype.sayHello = function() {
    return 'Hello i am ' + this.firstname + ' ' + this.lastname;
}

var barnabe = new Person('Barnabé', 'Zebear');

console.log(barnabe.sayHello());
// => Hello i am Barnabé Zebear
```

---

##### Classe es6 :

```javascript
// ES6

class Person {
    constructor(firstname, lastname) {
        this.firstname = firstname;
        this.lastname = lastname;
    }

    sayHello() {
        return `Hello i am ${this.firstname} ${this.lastname}`;
    }
}

const barnabe = new Person('Barnabé','Zebear');

console.log(barnabe.sayHello());
// => Hello i am Barnabé Zebear
```

---

### Les fonctions fléchés

* Elles offrent une syntaxe plus courte. <!-- .element: class="fragment" -->
* Elles ne possèdent pas de this. <!-- .element: class="fragment" -->

---

##### Exemple :

```javascript
var nums = [1,2,3,4,5,6,7,8];
nums.forEach( function(num) {
   console.log(num);
});

const nums = [1,2,3,4,5,6,7,8];
nums.forEach( nun => console.log(num));
```

Note: Dans notre cas (React) ça nous évite d'avoir a binder this et c'est chouette.

---

### Export / Import

* Exporter notre code sous forme de module. <!-- .element: class="fragment" -->
* Importer notre code sous forme de module. <!-- .element: class="fragment" -->
* Voyez ça un peu comme des require. <!-- .element: class="fragment" -->
* En es5 ça marche pas, pour le faire fonctionner il nous faut un outils comme Browserify, Webpack.. <!-- .element: class="fragment" -->

---

###### Import d'une function Annonyme

```javascript
// /libs/toto/index.js
export default function () {
    return 'Hello !';
}

// /app.js
import Toto from './libs/toto/index';

console.log(Toto());
// => Hello !
```

---

###### Import de plusieurs functions

```javascript
// /libs/toto/index.js
export function toto() {
    return 'Hello !';
}
export function titi() {
    return 'Goodbye !';
}

// /app.js
import {toto, titi} from './libs/toto/index';

console.log(toto());
// => Hello !
console.log(titi());
// => Goodbye !
```
