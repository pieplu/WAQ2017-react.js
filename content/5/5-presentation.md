## Faites communiquer vos composants

---

### Communication parent vers enfant

* C'est le cas le plus simple et le plus courant. <!-- .element: class="fragment" -->
* On peut faire passer des données d'un parent à un enfant sous forme de properties (props). <!-- .element: class="fragment" -->

---

##### Exemple: Communication parent -> enfant
```javascript
class Component extends React.Component {
    constructor(props) {
        super(props);

        this.click = () => alert('Yohooo');
        this.name = "Elmo";
        this.imageUrl = "https://pbs.twimg.com/profile_images/716986458406424576/8AOacOOQ.jpg";
    }

    render() {
        return (
            <ChildComponent name={this.name} image={this.imageUrl} click={this.click} />
        );
    }
}
```

---

##### Exemple: Communication parent -> enfant <small style="vertical-align:middle;">(suite)</small>
```javascript

// Et maintenant quand on clique sur Elmo un alert() nous dit 'Yohooo'
class ChildComponent extends React.Component {
    render() {
        return (
            <div onClick={this.props.click}>
                <img src={this.props.image} />
                <h1>{this.props.name}</h1>
            </div>
        );
    }
}
```

---

### Communication enfant vers parent

* Permet de déclencher une action depuis le composant enfant. <!-- .element: class="fragment" -->
* On peut faire passer des données d'un enfant à un parent via une fonction de 'callback'. <!-- .element: class="fragment" -->

---

##### Exemple: Communication enfant -> parent
```javascript
class Component extends React.Component {
  constructor() {
    super();
    this.state = { isCliked: false };
  }
  _handleClick(newState) {
    this.setState({ isClicked: newState });
  }
	render() {
    return (
      <div>
        <p>My button is {this.state.isClicked ? 'clicked' : 'not clicked'}</p>
        <ChildComponent
          setNewState={(newState) => this._handleClick(newState)}
          buttonState={this.state.isClicked} />
      </div>
    );
  }
}
```

---

##### Exemple: Communication enfant -> parent <small style="vertical-align:middle;">(suite)</small>
```javascript
class ChildComponent extends React.Component {
  _handleClick() {
  	let newState = !this.props.buttonState;
    this.props.setNewState(newState);
  }
  render() {
  	return(
    	<div>
    	  <button onClick={() => this._handleClick()}>
          Click me
        </button>
    	</div>
    )
  }
}

let root = document.querySelector('#root');
ReactDOM.render(<Component/>, root);
```

---

### Différence entre composant Statefull et Stateless

---

### Stateless <small>8</small>

* Les composants 'Stateless' sont de simples fonctions. <!-- .element: class="fragment" -->
* Elles peuvent recevoir des 'props' et faire un 'render'. <!-- .element: class="fragment" -->
* Ce type de composant est aussi appelé 'Dumb component'. <!-- .element: class="fragment" -->
* Ce sont des composants statiques, qui ne possédent pas d'états. <!-- .element: class="fragment" -->

---

##### Exemple: Composants 'Stateless'
```javascript
const items = [
	  {id: '1e91b13a-cdb2-4268-bc43-da5b08785ed3', content: 'First item'},
    {id: '66c4ef81-40a2-410b-9a5d-750542caf912', content: 'Second item'},
    {id: 'f6b36935-1426-471b-a998-e49e81a6e7a6', content: 'Third item'},
    {id: 'd8c32bba-d9cf-4c9d-8bd3-d595e6b34ee7', content: 'Fourth item'},
];

const App = ({data}) => {
  return (
  	<ul>
    	{data.map( (item) => {
        return <Item key={item.id} data={item} />;
      })}
    </ul>
  );
};

const Item = ({data}) => {
	return <li key={data.id}>{data.content}</li>;
};

const node = document.querySelector('#root');
ReactDOM.render(<App data={items} />, node);

// Tu peux voir le rendu là : https://jsfiddle.net/bmjvgw1v/1/
```

---

### Statefull <small>9</small>

* Les composants 'statefull' sont des classes qui héritent de React.Component. <!-- .element: class="fragment" -->
* Ces composants sont 'Statefull' car ils permettent la gestion des états d'un composant (State, LifeCycle). <!-- .element: class="fragment" -->

---

##### Exemple: Composants 'Statefull'
```javascript
const items = [
    {id: '1e91b13a-cdb2-4268-bc43-da5b08785ed3', content: 'First item'},
    {id: '66c4ef81-40a2-410b-9a5d-750542caf912', content: 'Second item'},
    {id: 'f6b36935-1426-471b-a998-e49e81a6e7a6', content: 'Third item'},
    {id: 'd8c32bba-d9cf-4c9d-8bd3-d595e6b34ee7', content: 'Fourth item'},
];

class App extends React.Component {
  constructor() {
    super();
    this.state = {
        data: items
    };
  }
  render() {
    let items = this.state.data;
    return (
      <ul>
        {items.map( (item) => {
          return <Item key={item.id} data={item} />
        })}
      </ul>
    );
  }
}
```

---

##### Exemple: Composants 'Statefull' <small style="vertical-align:middle;">(suite)</small>
```javascript
class Item extends React.Component {
  constructor(props) {
    super(props);
    this.data = props.data;
  }
  _showId() {
  	alert(`My id is ${this.data.id}`);
  }
  render() {
  	return <li key={this.data.id} onClick={this._showId.bind(this)}>{this.data.content}</li>;
  }
}
const node = document.querySelector('#root');
ReactDOM.render(<App />, node);

```

---

### L'attribut Ref

* L'attribut ref nous permet de récupérer le rendu DOM d'un composant. <!-- .element: class="fragment" -->
* Il peut être manipulé avec l'API DOM.<br/>Ex: this.login.focus() va 'focuser' le champ login. <!-- .element: class="fragment" -->

---

#### Exemple: Récupérer les valeurs d'un formulaire <small>10</small>

```javascript

class Login extends React.Component {
  constructor() {
    super();
  }
  _handleSubmit(event) {
  	event.preventDefault();
    let login = this.login;
    let password = this.password;
    let data = {login: login.value, password: password.value};
    console.log(data);
    // log input#login value et input#password value
  }
  render() {
  	return (
    	<form method="POST" onSubmit={this._handleSubmit.bind(this)}>
            <input type="text" ref={(ref) => this.login = ref} id="login" className="login" />
            <br />
            <input type="password" ref={(ref) => this.password = ref} id="password" className="password" />
            <br />
            <button type="submit">Log toi</button>
        </form>
    );
  }
}

const node = document.querySelector('#root');
ReactDOM.render(<Login />, node);

// Tu peux voir le rendu sur : https://jsfiddle.net/x56t9nng/6/
```
