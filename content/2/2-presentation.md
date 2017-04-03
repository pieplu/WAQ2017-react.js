## Créer un composant

---

### JSX

- JSX est une extension de la syntaxe de JavaScript. <!-- .element: class="fragment" -->
- Facile à prendre en main. <!-- .element: class="fragment" -->
- React fonctionne aussi sans JSX. <!-- .element: class="fragment" -->

---

#### JSX V.S Plain JS

---

```javascript
// Exemple pris de la documentation de React.js
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

#### Présentation

---

###### Exécuter du JavaScript parmis le JSX

```javascript
render() {
    const myVariable = 4+4;
    return (
        <p>
            {myVariable} // => 8
            {4+4} // => 8
        </p>
    );
}
```

---

###### Itérer sur un tableau

```javascript
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
```

---

###### Faire des conditions

```javascript
render() {
    return(
        {true ? 'toto' : 'titi'}
    )
}
```

---

###### Wrapper nos éléments

```javascript
render() {
    return (
        <div>first</div>
        <div>second</div>
    );
}
// Uncaught SyntaxError: embedded: Adjacent JSX elements must be wrapped
// in an enclosing tag

render() {
    return (
        <div>
            <div>first</div>
            <div>second</div>
        </div>
    );
}
```

---

### JSX:<br/> Les petites choses à savoir

---

###### Pour ajouter une classe, vous faites...

```javascript
render() {
    <div class="toto">
        Hello
    </div>
}

// Error: Warning: Unknown DOM property class. Did you mean className?

```

---

### Pourquoi ?

* 'Class' et 'For' sont des 'namespaces' réservés. <!-- .element: class="fragment" -->
* Il faut les remplacer par 'className' et 'htmlFor'. <!-- .element: class="fragment" -->

---

###### Exemple
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

### Les événements

* Les événements sont attachés aux composants sous le format 'camelCase'. <!-- .element: class="fragment" -->

---

###### Exemple
```javascript
render() {
    return (
        <div className="form-group">
            <label htmlFor="name">Nom</label>
            <input
                className="input"
                id="name"
                onFocus={this._handleFocus}
                onChange={this._handleChange} />
            <button onClick={this._handleClick}>Submit</button>
        </div>
    );
}
```

---

### Qu'est ce qu'un composant ?

- On peut voir un composant comme une fonction. <!-- .element: class="fragment" -->
- Il reçoit des données en paramètre et retourne du HTML. <!-- .element: class="fragment" -->
- Ce sont de petits modules facilement réutilisables. <!-- .element: class="fragment" -->
- Depuis React 14, on distingue deux types de composants: Stateless et Statefull. <!-- .element: class="fragment" -->

---

### Définir un composant

---

###### Notre composant

```javascript
import React from 'react'; // On importe React sous forme de module

// Maintenant que React est importé on va créer une classe
// On va l'appeler 'MonComposant'
// Cette classe va hériter de la classe React.Component
class MonComposant extends React.Component {
    // On va utiliser la fonction 'render'
    // pour retourner du Html
    render() {
        return (
            <p>Mon premier composant</p>
        ); // Markup qui sera retourné par le composant
    }
}
```

---

### Rendre le composant dans le DOM

---

### React nous permet de créer des composants mais ça s'arrête là.

- Nous pouvons rendre les composants dans le DOM grâce à React-dom. <!-- .element: class="fragment" -->
- React possède d'autres renders : <!-- .element: class="fragment" -->
- React native (Mobile), React Blessed (App pour Terminal), React VR, React Canvas...<!-- .element: class="fragment" -->

---

###### Rendre un composant dans le DOM <small>1</small>

```javascript
import React from 'react'; // On importe React sous forme de module (cf: Appendice 1)
import ReactDOM from 'react-dom';

// Maintenant que React est importé on va créer une classe
// On va l'appeler 'MonComposant'
// Cette classe va hériter de la classe React.Component
class MonComposant extends React.Component {
    // On va utiliser la fonction 'render'
    // pour retourner du Html
    render() {
        return (
            <p>Mon premier composant</p>
        ); // Markup qui sera retourné par le composant
    }
}

// On vient cibler l'id dans laquelle on veut rendre le composant
let root = document.querySelector('#root');
// On untilise la fonction render de ReactDOM pour venir lui indiquer quoi faire
// ici on lui dit 'Prends MonComposant et affiche le moi dans #root'

ReactDOM.render(<MonComposant />, root);
```
