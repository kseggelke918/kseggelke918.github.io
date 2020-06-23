---
layout: post
title:      "Setting Up Redux Store"
date:       2020-06-23 15:56:08 -0400
permalink:  setting_up_redux_store
---

WARNING!  Redux can be overkill for small applications so only use if the application warrants it due to scale or complexity.

STEP 1: SET UP IMPORTS

Imports are needed from redux, react-redux and redux-thunk.  These imports can all be in one file or in separate files depending on the file structure.

```
import { createStore, applyMiddleware, compose, combineReducers  } from 'redux' 
import thunk from 'redux-thunk'
import { Provider } from 'react-redux'
```

**createStore:** creates a Redux store that holds the complete state tree of the application.
**applyMiddleware:** most commonly used to support asynchronous actions by allowing dispatch of async actions in addition to normal actions; incorporates thunk in the store
**compose:** allows middlewares to be combined, takes in arguments of the functions to compose
**combineReducers:** combines multiple reducers; turns an object whose values are different reducing functions into a single reducing function that can be passed to createStore.
**thunk:**  recommended middleware for basic Redux to allow asynchoronous actions like fetch requests
**provider:** makes state global; wrapping a component gives that component and all its children access to the store

STEP 2: USE PROVIDER

Wrapping App inside Provider will give App.js and all it's children access to the store.

```
ReactDOM.render(
      <Provider>
			      <App />
			</Provider>,
			document.getElementById('root')
);
```

STEP 4: SET UP REDUCERS 

Create a separate file under SRC for each reducer with a switch statement.  The reducer takes in 2 arguments -  state and the action and then cases are created for each possible action.  Typically, state should have a default argument and also a default case that just returns however state was set as the initial default.

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


STEP 5: SET UP COMBINEREDUCERS() IF NECESSARY

If the application will need multiple reducers, set it up as one reducer argument with combinedReducers.  Make sure to import the reducers that are being combined.

```
import currentUser from './reducers/currentUser.js'
import loginForm from './reducers/loginForm.js'
import signupForm from './reducers/signupForm.js'

const reducer = combineReducers({
     currentUser, 
		 loginForm, 
		 signupForm
})
```

STEP 6: SETUP COMPOSEENHANCER

Note: window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ provides chrome with access to the Redux dev tools

```
const composeEnhancer = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
```

STEP 7: CREATE STORE

Using what we've set up in steps 1 - 5, we can now use createStore

```
const store = createStore(reducer, composeEnhancer(applyMiddleware(thunk)))
```

STEP 8: GIVE APP ACCESS TO STORE BY PASSING IN STORE AS PROVIDER PROP

```
ReactDOM.render(
      <Provider store={store}>
			      <App />
			</Provider>,
			document.getElementById('root')
);
```

The store is now created and can be viewed in the Redux dev tools.  From here, create actions and components for the application.  

STEP 9: USE CONNECT

Import { connect } from 'react-redux' to give the component access to the store

```
import { connect } from 'react-redux'
```

And use connect in the default statement

```
export default connect(mapStateToProps, { updateSignupForm })(Signup)
```

The mapStateToProps function provides the component access to the store as props while the second argument in connect is mapDispatchToProps which gives us access to dispatch (update) the store from the component.

A mapStateToProps function must be written, while the mapDispatchToProps can be written as shorthand as seen above.

```
const mapStateToProps = ({ currentUser }) => {
    return {
      currentUser,
      loggedIn: !!currentUser
    }
  }
```

