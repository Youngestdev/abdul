**TL;DR:** React's context API isn't a new thing in the React's ecosystem but React 16.3.0's improvement on the Context API is overwhelming such that it reduces/overthrows the need for Redux/Advanced state management libraries in small scale apps.
In this article, we'll be looking at how the new React Context API replaces the need for Redux by building a small app.
# What's Redux ?
Redux is a javascript library used for state management in popular javascript libraries such as **React** and **Angular** whereas *state management* simply means handling changes that occur upon running / executing a particular task.

> Note that state management can be performed locally using React's **setState** but is limited to small / byte-sized apps.

## Basic things to know about Redux.
+ The state of the entire app is usually stored in a single object within a single store.
- To change the state, you need to dispatch an action that describes what needs to happen.
* You cannot change properties of objects or make changes to existing arrays. You should always return a new reference to a new object or new array.

# Back to business - Building our test App.
Since we'll be demonstrating how React's new Context API replaces Redux, we'll build our app first using Redux for state management and second using React's new Context API.
## Project Requirements.
{% highlight text %}
{% raw %}
1. NodeJs and Npm
2. create-react-app ( React boilerplate scaffolder )
{% endraw %}
{% endhighlight %}

### Installing requirements ( if you don't have ).
If you haven't installed **NodeJS** and **npm**, check out the [Official installation procedures](https://nodejs.org/en/download/) to install NodeJs and Npm. 
### Setting up the project.
After installing the above, we'll install the boilerplate scaffolder, **create-react-app**. To do this, we run the command:
{% highlight text %}
{% raw %}
npm i -g create-react-app
// or
sudo npm i -g create-react-app
{% endraw %}
{% endhighlight %}

## Building the Redux app.
If you don't have a basic understanding of Redux, no worries as we won't dive into Redux alot and basic terms will be explained in the course of building.

### Starting Off.
We start by scaffolding a react app from the general boilerplate using the command:
{% highlight text %}
{% raw %}
$ create-react-app redux-version
{% endraw %}
{% endhighlight %}
##### Waiting for a few seconds.
Our app has been generated so we'll have to install the needed libraries: **redux** and **react-redux**. Navigate to the app's directory, *redux-version* and run the command from your terminal / command prompt.
{% highlight text %}
{% raw %}
npm i --save redux react-redux
// or
sudo npm i --save redux react-redux
{% endraw %}
{% endhighlight %}
> **Note:** Redux is the main library and react-redux helps facilitate the interaction between React and redux. In short, it acts like a proxy.

### Writing codes - Powering our engines!.
We begin writing our codes by kickstarting our code editors/ide(s) and then navigating into the *src* folder and creating the following files:
+ reducers.js
+ actions.js
+ food.js

#### reducers.js
The reducers.js file holds the redux store whereas the store is a place where the single source of truth of the state in our app is kept.

Let's write some code...
{% highlight js %}
{% raw %}
import Food from './food';

// Initializing state.
const initialState = {
    food: Food,
    searchTerm: '',
};
// Defining function that allows us to manage state in our app.
// Function 
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
{% endraw %}
{% endhighlight %}
> Actions are payloads of information that send data from your application to your store. They are the only source of information for the store.

Remember we created a **Food.js** file, we imported it's content in the *reducer.js* file. But, it's empty, let's write some code.
I'll be writing some examples of food and their origin in an object form enclosed in an array *as per react rules*.
{% highlight js %}
{% raw %}
export default [
  {
    name: "Chineese Rice",
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
{% endraw %}
{% endhighlight %}

Let's navigate into our actions.js file. In our actions.js file, we define the SEARCH_INPUT_CHANGED action as defined in the *reducers.js* file.
{% highlight js %}
{% raw %}
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