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
## Building the Redux app.
If you don't have a basic understanding of Redux, no worries as we won't dive alot into Redux and basic terms will be explained in the course of building.