## Créer un composant

---

### De quoi va-t-on parler ?

- JSX. <!-- .element: class="fragment" -->
- Qu'est ce qu'un composant. <!-- .element: class="fragment" -->
- Définir un composant. <!-- .element: class="fragment" -->
- Rendre le composant dans la page. <!-- .element: class="fragment" -->

---

### [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)

- JSX est une extension de la syntaxe de JavaScript. <!-- .element: class="fragment" -->
- Doit être "transpilé". <!-- .element: class="fragment" -->
- React fonctionne aussi sans JSX. <!-- .element: class="fragment" -->

---

```javascript
var comp = <p>Un texte</p>;
```

---

#### JSX VS Plain JS

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

###### Exécuter du JavaScript parmi le JSX

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

###### Faire des conditions

```javascript
render() {
    return(
        {true ? 'toto' : 'titi'}
    )
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

### JSX:<br/> Les petites choses à savoir

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

###### Ajouter une class ?

```javascript
render() {
    <div class="toto">
        Hello
    </div>
}

// Error: Warning: Unknown DOM property class. Did you mean className?

```

---

<!-- .slide: data-background-image="images/obama.gif"  -->
### Pourquoi ?

---

### Namespace réservé de React

#### Il faut le remplacer par 'className'. <!-- .element: class="fragment" -->

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

### Qu'est-ce qu'un composant ?

- On peut voir un composant comme une "super" fonction. <!-- .element: class="fragment" -->
- Reçoit des données en paramètre et retourne du HTML. <!-- .element: class="fragment" -->
- Petit module facilement réutilisable. <!-- .element: class="fragment" -->
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

## Ok
### Comment j'affiche ça dans ma page?<!-- .element: class="fragment" -->

---

### React nous permet de créer des composants. 
## Et ça s'arrête là.<!-- .element: class="fragment" -->

---

### Mais...
 
- On peut rendre les composants dans le DOM grâce à React-dom. <!-- .element: class="fragment" -->
- Rendre pour mobile avec React native <!-- .element: class="fragment" -->
- Dans le terminal avec React Blessed <!-- .element: class="fragment" -->
- Etc (React VR, React Canvas...) <!-- .element: class="fragment" -->

---

<!-- .slide: data-background-image="images/stars.gif"  -->
###### Rendre un composant dans le DOM <small>[1](https://jsfiddle.net/uj90kq60/)</small>

```javascript
import React from 'react'; // On importe React sous forme de module
import ReactDOM from 'react-dom';


class MonComposant extends React.Component {
    render() {
        return (
            <p>Mon premier composant</p>
        );
    }
}

// On vient cibler l'id dans laquelle on veut rendre le composant
let root = document.querySelector('#root');
// On utilise la fonction render de ReactDOM pour venir lui indiquer quoi faire
// ici on lui dit 'Prends MonComposant et affiche-le moi dans #root'

ReactDOM.render(<MonComposant />, root);
```

---

### En résumé

- JSX nous permet de générer des objets JavaScript avec une notation Html. <!-- .element: class="fragment" -->
- Remplacez 'classe' par 'className' et 'for' par 'htmlFor'. <!-- .element: class="fragment" -->
- Les évenements sont ajoutés au format 'camelCase' (onClick, onChange etc.). <!-- .element: class="fragment" -->
- Le rendu du Html dans un composant se faire dans la méthode 'render()'. <!-- .element: class="fragment" -->
- Pour rendre un composant dans le DOM il faut utiliser React-DOM. <!-- .element: class="fragment" -->
