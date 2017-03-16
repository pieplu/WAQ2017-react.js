# VI - Les Css dans React

## Sommaire

* [Introduction](#introduction)
* [La manière classique](#la-manière-classique)
* [Utilisation des css inline](#utilisation-des-css-inline)
    * [React way](#react-way)
    * [Radium](#radium)
* [Les css-modules](#les-css-modules)
* [Exercice](#exercice)

## Introduction

* Comme on l'a vu les composants React regroupe le markup et le javascript
* Il est également possible d'ajouter les Css dans les composants, de manière inline,
avec ou sans module externe ou en utilisant les Css Modules.
* Pour vous présenter les différentes approches on va utiliser exactement
le même composant et les styles avec ces trois approches.

## La manière classique

* La manière classique consiste à séparer les css dans un fichier global main.css,
comme on le fait actuellement dans les plupart des projets.
* Nos css sont donc appliqués au markup en reposant sur les class (classNames)
que nous définissons dans nos composants.

### Exemple

* Voici notre composant "counter" en css ainsi que le composant React correspondant
* Tout est "normal" le css s'applique en fonction des classNames définit.
* Maintenant, si je veux réutiliser comme tel ce composant dans un autre projet,
je vais avoir besoin de :

    * Copier le css, ou le components .scss qui se trouve soit dans main.css,
    soit dans le dossier /sass/components.
    * Copier le composant React
    * venir dans mon nouveau projet
    * copier le bout de code css ou le composant scss et l'inclure dans main.scss
    * copier mon composant React dans mon nouveau projet

* Ça fait pas mal de manipulation mais c'est pas non plus le Pérou.

```css
.counter {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

  .counter__inner {
    box-shadow: 0 2px 0 #e8e8e8;
    padding: 30px;
    background: #fff;
  }

    .counter__value {
      font-size: 18px;
      text-transform: uppercase;
      color: #2C3E50;
      font-weight: bold;
      margin: 0 0 15px 0;
    }

    .counter__button {
      padding: 15px 30px;
      border: 0;
      background: #E74C3C;
      font-size: 12px;
      text-transform: uppercase;
      color: #fff;
      font-weight: bold;
      outline: none;
      transition: background .2s ease-in;
    }

    .counter__button:hover,
    .counter__button:focus {
      cursor: pointer;
      background: #e83523;
    }
```

```javascript
class Counter extends React.Component {
	constructor() {
  	super();
    this.state = {
    	count: 0
    };
  }
  _increment(event) {
  	event.preventDefault();
    let newValue = this.state.count+1;
    this.setState({
    	count: newValue
    });
  }
  render() {
  	return (
    	<div className="counter">
     		<div className="counter__inner">
        	<p className="counter__value">{this.state.count}</p>
          <button onClick={this._increment.bind(this)} className="counter__button">Clique ici pour un +1</button>
        </div>
      </div>
    );
  }

}

const node = document.querySelector('#root');
ReactDOM.render(<Counter />, node);

// Tu trouveras la démo sur JsFiddle: https://jsfiddle.net/Lak049xL/8/
```

## Utilisation des css inline

* Les css-inline nous permettent d'accrôitre la réutilisabilité de nos composants
* Ils permettent également d'avoir des styles "scopé" aux composants et ainsi
d'éviter des problème liés à la cascade.
* Même si le ton de l'article est plutôt "trollesque" cet article est intéressant à lire http://putaindecode.io/fr/articles/css/stop-css/
* À voir aussi une présentation de Vjeux (Un ingénieur de chez Facebook) sur les Css dans React : https://speakerdeck.com/vjeux/react-css-in-js


### React way

* Par défaut, React nous permet d'écrire du css dans nos composants de cette manière:

```javascript
const styles = {
  base: {
  	height: '100%',
    display: 'flex',
    alignItems: 'center',
    justifyContent: 'center',
    textAlign: 'center'
  },
  inner: {
  	boxShadow: '0 2px 0 #e8e8e8',
    padding: '30px',
    background: '#fff'
  },
  value: {
  	fontSize: '18px',
    textTransform: 'uppercase',
    color: '#2C3E50',
    fontWeight: 'bold',
    margin: '0 0 15px 0'
  },
  button: {
  	padding: '15px 30px',
    border: 0,
    background: '#E74C3C',
    fontSize: '12px',
    textTransform: 'uppercase',
    color: '#fff',
    fontWeight: 'bold',
    outline: 'none'
  }
};

class Counter extends React.Component {
	constructor() {
  	super();
    this.state = {
    	count: 0
    };
  }
  _increment(event) {
  	event.preventDefault();
    let newValue = this.state.count+1;
    this.setState({
    	count: newValue
    });
  }
  render() {
  	return (
    	<div style={styles.base}>
     		<div style={styles.inner}>
        	<p style={styles.value}>{this.state.count}</p>
          <button onClick={this._increment.bind(this)} style={styles.button}>Clique ici pour un +1</button>
        </div>
      </div>
    );
  }

}

const node = document.querySelector('#root');
ReactDOM.render(<Counter />, node);

// Tu trouveras la démo sur JsFiddle: https://jsfiddle.net/rosqu0x3/
```

* C'est pratique si vous qu'un composant soit "standalone" seulement ça ne gère pas les pseudo-classes,
donc pas de hover, de focus etc..

### Radium

* Radium est un module React dévloppé par Formidable http://stack.formidable.com/radium/
* Il s'inspire de ce que propose React pour la gestion des styles mais va plus loin en ajoutant :
    * Gestion des pseudo-classes
    * Gestion des media-queries
    * Ajoute automatiquement les vendors prefix
    * Ajoute le support des keyframes en inline
    * Permet le support des modifiers via les props ou state

* De cette manière notre composant est totalement autonome, je peux facilement
le trimballer de projet en projet sans trop d'effort.

```javascript
class Counter extends React.Component {
	constructor() {
  	super();
    this.state = {
    	count: 0
    };
  }
  _increment(event) {
  	event.preventDefault();
    let newValue = this.state.count+1;
    this.setState({
    	count: newValue
    });
  }
  render() {
  	return (
    	<div className="counter">
     		<div className="counter__inner">
        	<p className="counter__value">{this.state.count}</p>
          <button onClick={this._increment.bind(this)} className="counter__button">Clique ici pour un +1</button>
        </div>
      </div>
    );
  }

}

const node = document.querySelector('#root');
ReactDOM.render(<Counter />, node);

// Tu trouveras la démo sur JsFiddle: https://jsfiddle.net/4L7cfxx2/1/
```

* Les css-inline sont intéressants dans React pour plusieurs raisons:
    * Ils permettent d'avoir toute la logique de rendu dans un seul et même fichier (le .jsx/.js)
    * Ils permettent d'accroitre la réutilisabilité des composants sans efforts.
    * Et comme ils peuvent se baser sur les props ou les states pour gérer leur modifier,
    dans l'éco-système React c'est pas mal.
    * Dans une vue plus globale, les styles sont gérés en inline dans React native.

* Par contre, qu'est ce qui se passe si un intervennant sans trop de connaissance de React doit intervenir
sur la partie Css de l'application ? Là c'est problématique...

## Les css-modules

* Les css-modules nous permettent un peu de combiner l'écriture classique des css et le inline-style.
* Dans la pratique, un css module est attaché à un composant React, le fichier css possède des classes pouvant être
très générique (.base, .primary) et au moment du rendu les classes seront réécrite puis attaché au noeux du DOM
auquels elles doivent s'appliquer.

### Exemple

```scss
// ./components/counter/counter.scss
.base {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

  .inner {
    box-shadow: 0 2px 0 #e8e8e8;
    padding: 30px;
    background: #fff;
  }

    .value {
      font-size: 18px;
      text-transform: uppercase;
      color: #2C3E50;
      font-weight: bold;
      margin: 0 0 15px 0;
    }

    .button {
      padding: 15px 30px;
      border: 0;
      background: #E74C3C;
      font-size: 12px;
      text-transform: uppercase;
      color: #fff;
      font-weight: bold;
      outline: none;
      transition: background .2s ease-in;

      &:hover,
      &:focus {
          cursor: pointer;
          background: #e83523;
      }
```
```javascript
import React from 'react';
import CSSModules from 'react-css-modules';
import styles from './counter.scss';

class Counter extends React.Component {
	constructor() {
  	super();
    this.state = {
    	count: 0
    };
  }
  _increment(event) {
  	event.preventDefault();
    let newValue = this.state.count+1;
    this.setState({
    	count: newValue
    });
  }
  render() {
  	return (
    	<div styleName="base">
     		<div styleName="inner">
        	<p styleName="value">{this.state.count}</p>
          <button onClick={this._increment.bind(this)} styleName="button">Clique ici pour un +1</button>
        </div>
      </div>
    );
  }

}

export default CSSModules(Counter, styles);
```
* Voici le rendu Html
* Comme on peut le voir les classes sont re-écrite de cette manière le scope des styles est locale
et ne risque pas de rentrer en collision avec d'autres classes.

```html
<div data-reactroot="" class="app-components-counter-___counter__base___LyXjt">
    <div class="app-components-counter-___counter__inner___2bJo6">
        <p class="app-components-counter-___counter__value___2DzHy">0</p>
        <button class="app-components-counter-___counter__button___2AcjY">Clique ici pour un +1</button>
    </div>
</div>
```

* On définit une fichier (dans mon exemple) .scss que nous vennons attacher au composant (import styles from './counter.scss';)
* On attache les classes css au composant en utilisant la propriété "styleName"
* Et hop on est prêt !

## Exercice

- Allez dans le dossier exercice et faites ```git checkout exercice-5```
- Ouvrir le dossier formation-react-exercices dans votre éditeur préféré et lisez TODO.MD
