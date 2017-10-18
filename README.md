# es6-notes

## Arrow Functions in ES6 

### Arrow functions =>
* are anonymous, they do not need names.
* no need for an explicit return keyword unless you are returning a function block, can return implicitly.
* are more concise than non arrow functions
* do not rebind the value of ‘this’ when you use an arrow function inside of another function (lexically scoped).
* useful as callbacks + have more expressive closure syntax
* readable inside of array prototype methods.

#### How to convert from a regular function to an arrow function -
```js
names = ["Alisa", "Amanda", "Alex"];

//function before any modification
const fullNames = names.map(function(name) { return `${name} is cool`});

//no need for the "function" in the definition, added the fat arrow after the parameter and before the block.
const fullNames = names.map((name) => { return `${name} is cool`});

// only need parenthesis around the argument if you use more than one, if no arguments indicate empty () declaration
const fullNames = names.map(name => { return `${name} is cool`});

//removed the explicit return, arrow functions use implicit returns
const fullNames = names.map(name => `${name} is cool`);

//if you want to do more than one thing you'll need to use the function block {}
const fullNames = names.map(name => { 
	`${name} is cool`; 
	...etc... 
});
```

#### How to return an object in an arrow function - 
If you want to return an object literal and not the actual function block, that will contain some logic or whatever so like, format it like => ({my obj}) instead of => {exp < 0 ? 1 : 0} 
```js
const race = '100m Dash';
const winners = ['Alisa', 'Patricia', 'Tkaczyk'];
const win = winners.map(winner, i) => ({name:winner, race:race, place: i});

//notice the variable race and property name race are assigning to themselves this may seem redundant.
//you can just pass in race itself and it will do the same thing because of the object scope.
const win = winners.map(winner, i) => ({name:winner, race, place: i});

//--- example 3
const ages = [10,2,35,67,32,67,88,92,11,67,100,6];
const old = ages.filter(age => age >= 60);
//filter will return true if age >= 60, and then return it to the left hand side of the arrow to filter so that old ages are in the new old array.


//--- the 'this' keyword does not get rebound.
const box = document.querySelector('.box');
box.addEventListener('click'), function() {
	console.log(this);
}};
// console will display <div class="box"></div> ... the div that was clicked on. Great.
//Now if we convert this to an arrow function.
box.addEventListener('click'), () => {
	console.log(this);
}};
// console will display window because when you use an arrow function the value of this is not rebound inside of the function, it's just inherited from whatever the parent scope is.
//you may or may not have tried using 'this' in a nested function or having to assign it to something else when reusing it later. for example setTimeout where you might have created an anonymous function that would not be bound to 
//anything and then by default be bound to the window. So to use 'this' would be misleading. 
//facing that type of issue, use an arrow function, because the parent scope would then be only one level above. 
box.addEventListener('click'), function() {
	console.log(this);
	setTimeout(() => {
		console.log(this);
	}, 500);
}}; 
```

#### WHEN NOT to use an arrow function?
- When you really need 'this'
- When you need a method to bind to an object (because it will look a level outside the object)
- When you need to add a prototype method (for classes)
- When you need arguments object, doesn't have to do with this -- I didn't understand this one, talks babout Array.from(arguments) where arguments is a keyword that arent found in arrow function scope. (Module #2 video #10)


You can also put an arrow functions in a variable - if you want to reuse it - it's possible. But could cause issues debugging while picking through the stack trace. 
```js
const sayHello = (name) => { alert(`Hello ${name}!`)}
sayHello('Alisa')
```


#### Default function arguments
When you pass in a bunch of args as expected but if they are undefined the function has to handle them
You can actually handle the function arguments in the function declaration and assign defaults in the function declaration.
```js
function calculateBill(total, tax = 0.13, tip = 0.15) {
	//no need for tax = tax || .13
}

const totalBill = calculateBill(100, undefined, 0.25);
```


 An arrow function is a lambda expression! Seen in CoffeeScript, Python, C#, VB... (ubiquitous in functional programming languages & languages that apply the functional programming paradigm) 

### What is a programming paradigm/the functional programming paradigm? 
a concept or way of programming, it's support is often determined by the execution model of the language. Languages support multiple paradigms (many to many). The different paradigms can be determined by the way the language manipulates state, its control structures, procedure calls, how it uses classes or prototypes.
Functional programming paradigm is based on lambda calculus which is a formal system for expressing computations. Concepts from this style are being supported in more languages. FP is declarative and uses expressions more than statements (that have nested expressions).

### What is a lambda?  
Put simply: an anonymous function that that can be passed around as an object to higher order functions! 
A higher order function is just a function that will take in another function as an argument, or return a function as output. 

### What is a closure?
A closure is a function that can maintain its own scope.
Put simply in an example: a nested function that can access the outer scope even after the outer function has returned.
Why are closures a thing? The counter dilemma 
- if you need a counter to be accessible by all functions, make a global variable!
- oh no! people are accessing my counter variable outside of the add() function, because it's global!
```js
function add() {
    var counter = 0;
    counter += 1;
}
```
- a nested function will solve this!
- see codepen for solution.
- makes it possible for a function to have "private" variables, any function where youre using a variable from outside the scope, is actually a closure.
