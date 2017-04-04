## Les 'state'

---

### Les états du composant

* Un composant possède des méthodes qui nous permettent de connaître son état (Lifecycle). <!-- .element: class="fragment" -->
* Par défaut, nous pouvous lui définir ses propres états. <!-- .element: class="fragment" -->
* La modification d'un état du composant entraîne un re-render du composant. <!-- .element: class="fragment" -->

---

### Props et State

---

#### Props

* Ils sont utilisés pour passer des données aux enfants. <!-- .element: class="fragment" -->
* Ils sont immutables. <!-- .element: class="fragment" -->
* Ils sont plus performants que les états pour passer des données. <!-- .element: class="fragment" -->

---

#### State

* Ils sont mutables. <!-- .element: class="fragment" -->
* Ils sont moins perfomants pour passer beaucoup de données. <!-- .element: class="fragment" -->

---

##### Exemple: [Counter](https://jsfiddle.net/r50shn3e/1/) <small>5</small>

```javascript
class Counter extends React.Component {
   constructor(props) {
      super(props);
      this.state = {
        clickCount: 0 // Par défaut l'état du compteur est à Zéro
      };
      this._incrementClick = this._incrementClick.bind(this);
    }
  _incrementClick() {
      this.setState({
        clickCount: this.state.clickCount + 1
        // Au clique on ajoute 1 au compteur
      });
  }
  render() {
    return (
      <div>
        {/*Ici le h2 est mis à jour avec la nouvelle valeur*/}
        <h2>{this.state.clickCount}</h2>
        <button onClick={this._incrementClick}> Add +</button>
      </div>
    );
  }
}
ReactDOM.render(<Counter />, document.querySelector('#root'));
```

---

#### Dans les faits

---

* Dans la plupart des cas les composants s'organisent ainsi:

    * Un composant parent, qui définit des états. <!-- .element: class="fragment" -->
    * Ces états sont passés en props aux enfants.<!-- .element: class="fragment" -->
    * Les enfants récupèrent les états.<!-- .element: class="fragment" -->
    * Les enfants ont très peu besoin de gérer les états (sauf cas spécifiques).<!-- .element: class="fragment" -->

---

### Lifecycle

* componentWillMount <!-- .element: class="fragment" -->
* componentDidMount <!-- .element: class="fragment" -->
* componentWillReceiveProps <!-- .element: class="fragment" -->
* shouldComponentUpdate <!-- .element: class="fragment" -->
* componentWillUpdate <!-- .element: class="fragment" -->
* componentDidUpdate <!-- .element: class="fragment" -->
* componentWillUnmount <!-- .element: class="fragment" -->

---

### componentWillMount

* Méthode appelée avant le rendu du composant.

---

### componentDidMount

* Méthode appelée après le rendu du composant.

---

### componentWillReceiveProps

* Méthode appelée lorsque le composant reçoit des 'props'.
* Cette fonction n'est pas appelée lors du rendu initial.

---

### shouldComponentUpdate

* Méthode qui permet de définir si un composant doit être mis à jour ou non.
* Par défaut la fonction retourne __true__.
* Pour ne pas re-rendre le composant, il faut retourner __false__.

---

##### Exemple: shouldComponentUpdate

```javascript
class Component extends React.Component {
  constructor(props) {
    super(props);
  }

  shouldComponentUpdate(nextProps, nextState) {
    return false;
  }

  render() {
    //...
  }
}
```

---

### componentWillUpdate

* Méthode appelée lorsque le composant reçoit de nouvelles valeurs.
* !! 'setState' ne fonctionne pas dans cette méthode.

---

### componentDidUpdate

* Méthode appelée après que le composant ait été mis à jour.

---

### componentWillUnmount

* Méthode appelée avant que le composant soit retiré du DOM.

---

##### Exemple: [Méthodes internes](https://jsfiddle.net/twh5fryy/6/) <small>6</small>
```javascript
class App extends React.Component {
  constructor() {
    super();
    this.state = {
      displayTimer: true,
      data: []
    };
    this._addItem = this._addItem.bind(this);
    this._removeItem = this._removeItem.bind(this);
  }
  _addItem() {
    return this._generateValue();
  }
  _generateValue() {
    let newValue = Math.floor(Math.random() * 100) + 1;
    let values = this.state.data;
    values.push(newValue);
    this.setState({
      data: values
    });
  }
  _removeItem(event) {
    if (!event.target.getAttribute('data-value')) {
      return false;
    }

    let valueToRemove = event.target.getAttribute('data-value');

  	this.setState({
      data: this.state.data.filter( (item) => {
        return item != valueToRemove;
      })
    });
  }
  render() {
    let items = this.state.data.map((value, index) => {
      return (
        <Component
          value={value}
          key={index}
          index={index}
          removeItem={this._removeItem} />
      );
    });
    return (
      <div>
        <button onClick={this._addItem}>
          Add one
        </button>
        <ul>
          {items.length > 0 ? items : 'Nothing to show'}
        </ul>
      </div>
    );
  }
}

class Component extends React.Component {
  constructor(props) {
    super(props);
  }
  componentWillMount() {
   console.log(`Component with index ${this.props.index} will mount.`);
  }
  componentDidMount() {
   console.log(`Component with index ${this.props.index} is mount.`);
  }
  componentWillUnmount() {
    console.log(`Component with index ${this.props.index} will unmount.`);
  }
  render() {
    return (
        <li>{this.props.value} <button data-value={this.props.value} onClick={this.props.removeItem.bind(this)}>X</button></li>
    );
  }
}

ReactDOM.render( < App / > , document.querySelector('#root'));
```

---

### Définir les états

* Lors de l'initialisation du composant, nous pouvons définir des états dans son constructor. <!-- .element: class="fragment" -->

---

##### Exemple: Définir un état dans le constructor
```javascript
class Component extends React.Component {
    constructor() {
        super();
        // Définir un état à l'initialisation du composant
        this.state = {
            firstState: 'value'
        };
    }
}
```

---


### Modifier les états

* React possède une méthode 'setState' pour modifier les états d'un composant. <!-- .element: class="fragment" -->

---

##### Exemple: Utilisation de 'setState'
```javascript
class Component extends React.Component {
    constructor() {
        // Définir un état à l'initialisation du composant
        this.state = {
            firstState: 'value'
        };
    }
    // Imaginons une fonction déclenchée au clic
    _handleClick() {
        // On définit une nouvelle valeur pour l'état 'firstState'
        this.setState({
            firstState: 'newValue'
        })
    }
}
```

---

### Passer des états à un composant enfant

* Sous forme de 'props', il est possible de passer l'état d'un composant parent à son enfant. <!-- .element: class="fragment" -->

---

##### Exemple: 'State' parent -> enfant
```javascript
class FirstComponent extends React.Component {
    constructor() {
        super();
        this.state = {
            firstState: 'value'
        };
    }
    render() {
        return (
            <SecondComponent value={this.state.firstState} />
        );
    }
}

class SecondComponent extends React.Component {
    render() {
        return (
            <p>The state has for value : {this.props.value}</p>
        );
    }
}
```
