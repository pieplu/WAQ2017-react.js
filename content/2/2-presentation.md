## II - Créer un composant

---

### Sommaire

* [JSX](#jsx)
* [Qu'est ce qu'un composant](#quest-ce-quun-composant)
* [Définir un composant](#définir-un-composant)
* [Rendre le composant dans la page](#rendre-le-composant-dans-la-page)
* [Exercice](#exercice)

---

### JSX

- [JSX](https://facebook.github.io/jsx/) est une extension de la syntaxe de JavaScript.
- On peut facilement le prendre main
- On peut utiliser écrire ses composants avec ou sans JSX (Mais c'est préférable d'utiliser JSX).

---

#### JSX V.S Plain JS

---

```javascript
// Exemple pris de la doc react
// source: https://facebook.github.io/react/docs/jsx-in-depth.html#the-transform

// Input (JSX):
var app = <Nav color="blue"><Profile>click</Profile></Nav>;
// Output (JS):
var app = React.createElement(
  Nav,
  {color:"blue"},
  React.createElement(Profile, null, "click")
);
```

---

#### Syntaxe

---

```javascript
// on peut éxécuter du JavaScript parmis le markup avec des "curly-brackets"
//
render() {
    const myVariable = 4+4;
    return (
        <p>
            {myVariable} // => 8
        </p>
    );
}

// Ce qui peut être pratique pour faire des boucles
//
render() {
    const names = ['Joe', 'Bob', 'Tom','Phil'];
    return(
        <ul>
        {names.map( (name, key) => {
            return <li key={key}>{name}</li>
        })}
        </ul>
    )
}

// On est obligé de wrapper plusieurs éléments dans un root container
//
render() {
    return (
        <div>first</div>
        <div>second</div>
    );
}

// SyntaxError: Adjacent JSX elements must be wrapped in an enclosing tag

// Il faut wrapper ça
render() {
    return (
        <div>
            <div>first</div>
            <div>second</div>
        </div>
    );
}

// Ou des conditions...
//
render() {
    return(
        {true ? 'toto' : 'titi'}
    )
}
```

---

#### Les choses à savoir

---

- Vous voulez ajouter une classe donc vous faites :

```javascript

render() {
    <div class="toto">
    </div>
}

// Error: Warning: Unknown DOM property class. Did you mean className?

```

---

- En JavaScript 'Class' et 'For' sont des namespaces réservés, il faut les remplacer par 'className' et 'htmlFor'.
- Les events sont attachés aux composants au format camelCase (onClick is back :) )

```javascript
render() {
    return (
        <div className="form-group">
            <label htmlFor="name">Nom</label>
            <input
                className="input"
                id="name"
                onChange={this._handleChange} />
        </div>
    );
}
```

---

### Qu'est ce qu'un composant

---

- On peut voir les composants comme des fonctions qui recoivent des 'props' et des 'states'
(on verra ça en détails plus tard) et qui affichent du HTML.
- Dans l'idée ce sont des petits modules qu'on peut facilement réutiliser (penser aux légo, a Twig, aux partials (rails))
- Depuis React 14 on distingue deux types de composants: Stateless et Statefull (on verra ça plus tard).

---

### Définir un composant

- Voici comment nous pouvons définir trés simplement un composant.

```javascript
import React from 'react'; // On importe React sous forme de module (cf: Appendice 1)

// Maintenant que React est importé on va créer une classe
// On va l'appeller 'MonComposant'
// Cette classe va hériter de la classe React.Component
class MonComposant extends React.Component {
    // On va utiliser la function 'render'
    // pour retourner du Html
    render() {
        return (
            // Markup qui sera retourné par le composant
            <p>Mon premier composant</p>
        );
    }
}
```

- Le composant est maintenant prêt, dans il nous reste plus qu'à le rendre dans le DOM.

---

### Rendre le composant dans la page

- React nous permet de créer des composants mais ça s'arrête là.
- Nous pouvons rendre les composants dans le DOM grâce à React-dom.
- React possède d'autre renders :
    - React native : https://facebook.github.io/react-native/
    - React blessed : https://github.com/Yomguithereal/react-blessed
    - React GL : https://projectseptemberinc.gitbooks.io/gl-react/content/
    - React Canvas : https://github.com/Flipboard/react-canvas

---


```javascript
import React from 'react'; // On importe React sous forme de module (cf: Appendice 1)
import ReactDOM from 'react-dom';

// Maintenant que React est importé on va créer une classe
// On va l'appeller 'MonComposant'
// Cette classe va hériter de la classe React.Component
class MonComposant extends React.Component {
    // On va utiliser la function 'render'
    // pour retourner du Html
    render() {
        return (
            // Markup qui sera retourné par le composant
            <p>Mon premier composant</p>
        );
    }
}

// On vient targeter l'id dans laquelle on veut rendre le composant
let root = document.querySelector('#root');
// On untilise la function render de ReactDOM pour venir lui indiquer quoi faire
// ici on lui dit 'Prends MonComposant et affiche le moi dans #root'
ReactDOM.render(<MonComposant />, root);
```

---

### Exercice

#### Pré-recquis

- [Node.js](https://nodejs.org/en/)
- [NVM](https://github.com/creationix/nvm)
- [Git](https://git-scm.com/)


#### Mise en place

- Cloner le repository ```$ git clone -b exercice-1 git@gitlab.savoirfairelinux.com:frontend/formation-react-exercices.git```
- ```$ cd formation-react-exercices```
- ```$ nvm install```
- ```$ npm install```
- ```$ npm run start```
- Ouvrir votre navigateur à l'url : http://localhost:8080
- Ouvrir le dossier formation-react-exercices dans votre éditeur préféré et lisez TODO.MD
