## Sommaire

* [ES6](#es6)
* [Let et Const](#let-et-const)
* [Les classes](#les-classes)
* [Les fonctions fléchés](#les-fonctions-fléchés)
* [Destructuring](#destructuring)
* [Rest et Spread](#rest-et-spread)
* [Export / Import](#export-import)

# I A - Point rapide ES6

Les exercices de la formation ainsi que les exemples seront tous en ES6.
On va donc faire un petit "check-up" pour que tout le monde parte du bon pied.

## ES6

* Nouvelle version de language JavaScript.
* Sortie en Juin 2015.
* Ajoute pas mal de nouveautés : Classes, Modules, Generateurs, fonctions flécheés...

## Let et Const

### Const

* Const ne peut être assigné qu'une seule fois et a pour portée le bloc.

#### Exemples :

```javascript
const toto = 'hello';

function sayHello() {
    toto = 'titi';
    console.log(toto);
}

sayHello();

// => Uncaught TypeError: Assignment to constant variable.
```

* Normal, on veut ré-assigner la valeur de toto dans la function sayHello

#### Exemples :

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

* Dans ce cas on a ré-assigner const toto mais dans le scope de la function.

### Let

* Let c'est un peu le nouveau var, sauf qu'il est scopé au bloc.
* Il fait sensiblement la même chose que const mais sa valeur peu lui être ré-assignée.

#### Exemples :

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

## Les classes

* En fait ça ajoute aucune fonctionnalité par rapport a ES5.
* Par contre la syntaxe est bien plus facile à lire

#### Exemples :

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

* On peut facilement faire de l'héritage avec le mot clé extends

#### Exemples :

```javascript
class Person {
    constructor(firstname, lastname) {
        this.firstname = firstname;
        this.lastname = lastname;
    }

    sayHello() {
        return `Hello i am ${this.firstname} ${this.lastname}`;
    }
}

class Animal extends Person {
    constructor(firstname, lastname, yellType) {
        // Grace a super on recupere les methodes de la classe parente
        super(firstname, lastname);
        this.yellType = yellType;
    }

    sayYell() {
        return `${this.firstname} make ${this.yellType}`;
    }
    // Exemple de getter
    get yell() {
        return this.yellType;
    }

    // Exemple de setter
    set yell(value) {
        return this.yellType = value;
    }

}

const barnabe = new Animal('Barnabé', 'Zebear', 'Grr Grr Grr');

console.log(barnabe.sayHello());
// => Hello i am Barnabé Zebear
console.log(barnabe.sayYell());
// => Barnabé make Grr Grr Grr
console.log(barnabe.yell);
// => Grr Grr Grr

barnabe.yell = 'Miaou';
console.log(barnabe.yell);
// => Miaou
```

## Les fonctions fléchés

* Elle offre une syntaxe plus courte
* Elle ne possède pas de this

#### Exemple :

```javascript
var nums = [1,2,3,4,5,6,7,8];
nums.forEach( function(num) {
   console.log(num);
});

const nums = [1,2,3,4,5,6,7,8];
nums.forEach( nun => console.log(num));
```

* Dans notre cas (React) ça nous évite d'avoir a binder this et c'est chouette.

## Destructuring

* On peut assigner des variables en se posant sur la structure de l'objet ou du tableau (c'est pas clair voyons dans les faits)

#### Exemples :

```javascript
// ES5

var myObj = {
    id: 789,
    name: 'First post'
};

var id = myObj.id;
// => 789
var name = myObj.name;
// => First post

// ES6

const myObj = {
    id: 789,
    name: 'First post'
};

const {id, name} = myObj;
// => 789
// => First post
```

* Avec les tableaux c'est pareil

```javascript
const myArray = ['first','second','three'];
const [first, second, three] = myArray;
// => first
// => second
// => three
```

* Et on peut combiner les deux

```javascript
const myHybrid = {
    id: 789,
    name: 'My first post',
    comments: [
        {id: 1, content: 'Frist comment'},
        {id: 2, content: 'Second comment'},
        {id: 3, content: 'Third comment'}
    ]
};

const {id, name, comments: [first, second]} = myHybrid;
console.log(id);
// => 789
console.log(name);
// => My first post
console.log(first);
// => Object {id: 1, content: "Frist comment"}
console.log(second);
// => Object {id: 2, content: "Second comment"}
```

## Rest et Spread

### Rest

* On peu facilement recuperer les valeurs en param d'une function sous forme de tableau

#### Exemples :

```javascript
function toto(first, ...others) {
    console.log(first);
    console.log(others);
}

toto('one', 'two', 'three', 'four', 'five');
// => one
// => ["two", "three", "four", "five"]
```
### Spread

* Permet de développer un objet itérable

#### Exemple :

```javascript
const toto = "Hello world";
console.log([...toto]);
// => ["H", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"]
```

## Export / Import

* On peut facilement exporter notre code sous forme de module et l'importer.
* Voyez ça un peu comme des require
* En es5 ça marche pas, pour le faire fonctionner il nous faut un outils comme Browserify, Webpack..

#### Exemples :

* Import d'une function Annonyme

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

* Import de plusieurs functions

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

* Et la globalement tu piges que tu peux avoir des function locale non exportable

```javascript
// /libs/toto/index.js
function tata(name) {
    return `Hola ${name}`;
}
export function titi() {
    return tata('Joe');
}

// /app.js
import {titi} from './libs/toto/index';

console.log(titi());
// => Hola Joe
```
