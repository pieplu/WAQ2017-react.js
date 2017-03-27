## Les props

---

### Qu'est ce que les "Props"

* Props est le diminutif de 'properties'. <!-- .element: class="fragment" -->
* Les props sont un moyen de passer des données d'un composant à un autre. <!-- .element: class="fragment" -->
* Ils s'écrivent à la manières des "data attributes" en html. <!-- .element: class="fragment" -->
* On peut leur faire passer tout type de données (String, Array, Objects, function...). <!-- .element: class="fragment" -->
* Les props sont immutables. <!-- .element: class="fragment" -->

---

#### Exemple

```javascript
// On définit un component au nom original de Component
// Par défaut on initialise le nom et l'url de l'image
class Component extends React.Component {
    constructor(props) {
        super(props);

        this.name = "Elmo";
        this.imageUrl = "https://pbs.twimg.com/profile_images/716986458406424576/8AOacOOQ.jpg";
    }

    render() {
        return (
            <ChildComponent name={this.name} image={this.imageUrl} />
        );
    }
}

// On définit notre composant enfant
// et on lui demande d'afficher les valeurs passés en props
class ChildComponent extends React.Component {
    render() {
        return (
            <div>
                <img src={this.props.image} />
                <h1>{this.props.name}</h1>
            </div>
        );
    }
}

```

---

### Les propsTypes

* Permet de vérifier les props. <!-- .element: class="fragment" -->
* Documente nos composants. <!-- .element: class="fragment" -->

---

#### Exemple

---

* Imaginons nous sommes en train de développer une application et nous devons créer un champs de formulaire,
il y a de forte chance pour que l'application ait besoin d'afficher plusieurs fois ce champs de formulaire
dans des contextes différents (un champs de recherche, un champs pour entrer le titre d'un blog post...)

---

```javascript

class Input extends React.Component {
    render() {
        return (
            <input
                type={this.props.type}
                name={this.props.name}
                placeholder={this.props.placeholder}
                onFocus={this.props.onFocus} />
        );
    }
}

class Form extends React.Component {
    constructor(props) {
        super(props);
        this._onFocus = () => {
            console.log('focus');
        };
    }
    render() {
        return (
            <Input
                type="text"
                name="login"
                placeholder="Nom d'utilisateur"
                onFocus={this._onFocus} />
        );
    }
}
```

---

* Notre composant est réutilisable c'est chouette, sauf que qu'est ce qui se passe si
dans name je lui passe une fonction...

---

```html
<input data-reactroot=from"" type="text" name="function () {
            window.runnerWindow.proxyConsole.log('focus');
        }" placeholder="Nom d'utilisateur">
```

* Oups, ça va beaucoup moins bien fonctionner là, c'est là où les propsTypes vont rentrer en jeux.

---

```javascript

class Input extends React.Component {
    static propTypes = {
        type: React.PropTypes.string,
        name: React.PropTypes.string,
        placeholder: React.PropTypes.string
    }
    render() {
        return (
            <input
                type={this.props.type}
                name={this.props.name}
                placeholder={this.props.placeholder}
                onFocus={this.props.onFocus} />
        );
    }
}

class Form extends React.Component {
    constructor(props) {
        super(props);
        this._onFocus = () => {
            console.log('focus');
        };
    }
    render() {
        return (
            <Input
                type="text"
                name={this._onFocus}
                placeholder="Nom d'utilisateur"
                onFocus={this._onFocus} />
        );
    }
}
```

---

* Maintenant en ayant définit des PropTypes pour mon composant Input j'ai une erreur qui apparait dans la console.

```
Warning: Failed propType: Invalid prop `name` of type `function` supplied to `Input`, expected `string`. Check the render method of `Form`.
```

---

* React nous dit clairement ce qui ne va pas et où le corriger.
* Un debuggage facile en cas d'erreur.
* Une documentation du code.

---

### Le cas de children

* React intégre par défaut un props 'children'.
* Ce props children retourne tout ce qu'on passe entre la balise ouvrante et fermante du component.
* props.children peut-être à la fois un tableau et un objet. Si on lui passe plusieurs components, ce sera un Array,
sinon ce sera un objet.

---

#### Example

* Dans cet exemple on définit  4 Components :
* Un premier qui sera notre Component "Master ou Root" (App)
* Puis trois autres :
* Un component FirstComponent qui se content d'afficher ce qu'on lui passe (this.props.children)
* SecondComponent qui affiche "Content of SecondComponent"
* ThirdComponent qui affiche "Third component"

---

```javascript

class App extends React.Component {
  render(){
    return (
         <FirstComponent>
           <SecondComponent />
            <ThirdComponent />
        </FirstComponent>
    );
  }
}

class FirstComponent extends React.Component {
  render() {
    return (
      <div>
        {this.props.children}
      </div>
    );
  }
}

class SecondComponent extends React.Component {
  render(){
    return (
        <p>Content of SecondComponent</p>
    );
  }
}

class ThirdComponent extends React.Component {
  render() {
    return (
      <p>Third Component</p>
    );
  }
}

const root = document.querySelector('#root');
ReactDOM.render(<App />, root);
```