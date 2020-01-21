---
layout: post
title: Course Note of Codecademy Introduction To JavaScript
categories: [blog ]
tags: [Coding, JavaScript, ]
description: "Learn note of Introduction To JavaScript"
image:
  feature: windows.jpg
  credit: Azeril
  creditlink: azeril.com
---


Note about Codecademy JavaScript Course [Introduction To JavaScript](https://www.codecademy.com/learn/learn-javascript)


What can we use JavaScript for?

* make websites respond to user interaction
* build apps and games (e.g. blackjack)
* access information on the Internet (e.g. find out the top trending words on Twitter by topic)
* organize and present data (e.g. automate spreadsheet work; data visualization)

### Confirm and prompt

- confirm `confirm(“words”);` Web action confirm
- prompt `prompt();` Ask for input



### Data Types

There are three essential data types in JavaScript: strings, numbers, and booleans.

- a. numbers   (e.g. 4.3, 134)
- b. strings (e.g. "dogs go woof!", "JavaScript expert")
- c. booleans ~ A boolean is either true or false.  (e.g. false, 5 > 4)

### Math

* ( ): control order of operations
* * and /: multiplication and division
* - and +: subtraction and addition
* % modulo. ~ When % is placed between two numbers, the computer will divide the first number by the second, and then return the remainder of that division. ~ module it good at testing divisibility. 
 We can use modulos in comparisons, like this: `10 % 2 === 0 //evaluates to true` And `7 % 3 === 0 //evaluates to falsebecause there is 1 left over.`


### Comments 

We write a single line comment with //and a multi-line comment with /* and */.

Two types of code comments in JavaScript:

* A single line comment will comment out a single line, and is denoted with two forward slashes // preceding a line of JavaScript code.
* A multi-line comment will comment out multiple lines, and is denoted with /* to begin the comment, and */ to end the comment.

### console.log

console.log() ~ Prints into the console whatever we put in the parentheses.This is commonly called printing out.

We can use the + operator from earlier to interpolate (insert) a variable into a string.

List of comparison operators:

Comparisons need two things to compare and they will always return a boolean (true or false).

Comparison operators, like <, >, <=, and >= can compare two variables. After they compare, they always return either true or false.

```
* > Greater than
* < Less than
* <= Less than or equal to
* >= Greater than or equal to
* === Equal to (To check if two things equal each other)
* !== Not equal to 
```

Logical Operators, like &&, ||, !==, and !, can compare two variables to see if a certain condition exists:

```
* && Checks if both sides are true.
* || Checks if either side is true.
* ! Changes a variable that is true to false, and vice versa.
* !== Checks if both sides are not equal.
```


### Conditionals

-  if

An if statement is made up of the if keyword, a condition like we've seen before, and a pair of curly braces { }. If the answer to the condition is yes, the code inside the curly braces will run.

```
if( "myName".length >= 7 ) {
    console.log("You have a long name!");
}
```

- if…else

if the condition is true, then only the code inside the first pair of curly braces will run. Otherwise, the condition is false, so only the code inside the second pair of curly braces after the elsekey word will run.


- if…else if…else

We can add extra conditions with to if/else statements with else if conditions.

- switch

To deal with times when you need many else if conditions, we can turn to a switch statement to write more concise and readable code.

```
switch (condition) {
    case “”: 
        …;
        break;
    case “”:
    repeat_repeat
    repeat…
    default:
        …;
        break;
}
```
### Substrings

`”some word".substring(x, y)` 
~ where x is where you start chopping and y is where you finish chopping the original string. 
~ Each character in a string is numbered starting from 0.

## Variables

 We store data values in variables. We can bring back the values of these variables by typing the variable name.Then, if the variable's value changes we only have to change the variable's value instead of re-writing the entire program.

Variables are useful in two ways:
* They allow us to use the same value over and over, without having to write a string or other data type over and over.
* More importantly, we can assign variables different values that can be read and changed by the program without altering our code.


`var varName = data;`

A variable's value is easily changed. Just pretend you are creating a new variable while using the same name of the existing variable.

## Random Number

- `Math.random();`

This code will return a random number between 0 and 1. JavaScript will generate a random number for us with this code.

- `Math.floor(Math.random() * 50);`
 Math.floor will take a decimal number, and round down to the nearest whole number. 


## Control Flow

In programming, making decisions with code is called control flow.

This sentence looks fairly similar when we write it with JavaScript. See for yourself:


    var needCoffee = true;
    if (needCoffee) {
        console.log('Finding coffee');
        } else {
        console.log('Keep on keeping on!');
    }


- If the variable needCoffee is true, JavaScript will run one code block, and if a variable is false, it will run another.
- `needCoffee` is the condition we are checking inside the if's parentheses. Since it is equal to true, our program will run the code between the first opening curly brace { (line 2) and the first closing curly brace } (line 4). It will completely ignore the else { ... } part. In this case, we'd see 'Finding coffee' log to the console. Note: Code between curly braces are called blocks. if/else statements have two code blocks.
- If we adjusted needCoffee to be false, only the else's console.log will run.
- if/else statements are how we can process yes/no questions programmatically.

Why is there a \ in 'I lead a muggle\'s life.'? Since the string is surrounded by single quotes, we can use a back slash to add a single quote within the string. This is called escaping a character.


## Function


A function is a block of code designed to perform a task.

Functions are like recipes. They take data or variables, perform a set of tasks on them, and then return the result. The beauty of functions is that they allow us to write a chunk of code once, then we can reuse it over and over without writing the same code over and over.

function fun_name(parameter) {
    xxx;
}

If we want to give a function a number. To do this, we can use parameters. Parameters are variables that we can set when we call the function. 

when we call fun_name(x), the x is called an argument.Arguments are provided when you call a function, and parameters receive arguments as their value.

- return ~ To return a result, we can use the return keyword. 

* Functions are written to perform a task.
* Functions take data or variables, perform a set of tasks on them, and then return the result.
* We can define parameters when calling the function.
* When calling a function, we can pass in arguments, which will set the function's parameters.
* We can use return to return the result of a function which allows us to call functions anywhere, even inside other functions.


## Scope

* Scope is the idea in programming that some variables are acessible/inaccessible from other parts of the program.
* Global Scope refers to variables that are accessible to every part of the program.
* Functional Scope refers to variables created inside functions, which are not accessible outside of its block.


## Array

We can also access each individual character in a string the same way you do with arrays.

- push()
- pop()

* Arrays are lists and are a way to store data in JavaScript. Each item inside of an array is at a numbered position. Arrays are created with brackets [].
* We can access one item in an array using it's numbered position, with syntax like: myArray[0].
* Arrays have a length property, which allows you to see how many items are in an array.
* Arrays also have their own methods, including `push` and `pop`, which add and subtract items from an array, respectively.


## Loop

Loops are especially helpful when we have an array where we'd like to do something to each item in the array, like logging each item to the console.

* `for` loops allow us to repeat a block of code a known amount of times.
* We can use a `for` loop inside another `for` loop to compare two lists.
* `while` loops are for looping over a code block an unknown amount of times.

## jQuery

The Document Object Model, commonly referred to as the DOM', is the term for elements in an HTML file. Elements are any HTML code denoted by HTML tags, like \<div>, \<a>, or \<p>.

To better interact with DOM elements, we can use a library. A library is a set of code that contains useful pre-written functions that help with certain tasks.

* Include jQuery in our project. jQuery is a library, which means it is a set of code in a file, therefore we will need to link that file in our HTML in order to access it. Once we link it in our HTML file, we can use its functions and syntax in our js/main.js file.
* Once linked, we'll need to make sure our HTML is loaded before we run our jQuery and JavaScript code. 
The link to jQuery needs to be above the link to the js file, which will give js file access to the jQuery library.

```
$(document).ready(main);
```

jQuery has a built in function to check if the page is ready before it will run our code. `main` here is a callback, which means that our code will wait until the `document`(the DOM) is loaded,or ready.

How to select HTML element?

```
document.getElementsByClassName(‘skillset’); // in JavaScript
$(‘.skillset’); // in jQuery
```

example: `var $skillset = $('.skillset');` 

It is a common convention to name variables that hold jQuery selectors with a dollar sign `$`.

- hide() ~ hide elements with jQuery
- fadeIn() ~  fade an element in over a period of time in milliseconds. 
- on(‘click’) ~ It’s listens to an element for a click event
- show() ~ To make projects visible
- toggle() ~ toggle will hide or show an element, each time it is triggered. 
- toggleClass() ~  Toggle a CSS class on the jQuery selector it's connected to. If the element has the class applied to it, toggleClass will remove it, and if the element does not have the class, it will add it.
- next() ~ We can attach next to any jQuery selector to select the next direct element. Then, we can attach any jQuery function to next(). 
- text() ~ With this function, we can change the text of an element. 
- slideToggle() ~ Animate an element’s entrance and exit. It can be called directly on a jQuery selector.Also, it takes a parameter of milliseconds that the animation should last. 
- `$(this)` ~ `this` is a JavaScript keyword.`this` will only acting the class of the one that is worked.`$(this)` behaves just like our other selectors.

