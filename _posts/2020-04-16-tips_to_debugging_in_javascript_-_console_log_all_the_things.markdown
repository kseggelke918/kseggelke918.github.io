---
layout: post
title:      "Tips to Debugging in JavaScript - console.log all the things"
date:       2020-04-16 04:49:03 +0000
permalink:  tips_to_debugging_in_javascript_-_console_log_all_the_things
---


Debugging in JavaScript turned out to be quite different than debugging on the backend.  After getting used to pry and byebug and going to the terminal or rails console for most of my debugging needs, it was time to learn how to debug using developer tools.  A couple helpful items to use when debugging is **console.log** and the **brower developer tools**.

**CONSOLE.LOG()**

It isn't always intuitive to know what data you're working in and what format you're getting it.  When trying to get a handle on what data is being used or called, put it in a console.log and view it from your developer tools in the console.  Then you can view and play with the data.

```
document.addEventListener('DOMContentLoaded', (event) => {
    fetch("http://localhost:3000/houses")
        .then (function(response){
            return response.json();
        })
        .then (function(json){
            json.data.forEach(function(houses){
                House.createHouse(houses)
                console.log(houses)
            })
        })
})
```

![](https://github.com/kseggelke918/javascript_project_4/blob/master/Screen%20Shot%202020-04-15%20at%2010.13.38%20PM.jpg?raw=true)

By reviewing what shows in the console, I know how to access each piece of data.  To access my attributes, I need `houses.attributes` or to access a specific attribute, I need `houses.attributes.name` and to access my belongs_to relationships, I need `houses.attributes.characters`.

**BREAK POINTS**

Another helpful debugging tool is to use the breakpoints in the developer tools.  When debugging, you may need to see what is happening at certain points in your code and maybe even ask your code "questions" to see what it's interpretting.  To do this, select 'sources' in the developer tools and just click on lines of code where you want your program to pause so you can ask it questions.  You can add as many breaks as you want.

![](https://github.com/kseggelke918/javascript_project_4/blob/master/Screen%20Shot%202020-04-15%20at%2010.39.58%20PM.jpg?raw=true)

In the example above, I placed a break point by click on line 11.  Then while executing, the code paused on line 11.  In the console, I can ask questions about code that has already been run, like what the value of 'div' is.  This is very useful when your program is running through multiple lines of code or multiple functions while completing a task.  You can put a break (or many!) in every function to make sure the right data is being sent to each function and see how that data is being interpretted making it easier to target the line of code causing the error(s).

Happy Coding!
