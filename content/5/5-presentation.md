# V - Faites communiquer vos composants

---

## Sommaire

* [Communication parent vers enfant](#communication-parent-vers-enfant)
* [Communication enfant vers parent](#communication-enfant-vers-parent)
* [Point rapide composant Statefull et Stateless](#point-rapide-composant-statefull-et-stateless)
    * [Stateless](#stateless)
    * [Statefull](#statefull)
* [L'attribut Ref](#lattribut-ref)
* [Exercice](#exercice)

---

## Communication parent vers enfant

* C'est le cas le plus simple et le plus courant
* On peut faire passer des données d'un parent à un enfant sous forme de properties

---

### Exemple

* Souvenez vous de l'exemple du Elmo...

---

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

// Et maintenant quand on clique sur Elmo une alert nous dit 'Yohooo'
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

## Communication enfant vers parent

* C'est exactement ce    qu'on à fait dans l'exercice précédent, au clique sur
le bouton 'supprimer' du panier on vennait faire ça : http://cloud.tonours.fr/index.php/apps/files_sharing/ajax/publicpreview.php?x=1428&y=1000&a=false&file=cart.jpg&t=XwIZyg7ciQqkxX2


---

## Point rapide composant Statefull et Stateless

---

### Stateless

---

* On distingue deux types de composants, les composants Stateless et Statefull.
* Les composant Stateless sont de simple function, elle peuvent recevoir des props et faire un render.
* On peut utiliser ce type de composant lorsqu'on parle de "Dumb composant", c'est à dire des composants
statiques dont le but est seulement d'afficher quelque chose comme le ferait Twig, Erb ou Handlebars.

---

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

### Statefull

* Les composants stateFull vont être des class qui héritent de React.Component.
* Ces composants sont dit "Statefull" car ils nous permettent de gérer l'état d'un composant
(State, LifeCycle)
* C'est le type de composant que nous utilisons depuis le début de la formation

---

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

// Tu peux voir le rendu ici : https://jsfiddle.net/23syfzpg/3/
```

---

## L'attribut Ref

* L'attribut ref nous permet de venir récupérer le DOM rendu par un composant.
* Comme il nous renvoit un élément du DOM on peut le manipuler avec l'API DOM,
ex: this.login.focus() va focuser le champs login.

---

### Exemple

* Un cas d'usage plutôt commun est celui de la récupération de la valeur d'un champ de formulaire

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

// Tu peux voir le rendu sur : https://jsfiddle.net/x56t9nng/5/
```

---

## Exercice

- Allez dans le dossier exercice et faites ```git checkout exercice-4```
- ```$ npm run start```
- Ouvrir votre navigateur à l'url : http://localhost:8080
- Ouvrir le dossier formation-react-exercices dans votre éditeur préféré et lisez TODO.MD
