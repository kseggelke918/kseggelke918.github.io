---
layout: post
title:      "Regular Functions vs Arrow Functions and How They Handle `this`"
date:       2020-06-26 16:27:01 -0400
permalink:  regular_functions_vs_arrow_functions_and_how_they_handle_this
---


A big difference between Arrow Functions and Regular Functions is their value of `this`.  Arrow functions don't have their own `this`, instead `this` is bound to the value of `this` in the closest non-arrow parent function -- it's lexically bound.  What does this mean?  The arrow function will use `this` from the code that contains the arrow function.

Arrow functions will not redefine the value of `this` within their body!  This can make arrow functions easier to predict since the value of `this` remains the same throughout the lifecycle of the function.  It also means we don't have to use `.bind` to pass `this` into the function which is needed in regular functions.


**Example**

```
import React from 'react';

class App extends React.Component {
  render() {
    const person = { 
      name: 'John Doe', 
      arrowFunction:() => { 
      console.log("My name in Arrow is " + this.name); 
      }, 
      regularFunction(){ 
      console.log("My name in Regular is " + this.name); 
      } 
};
    return (
      <div className="App">
        {person.arrowFunction()}
        {person.regularFunction()}
      </div>
    );
    
  }
}

export default App;
```

What shows in the console is: 

```
// My name in Arrow is undefined
// My name in Regular is John Doe
```

Arrow functions are often the best choice, but just remember, they are best used when you need `this` to be bound to the context and not just the function itself.

