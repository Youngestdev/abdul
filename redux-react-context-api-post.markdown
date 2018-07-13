---
layout: post
 title: "React Context API—A Replacement for Redux?"
 description: "Learn how to migrate from a React application that is based on Redux to the new React Context API."
 longdescription: "In this article, We'll be discussing how to move Redux based apps to use the new React Context API. Also, we'll be discussing when we should use Redux or React Context API."
 date: 
 category: Technical Guide, Frontend, React
 author:
   name: Abdulazeez Abdulazeez Adeshina
   url: https://twitter.com/kvng_zeez
   avatar: 
   mail: laisibizness@gmail.com
 design:
   bg_color: "#1A1A1A"
   image: https://cdn.auth0.com/blog/reactjs16/logo.png
 tags:
 - react
 - redux
 - react-context
 - context
 - auth0
 - frontend
---

**TL;DR:** React's context API isn't a new thing in the React's ecosystem but React 16.3.0's improvement on the Context API is overwhelming such that it reduces/overthrows the need for Redux/Advanced state management libraries in small scale apps.
In this article, we'll be looking at how the new React Context API replaces the need for Redux by building a small app.
{% include tweet_quote.html quote_text="Learn how to migrate from Redux to the new React Context API in this practical tutorial." %}

## What's Redux ?
Redux is a JavaScript library used for state management in popular JavaScript libraries such as **React** and **Angular** whereas *state management* simply means handling changes that occur upon running / executing a particular task.

> Note that state management can be performed locally using React's `setState` but is limited to small / byte-sized apps.

## Basic things to know about Redux.

- The state of the entire app is stored in a single object within a single store.
- To change the state, you need to dispatch an action that describes what needs to happen.
- You cannot change properties of objects or make changes to existing arrays. You should always return a new reference to a new object or new array.

## Back to business - Building our test App.

Since we'll be demonstrating how React's new Context API replaces Redux, we'll build our app first using Redux for state management and second using React's new Context API. The example app we'll be building is an app that consists of some foods and their origin, we'll be adding a search functionality that enables the user get the list of food using a certain keyword.

### Project Requirements.

1. NodeJs and NPM.
2. `create-react-app` ( React boilerplate scaffolder )

If you haven't installed **NodeJS** and **npm**, check out the [Official installation procedures](https://nodejs.org/en/download/) to install NodeJs and Npm. 

### Setting up the project.

After installing the above, we'll install the boilerplate scaffolder, `create-react-app`. To do this, we run the command:
```sh
npm i -g create-react-app
```

## Building the Redux app.

If you don't have a basic understanding of Redux, no worries as we won't dive into Redux alot and basic terms will be explained in the course of building.

### Starting Off.

We start by scaffolding a react app from the general boilerplate using the command:

```sh
$ create-react-app redux-version
```

##### Waiting for a few seconds.
Our app has been generated so we'll have to install the needed libraries: `redux` and `react-redux`. Navigate to the app's directory, *redux-version* and run the command from your terminal / command prompt.

```sh
npm i --save redux react-redux
```

> **Note:** Redux is the main library and `react-redux` helps facilitate the interaction between React and Redux. In short, it acts like a proxy.

### Writing codes - Powering our engines!.

We begin writing our codes by kickstarting our code editors/ide(s) and then navigating into the *src* folder and creating the following files:

- reducers.js
- actions.js
- food.js

#### Creating a Reducer on Redux.

The `reducers.js` file holds the Redux store whereas the `store` is a place where the single source of truth of the state in our app is kept.

Let's write some code...

```js
import Food from './food';

// Initializing state.
const initialState = {
    food: Food,
    searchTerm: '',
};

// Defining function that allows us to manage state in our app.

export default function food(state = initialState, action) {
    // switch between the action type
    switch (action.type) {
        case 'SEARCH_INPUT_CHANGED':
            const {searchTerm} = action.payload;
            return {
                ...state,
                searchTerm: searchTerm,
                food: searchTerm ? Food.filter(
                    food.name.toLowerCase().indexOf(searchTerm.toLowerCase()) > -1 )
                    : Food,
        };
        default:
          return state;
        }
    }

```

> Actions are payloads of information that send data from your application to your store. They are the only source of information for the store.

Remember we created a `Food.js` file, we imported it's content in the `reducer.js` file. But, it's empty, let's write some code.
I'll be writing some examples of food and their origin in an object form enclosed in an array *as per react rules*.

```js
export default [
  {
    name: "Chinese Rice",
    origin: "China",
    continent: "Asia"
  },
  {
    name: "Amala",
    origin: "Nigeria",
    continent: "Africa"
  },
  {
    name: "Banku",
    origin: "Ghana",
    continent: "Africa"
  },
  {
    name: "Pão de Queijo (Brazillian cheese bread",
    origin: "Brazil",
    continent: "South America"
  },
  {
    name: "Ewa Agoyin",
    origin: "Nigeria",
    continent: "Africa"
  }
];
```

Let's navigate into our `actions.js` file. In our `actions.js` file, we define the SEARCH_INPUT_CHANGED action as defined in the `reducers.js` file.

```js
function searchTermChanged(searchTerm) {
    return {
        // define action type
        type: 'SEARCH_INPUT_CHANGED',
        // define action payload
        payload: {searchTerm},
    };
}
export default {
  searchTermChanged,
};
```

The react-redux library comes to play in a few seconds. The library has a component, `Provider` that makes our Redux Store available to the rest of our app's component.

> Providers make our app's state available to all components, to achieve state's availability, we'll encapsulate the *App* component into a Provider.

### Creating the store & Constructing state's general availability.

We'll now create a Store using all the details written down in the reducers file, we'll replace the pre-existing code in `index.js` with the one below:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import {Provider} from 'react-redux';
import {createStore} from 'redux';
import reducers from './reducers';
import App from './App';

// Creating the store using the reducers info.
// That's because reducers are the building blocks of a Redux Store.
const store = createStore(reducers);

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);
```

### Building the main interface.

We've been working on the state management with Redux and neglected the end output. Well, let's navigate to our App.js and replace the code in it with the one below:

{% highlight js %}
{% raw %}
import React from "react";
import { connect } from "react-redux";
import actions from "./actions";
import "./App.css";

// Define our component.
function App({ food, searchTerm, searchTermChanged }) {
  return (
    <div>
      <form>
        <div className="search">
          <input
            type="text"
            name="search"
            placeholder="Search"
            value={searchTerm}
            onChange={e => searchTermChanged(e.target.value)}
          />
        </div>
      </form>
      <table>
        <thead>
          <tr style={{ textAlign: "center" }}>
            <th>Name</th>
            <th>Origin</th>
            <th>Continent</th>
          </tr>
        </thead>
        <tbody>
          {food.map(theFood => (
            <tr key={theFood.name}>
              <td>{theFood.name}</td>
              <td>{theFood.origin}</td>
              <td>{theFood.continent}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
// Export our component.
export default connect(store => store, actions)(App);
{% endraw %}
{% endhighlight %}

> Connect as used in the export statement, is a High Order Component (HOC) that enables the accessibility and availability of different set of properties of a state to children component.

Let's power on our engine finally, run the command:

```sh
npm run start
```

### Beautifying Output.

I bet the output looks rough, so let's replace the content in the App.css with this:

{% highlight css %}
{% raw %}
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

th {
  background-color: #eee;
}

input {
  min-width: 300px;
  border: 1px solid #999;
  border-radius: 2px;
  line-height: 25px;
}
{% endraw %}
{% endhighlight %}

### Pheew. Done and Dusted.

Well, we're actually through with our Redux version. As I said earlier, Redux isn't necessarily needed as the size of the app matter talkless of the introduction of the new Context API.

#### So What's a Context API ?

Context provides a way to pass data through the component tree without having to pass props down manually at every level. In React, data is often passed from a parent to its child component as a prop. — Context, React.

##### Is the Context API a new thing ?
No, it isn't. Even Redux uses the context API: `<Provider />`

#### Using the new React Context API - Things to take note of.

Using the new Context API depends on three main steps:
1. Passing the initial state to `React.createContext`. This function then returns an object with a Provider and a Consumer.
2. Using the Provider component at the top of the tree and making it accept a prop called value. This value can be anything!
3. Using the Consumer component anywhere below the Provider in the component tree to get a subset of the state.

### Rewritting Our App - Moving From Redux To Context API.
Sigh, we won't really do alot of work anymore. Migrating from Redux to the new Context API is quite easy.
The first step is removing **every** trace of **Redux** from our app. We'll start by removing the libraries from our app.
```sh
npm rm redux react-redux
```
Then, we remove the below codes from App.js

```js
// Remove these lines of code.
import {connect} from 'react-redux';
import actions from './actions';
```

and replace the last line of App.js from `export default connect(store => store, actions)(App);` to `export default App;`.

With these changes in place, we can rewrite your app with React Context API. So, to convert the previous app from a Redux powered app to using the Context API, we will need a context to store the app's data (this context will replace the Redux Store). Also, we will need a Context.Provider component which will have a state, props, and a normal React component lifecycle.
Therefore, we will create a `providers.js` file in the **src** folder and add the following code to it:

```js
 import React from 'react'; 
 import Food from './food';

const DEFAULT_STATE = { allFood: Food, searchTerm: '', };
export const ThemeContext = React.createContext(DEFAULT_STATE);
export default class Provider extends React.Component { state = DEFAULT_STATE;
searchTermChanged = searchTerm => { this.setState({searchTerm}); };
render() { 
    return ( 
        <ThemeContext.Provider value={{ ...this.state, searchTermChanged: this.searchTermChanged, }} > {this.props.children} </ThemeContext.Provider> ); } }
```

The Provider class is responsible for encapsulating other components inside the ThemeContext.Provider so these components can have access to our app's state and to the searchTermChanged function that provides a way to change this state.

To consume these values later in the component tree, we will need to create a `ThemeContext.Consumer` component. This component will need a render function that will receive the above value props as arguments to use at will. So, create another file called `consumer.js` in the src directory and write the following code into it:

```js
import React from 'react';
import {ThemeContext} from './provider';

export default class Consumer extends React.Component {
  render() {
    const {children} = this.props;

    return (
      <ThemeContext.Consumer>
        {({allFood, searchTerm, searchTermChanged}) => {
          const food = searchTerm
            ? allFood.filter(
              food =>
                food.name.toLowerCase().indexOf(searchTerm.toLowerCase()) > -1
            )
            : allFood;

          return React.Children.map(children, child =>
            React.cloneElement(child, {
              food,
              searchTerm,
              searchTermChanged,
            })
          );
        }}
      </ThemeContext.Consumer>
    );
  }
}
```
Now, to finalise the migration, we will open our `index.js` file and inside the `render()` function, wrap the App component with the Consumer component. Also, wrap the Consumer inside the Provider component. Exactly as shown here:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import Provider from './provider';
import Consumer from './consumer';
import App from './App';

ReactDOM.render(
  <Provider>
    <Consumer>
      <App />
    </Consumer>
  </Provider>,
  document.getElementById('root')
);
```
### Voom..

We're finally done !. Yipee.

# Redux vs Context API - When to use either of them.

Redux is an **advanced** state management library that should be used when building large scale react apps. That is as a result of fast caching and easing the loading actions conducted by React when loading new changes effected by the state. The Context API on the other hand, should be used in small scale react apps where byte-sized changes are made. Using the Context API, we do not have to write a lot of code to work out the logic exhibited by state changes.

## In summary,

Redux should be used when building large scale apps and the Context API should be used when building small scale apps.

# Conclusion.

We surely prefer the Contextual API method of handling state management right ? Yes.

