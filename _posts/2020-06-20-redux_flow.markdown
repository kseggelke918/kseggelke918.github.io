---
layout: post
title:      "Redux Flow"
date:       2020-06-20 22:29:22 +0000
permalink:  redux_flow
---


There are benefits to using Redux, however, you really should only use it when you need it.  There is a lot of set up involved, so make sure it's worth it!

Redux has a central "store" that holds the state for the entire application.  You can access that state from any component in the application without having to pass props.  There are 3 parts to Redux: actions, store and reducers.

**Store: **

The store holds the entire application's state.  There is only one store that can be accessed or updated.  To create a store, import 'createStore' from redux:

```
import { createStore } from 'redux'
```

Then create a store variable that calls the createStore function calling the reducer(s) in the argument.

```
const store = createStore(reducer)
```

You could also user Redux Thunk when creating the store.  Thunk is a middleware that extends the store's capabilities and allows you to do asynchronous logic (like fetch requests) that interacts with the store.  To use Thunk, import it from 'redux-thunk' and add it to the createStore method.  At the same time, you can allow Redux Devtools to be used in Chrome. 

```
const composeEnhancer = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
  
  const store = createStore(reducer, composeEnhancer(applyMiddleware(thunk)))
```

Thunk middleware allows action creators to return a function instead of an action.

**Actions: **

Actions are how you send data from the application to the Redux store.  To send information to the store, we use store.dispatch().  

Actions are just plain JavaScript objects that need a type property so it knows what type of action is being performed.

```
export const clearCurrentUser = () => {
    return {
        type: 'CLEAR_CURRENT_USER'
    }
}
```

Actions often have what is generally considered "payload" that contains information the action needs.

```
export const setCurrentUser = user => {
    return {
        type: "SET_CURRENT_USER",
        user: user 
    }
}
```

Here, the user is really: 

```
user: {
      username: 'test_user',
			password: 'password',
			name: 'Susie Test'
```


Action Creators: 
It is east to confuse actions with action creators.  While an action is a plain old JavaScript Object, action creators create actions.  In order to initiate a dispatch, you would pass the result to a dispatch function (with or without an argument)

```
dispatch(clearCurrentUser())
```

This is often done as part of a larger method like in the example below, dispatch is called while also sending a logout request to the backend via fetch:


```
export const logout = () => {
    return (dispatch) => {
        dispatch(clearCurrentUser())
        return fetch('http://localhost3000/api/v1/logout', {
           credientials: 'include', 
           method: 'DELETE' 
        })
    }
}
```

The dispatch() function can be accessed directly from the store, thought it's more often accessed using react-redux's connect() function 

**Reducers: **

Reducers are what's considered 'pure functions' that take the current state, do some action, and then return the new state.

```
export default (state = null, action) => {
    switch (action.type) {
        case 'SET_CURRENT_USER': 
            return action.user 
        case "CLEAR_CURRENT_USER":
            return null 
        default:
            return state
    }
}
```

Reducers often have a default or initial state set in the argument.  In the above example, we set the user to the user the action was called on and we return null when clearing the user.

