# IV - Les States

---

## Sommaire

* [Les états du composants](#les-états-du-composants)
* [Props et State](#props-et-state)
    * [Props](#props)
    * [States](#states)
    * [Dans les faits](#dans-les-faits)
* [Lifecycle](#lifecycle)
* [Définir les états](#définir-les-états)
* [Modifier les états](#modifier-les-états)
* [Passer des états à un composant enfant](#passer-des-états-à-un-composant-enfant)
* [Exercice](#exercice)

---

## Les états du composants

* Un composant possède des états par défault (Lifecycle)
* On peut lui définir des états par défauts qui évoluent au fil de la vie du composant. (isClicked: false par défaut puis true lorsqu'un bouton est cliqué)
* La modification d'un état du composant entraine un update du component (re-render du composant)

---

## Props et State

---

### Props

* Ils sont immutables
* Sont utilisé pour passer des data aux enfants
* Plus performants que les states pour passer des données

---

### States

* Dans l'idéal les states doivent être gérés dans le composant "Master"
* Sont moins perfomants pour passer beaucoup de données
* Sont mutables
* Pour passer des états aux enfants, utilisez des props :-)

---

### Dans les faits

---

* Dans la plupart des cas les composants s'organise comme ça:

    * On a un composant parent qui souvent définit des états.
    * Les états sont passés en props aux enfants (au besoin)
    * Les enfants, sauf cas spécifiques ont trés peu besoin de gérer les états.

---

## Lifecycle (TODO définir?)

---

* React possède des fonctions interne lui permettant de connaitre l'état du composant.
* componentWillMount: Méthode appellée juste avant le rendu du composant (client + serveur)
* componentDidMount: Méthode appellée au moment du rendu côté client
* componentWillReceiveProps: Méthode appellée au moment où le composant reçoit des props,
cette function n'est pas appellée lors du rendu inital.
* shouldComponentUpdate: Méthode qui permet de définir si un composant doit être mis à jour ou non,
par défaut sa valeur est forcément true, si vous voulez ne pas re-rendre le composant il faut passer le function a false.
Dans les faits on utilise trés peu cette méthode. (1)
* componentWillUpdate: Méthode appellée juste avant la récéption de nouvelle valeur pour les props et les states. !! setState ne fonctionne pas dans cette méthode
* componentDidUpdate: Méthode appellée juste après que le composant ait été mis à jour.
* componentWillUnmount: Méthode appellée juste avant qu'un composant soit retiré du DOM

    1 - Par défaut React modifie le dom seulement si une modification à lieu.

    Exemple, j'ai un timer, toute les secondes j'incrémente le timer et j'affiche la nouvelle valeur dans la page.
    Toutes les secondes j'ai donc une modification du DOM qui est fait avec ma nouvelle valeur.
    Maintenant imaginon que je prends le même code sauf qu'au lieu d'incrémenter la valeur je laisse la valeur à
    sont état initial c'est à dire "zero". Je vais voir que le composant se met bien à jour (shouldComponentUpdate),
    par contre si j'inspecte le dom aucune modification n'est fait.

---

## Exemple

---

* Utilisation des Méthodes interne du composant pour mettre en place un timer

---

```javascript
class Counter extends React.Component {
   // Par défaut l'état du compteur est à Zéro
   constructor(props) {
      super(props);
      this.state = {
        clickCount: 0
      };
      this._incrementClick = this._incrementClick.bind(this);
    }
  // Au clique sur le bouton j'ajoute 1 au compteur
  _incrementClick() {
      this.setState({
        clickCount: this.state.clickCount + 1
      });
  }
  render() {
    return (
      <div>
        {/*Ici le h2 est mis à jour avec la nouvelle valeur*/}
        <h2>{this.state.clickCount}</h2>
        <button onClick={this._incrementClick}>
          Add +
        </button>
      </div>
    );
  }
}

ReactDOM.render(<Counter />, document.querySelector('#root'));
```

---

* Voir quand les Méthodes internes sont trigger (https://jsfiddle.net/twh5fryy/6/)

---

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

## Définir les états

* Nous pouvons définir des états par défaut à notre composant lors de son initialisation dans le constructor

---

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


## Modifier les états

* React intègre une méthode setState pour modifier les états d'un composants

---

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

## Passer des états à un composant enfant

* En se reposant sur les props, il est possible de passer l'état d'un composant parent à son enfant sous forme de prop.

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

---

## Exercice

- Allez dans le dossier exercice et faites ```git checkout exercice-3```
- ```$ npm run start```
- Ouvrir votre navigateur à l'url : http://localhost:8080
- Ouvrir le dossier formation-react-exercices dans votre éditeur préféré et lisez TODO.MD
