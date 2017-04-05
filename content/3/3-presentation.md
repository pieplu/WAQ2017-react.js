## Les 'props'

---

### De quoi va-t-on parler ?

- Qu'est ce que les 'Props'. <!-- .element: class="fragment" -->
- Les propsTypes. <!-- .element: class="fragment" -->
- Le cas de children. <!-- .element: class="fragment" -->

---

### Qu'est-ce que les 'Props' ?

* Props est le diminutif de 'properties'. <!-- .element: class="fragment" -->
* Les props sont un moyen de passer des données d'un composant à un autre. <!-- .element: class="fragment" -->
* Ils s'écrivent à la manière des "data-attributes" en Html. <!-- .element: class="fragment" -->
* On peut leur faire passer plusieurs types de données (String, Array, Objects, Function...). <!-- .element: class="fragment" -->
* Les props sont immutables. <!-- .element: class="fragment" -->

---

#### Exemple

```javascript
// On définit un composant au nom original de Component
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
```

---

#### Exemple ChildComponent <small>2</small>

```javascript
// On définit notre composant enfant
// et on lui demande d'afficher les valeurs passées en 'props'

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

### 'Props' par défaut

- Il est possible de définir des 'props' par défaut grâce à 'defaultProps'.<!-- .element: class="fragment" -->

---

#### Exemple defaultProps

```javascript
// Maintenant, si on ne passe pas de props depuis le parent,
// notre composant affichera des données par défaut

class ChildComponent extends React.Component {
    static defaultProps = {
        name: 'Grover',
        image: 'https://s-media-cache-ak0.pinimg.com/originals/2d/8a/4d/2d8a4d3cdb382cf0bd2c130ac5e2cd81.jpg'
    }
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

### Les 'propsTypes'

* Ils permettent de vérifier le type des 'props'. <!-- .element: class="fragment" -->
* Ils documentent nos composants. <!-- .element: class="fragment" -->

---

#### Exemple: Input réutilisable

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
```

---

#### Exemple: Form

```javascript
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

* Notre composant Input est réutilisable, c'est chouette !
* Cependant, si dans 'name' je lui passe une fonction... <!-- .element: class="fragment" -->

---

<!-- .slide: data-background-image="images/chat-drame.gif"  -->
#### Et là, c'est le drame !

<small>console:</small>
```html
<input data-reactroot=from"" type="text" name="function () {
            window.runnerWindow.proxyConsole.log('focus');
        }" placeholder="Nom d'utilisateur">
```

---

* C'est à ce moment, où les 'propsTypes' vont rentrer en jeux.

---

#### Exemple: Input
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
```

---

#### Un peu plus explicite...

<small>console:</small>
```javascript
Warning: Failed propType: Invalid prop `name` of type `function` supplied
to `Input`, expected `string`. Check the render method of `Form`.
```

---

* React nous dit clairement ce qui ne va pas et où le corriger.
* Il permet un debuggage facile en cas d'erreur. <!-- .element: class="fragment" -->
* Il documente notre code. <!-- .element: class="fragment" -->

---

### Les enfants (Le cas de children)

---

#### Component Inception

![](images/react-child-component-diagram.svg)<!-- .element: class="img--no-border" -->

---

* React intègre par défaut un props 'children'.
* Ce props 'children' retourne tout ce qui est mis entre la balise ouvrante et fermante du composant. <!-- .element: class="fragment" -->
* 'props.children' peut être à la fois un tableau ou un objet. <!-- .element: class="fragment" -->
* Si on lui passe plusieurs composants, ce sera un Array, sinon ce sera un objet. <!-- .element: class="fragment" -->

---

#### Exemple: SecondComponent

```javascript
class SecondComponent extends React.Component {
  render(){
    return (
        <p className="alert alert-success">Content of SecondComponent</p>
    );
  }
}
```

---

#### Exemple: ThirdComponent

```javascript
class ThirdComponent extends React.Component {
  render() {
    return (
      <p className="alert alert-info">Third Component</p>
    );
  }
}
```

---

#### Exemple: FirstComponent

```javascript
class FirstComponent extends React.Component {
  render() {
    return (
      <div className="panel panel-primary">
        {this.props.children}
      </div>
    );
  }
}
```

---

#### Exemple: App <small>4</small>

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

const root = document.querySelector('#root');
ReactDOM.render(<App />, root);
```

---

### En résumé

- Les 'props' sont utilisés pour faire passer des données entre composants. <!-- .element: class="fragment" -->
- Les 'props' acceptent plusieurs type de données. <!-- .element: class="fragment" -->
- Les 'propsType' permettent de spécifier le typage de nos 'props'. <!-- .element: class="fragment" -->
