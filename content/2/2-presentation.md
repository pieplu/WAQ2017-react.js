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

#### Presentation

---

###### Exécuter du JavaScript parmis le JSX

```javascript
render() {
    const myVariable = 4+4;
    return (
        <p>
            {myVariable} // => 8
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

###### On peut faire des conditions

```javascript
render() {
    return(
        {true ? 'toto' : 'titi'}
    )
}
```

---

###### On doit wrapper nos elements

```javascript
render() {
    return (
        <div>first</div>
        <div>second</div>
    );
}
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

### JSX: Les petites choses à savoir

---

###### Vous voulez ajouter une classe donc vous faites

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

* 'Class' et 'For' sont des namespaces réservés. <!-- .element: class="fragment" -->
* Il faut les remplacer par 'className' et 'htmlFor'. <!-- .element: class="fragment" -->
* Les events sont attachés aux composants au format camelCase. <!-- .element: class="fragment" -->

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

### Qu'est ce qu'un composant

- On peut voir les composants comme des fonctions qui recoivent des 'props' et des 'states' et qui affichent du HTML. <!-- .element: class="fragment" -->
- Dans l'idée ce sont des petits modules qu'on peut facilement réutiliser. <!-- .element: class="fragment" -->
- Depuis React 14 on distingue deux types de composants: Stateless et Statefull. <!-- .element: class="fragment" -->

---

### Définir un composant

---

###### Notre composant

```javascript
import React from 'react'; // On importe React sous forme de module

// Maintenant que React est importé on va créer une classe
// On va l'appeller 'MonComposant'
// Cette classe va hériter de la classe React.Component
class MonComposant extends React.Component {
    // On va utiliser la function 'render'
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
// On va l'appeller 'MonComposant'
// Cette classe va hériter de la classe React.Component
class MonComposant extends React.Component {
    // On va utiliser la function 'render'
    // pour retourner du Html
    render() {
        return (
            <p>Mon premier composant</p>
        ); // Markup qui sera retourné par le composant
    }
}

// On vient targeter id dans laquelle on veut rendre le composant
let root = document.querySelector('#root');
// On untilise la function render de ReactDOM pour venir lui indiquer quoi faire
// ici on lui dit 'Prends MonComposant et affiche le moi dans #root'

ReactDOM.render(<MonComposant />, root);
```
