## Les States

---

### Les états du composants

* Un composant possède des états par défault (Lifecycle). <!-- .element: class="fragment" -->
* On peut lui définir des états par défauts et les faire évoler au fil de la vie du composant. <!-- .element: class="fragment" -->
* La modification d'un état du composant entraine un update du component. <!-- .element: class="fragment" -->

---

### Props et State

---

#### Props

* Ils sont immutables. <!-- .element: class="fragment" -->
* Sont utilisé pour passer des data aux enfants. <!-- .element: class="fragment" -->
* Plus performants que les states pour passer des données. <!-- .element: class="fragment" -->

---

#### States

* Dans l'idéal les states doivent être gérés dans le composant "Master". <!-- .element: class="fragment" -->
* Sont moins perfomants pour passer beaucoup de données. <!-- .element: class="fragment" -->
* Sont mutables. <!-- .element: class="fragment" -->
* Pour passer des états aux enfants, utilisez des props :-) <!-- .element: class="fragment" -->

---

#### Dans les faits

---

* Dans la plupart des cas les composants s'organise comme ça:

    * On a un composant parent qui souvent définit des états.
    * Les états sont passés en props aux enfants (au besoin)
    * Les enfants, sauf cas spécifiques ont trés peu besoin de gérer les états.

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

* Méthode appellée juste avant le rendu du composant (client + serveur)

---

### componentDidMount

* Méthode appellée au moment du rendu côté client.

---

### componentWillReceiveProps

* Méthode appellée au moment où le composant reçoit des props, cette function n'est pas appellée lors du rendu inital.

---

### shouldComponentUpdate

* Méthode qui permet de définir si un composant doit être mis à jour ou non.
* Par défaut sa valeur est forcément __true__
* Si vous voulez ne pas re-rendre le composant il faut passer la function a __false__.

---

### componentWillUpdate

* Méthode appellée juste avant la récéption de nouvelle valeur pour les props et les states.
* !! setState ne fonctionne pas dans cette méthode.

---

### componentDidUpdate

* Méthode appellée juste après que le composant ait été mis à jour.

---

### componentWillUnmount

* Méthode appellée juste avant qu'un composant soit retiré du DOM.

---

##### Exemple: [Counter](https://jsfiddle.net/r50shn3e/1/)

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
        // Au clique ajoute 1 au compteur
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

##### Exemple: [Méthodes internes](https://jsfiddle.net/twh5fryy/6/)
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

* Nous pouvons définir des états par défaut à notre composant lors de son initialisation dans le constructor. <!-- .element: class="fragment" -->

---

##### Exemple: Définir un état dans le constructor
```javascript
class Component extends React.Component {
    constructor() {
        // Définir un état à l'initialisation du composant
        this.state = {
            firstState: 'value'
        };
    }
}
```

---


### Modifier les états

* React intègre une méthode setState pour modifier les états d'un composant. <!-- .element: class="fragment" -->

---

##### Exemple: Utilisation de setState
```javascript
class Component extends React.Component {
    constructor() {
        // Définir un état à l'initialisation du composant
        this.state = {
            firstState: 'value'
        };
    }
    // Imaginons une function déclenché au clique
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

* En se reposant sur les props, il est possible de passer l'état d'un composant parent à son enfant sous forme de props. <!-- .element: class="fragment" -->

---

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
