---
layout: post
title:      "What is React Router?"
date:       2020-12-17 18:09:00 +0000
permalink:  what_is_react_router
---


Usually when you visit a web page you would have to fetch that page from the server; however, with React and React Router when visiting a page you would just load up a specific component. No need to fetch a page from the server. This makes the page have a more “app-like” feeling. In this post I am going to show you how I implemented React Router in my own React project.
The very first thing that needs to be done is installing the React Router library. Simply change into your React project directory and install the it like so.
npm install react-router-dom
Next I created some components that will serve as our “pages”. In my application I decided I would have a Home and MyList components. Home component will have a ‘/’ path, MyList will have a ‘/activities’ path. These components represent different pages even though this is still a Single Page Application (SPA).
They components will look very simple. Here is the Home component.

```
import React from 'react';
const Home = () => {
  return (
    <div>
      <h1>Home</h1>
    </div>
  );
}
export default Home;
And this is the MyList component.
import React from 'react';
const Mylist = () => {
  return (
    <div>
      <h1>MyList</h1>
    </div>
  );
}
export default MyList;
```

These are going to be the children components of our parent App.js.

```
import React, { Component } from 'react';
import Home from './Components/Home'
import MyList from './Components/MyList'
class App extends Component {
  render() {
    return (
      <div>
        <Home />
        <MyList />
      </div>
    )
  }
}
```
The code that we have right now would just render every component on top of each other in the same page. We want to be able to only load certain components depending on the URL. So how does React Router help us with that? Well let’s take a look.

```
import React, { Component } from 'react';
import Home from './Components/Home'
import MyList from './Components/MyList'
import {BrowserRouter as Router, Route} from 'react-router-dom'
class App extends Component {
  render() {
    return (
      <Router>
        <div>
          <Route exact path='/' component={Home} />
          <Route exact path='/activities' component={MyList} />
        </div>
      </Router>
    )
  }
}
```

So from the top we can see that we are importing BrowserRouter and Route from the react-router-dom library. BrowserRouter as Router just means that we are going to be renaming BrowserRouter to shorten it and make it a tad bit easier to use. Inside the return statement we have wrapped our div element with a Router component. This will allow use to make use of Routes. Inside the div element we have two Route components. We give them two props, one of them is to specify the path and the other is to assign the component we want to load when we are inside the specified path. We specify exact path so that the component renders only if the path in the url is the exact same match as the path specified. That is all that we need for our routes to work. Now you can navigate to the paths and see them in action.

There are a few reasons why client-side routing is a good idea.

* Current page doesn’t reload to get to a new view. This gives the user a more pleasant Single Page Application feel to your app.
* Since there is not much data that needs to be processed switching between routes is much faster.
* Transitions and animation can be more easily implemented.

Like many things in programming client-side routing also has its drawbacks.

* The initial loading time is much longer since the whole application needs to load up at once.
* Some additional views will increase initial loading times even though the user might not even visit those pages.
* It requires a library, which means that there is more code and more dependencies, unlike server-side routing.

Either way there is a place for client-side routing so the best way to know when is the right time to use it is to practice making projects with routes in them. Happy coding!
