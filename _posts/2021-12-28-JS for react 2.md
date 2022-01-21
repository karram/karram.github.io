# JS for React 
## Variable declaration
### const
Identifiers whose value does not change.

    const x = true
    x = false // Exception
    

### let
lexical scoping for variables. Previously, variables declared inside {} or a for loop would be still globals. Variables declared with let have a limited scope.

    var y = "Hello"
    if (y) {
       let y = "world"
       console.log("block", y) // prints block world
    }
    console.log("global", y) // prints global Hello
    
## String templates

Provides an alternative to string concatenation. Supports multi-line template strings and whitespace inside the template. Useful for email html templates, for example.

For example

    console.log(arg1 + " " + arg2)

can be replaced with

    console.log(`${arg1} ${arg2}`)

## Functions

### Function declaration
starts with the keyword function, followed by function name and args, and body. Default params are supported.

    function myfn(arg1, arg2 = "World") {
        console.log(`${arg1}, ${arg2}!`)
    }

    myfn("Hello", "World") // prints Hello, World!
    myfn("Hello") // also prints Hello, World!

### Function expression
Another way is to use a function expression that creates the function as a variable

    const myfn2 = function(arg1, arg2) {
        console.log(`${arg1}, ${arg2}!`)
    }

    myfn2("Hello", "world")

### Declaration vs Expression
Function created using a function declaration can be used before the declaration itself. This wont work with function expressions. JS automatically moves (hoists) all function declarations to the top of the script.

### Arrow Functions
Create a function without using the function keyword or a return statement. Compact representation

    const myfn3 = (arg1, arg2 = "World") => `${arg1}, ${arg2}!`

    myfn3("Hello") // prints Hello, World!

### Returning objects
Enclose the object in parentheses

    const person = (firstname, lastname) => ({
        first: firstname,
        last: lastname
        })

    console.log(person("Luke", "Skywalkerrr"))

### Arrow functions and scope
Arrow functions can be used to limit the scope of "this". Without the arrow function used in setTimeout, this would be bound to something else.

    const tahoe = {
        mountains: ["Freel", "Rose", "Tallac", "Rubicon", "Silver"],
        print: function(delay = 1000) {
        setTimeout(() => {
                console.log(this.mountains.join(", "));
            }, delay);
        }
    };

    tahoe.print(); // Freel, Rose, Tallac, Rubicon, Silver

## Compiling JS
JS code can be compiled using Babel. The purpose of compilation is to convert modern code into a version of JS code supported widely by all browsers. For example, a new feature may not be supported by all browsers. You could write code using the new feature, and use Babel to generate JS code with the same functionality but compatible with even older browsers.

Babel is not the only such tool. It is one of the more popular tools. 

[Babel REPL](https://babeljs.io/repl "Babel Repl")

## Objects and Arrays

### Destructuring Objects

Pulling desired fields out of objects into local variables.

    const sandwich = {
        bread: "dutch crunch",
        meat: "tuna",
        cheese: "swiss",
        toppings: ["lettuce", "tomato", "mustard"]
    };
    const { bread, meat } = sandwich;
    console.log(bread, meat); // dutch crunch tuna

Can also be used to destructure function arguments. In the example below, the firstname field is pulled out of the object passed in as arg

    const lordify = ({ firstname }) => {
        console.log(`${firstname} of Canterbury`);
    };
    const regularPerson = {
        firstname: "Bill",
        lastname: "Wilson"
    };
    lordify(regularPerson); // Bill of Canterbury

This can be extended to complex objects

    const regularPerson = {
        firstname: "Bill",
        lastname: "Wilson",
        spouse: {
            firstname: "Phil",
            lastname: "Wilson"
        }
    };

    const lordify = ({ spouse: { firstname } }) => {
        console.log(`${firstname} of Canterbury`);
    };
    
    lordify(regularPerson); // Phil of Canterbury

### Destructuring Arrays
Used to extract desired values from an array. Skip over unused values using a comma.

    const [firstAnimal] = ["Horse", "Mouse", "Cat"]; // fetches the first value from the array. In this case, Horse
    const [, , thirdAnimal] = ["Horse", "Mouse", "Cat"]; // skips over the first two vals, and assigns Cat to thirdAnimal

### Object literal enhancement
Creates an object from smaller pieces or parts. Can also be used to create object methods.

    const name = "Tallac";
    const elevation = 9738;
    const print = function() {
        console.log(`Mt. ${this.name} is ${this.elevation} feet tall`);
    };

    const funHike = { name, elevation, print };
    funHike.print()

### Spread operator (...)

#### Combine arrays
    
    var a = ["1", "2", "3"]
    var b = ["4", "5", "6"]
    var comb = [...a, ...b]
    console.log(comb) // prints ["1", "2", "3", "4", "5", "6"]

#### Get remaining items in an array
    const lakes = ["Donner", "Marlette", "Fallen Leaf", "Cascade"];
    const [first, ...others] = lakes;
    console.log(others.join(", ")); // Marlette, Fallen Leaf, Cascade

#### Collect arguments to a function
    function directions(...args) { // args contains all the parameters passed to the function as a single array

#### Also works on objects
    const morning = {
        breakfast: "oatmeal",
        lunch: "peanut butter and jelly"
    };
    const dinner = "mac and cheese";
    const backpackingMeals = {
        ...morning,
        dinner
    };

    console.log(backpackingMeals);

## Asynchronous JS

### Promise
An object that indicates whether an async operation is pending, completed, or failed. For example, fetch returns a promise

    console.log(fetch("https://api.randomuser.me/?nat=US&results=1"));

### Acting on a promise - then() and catch()
.then() takes a callback function that acts on a promise. For example
    
    fetch("https://api.randomuser.me/?nat=US&results=1")
    .then(res => res.json())
    .then(json => json.results)
    .then(console.log)
    .catch(console.error);

Output of each then becomes the input to the next then (technically the next cbk fn in then(...))

### Acting on a promise - async and await

Declare a function as async() to indicate it needs to wait for an async operation to finish. Use await to signal which async operation to wait for.
The code here looks sequential, like a normal function, compared to the chain of .then().

    const getFakePerson = async () => {
        try {
            let res = await fetch("https://api.randomuser.me/?nat=US&results=1");
            let { results } = res.json();
            console.log(results);
        } catch (error) {
            console.error(error);
        }
    };

    getFakePerson();


### Building a promise
    const getPeople = count =>
    new Promise((resolves, rejects) => {
        const api = `https://api.randomuser.me/?nat=US&results=${count}`;
        const request = new XMLHttpRequest();
        request.open("GET", api);
        request.onload = () =>
        request.status === 200
            ? resolves(JSON.parse(request.response).results)
            : reject(Error(request.statusText));
        request.onerror = err => rejects(err);
        request.send();
    });

## Classes
JS used to support only prototypical inheritance. Now, the syntax has improved to look like other languages (think C++ or C#) but behind the scenes it still uses prototypes.

Example with inheritance

    class Expedition extends Vacation {
        constructor(destination, length, gear) {
            super(destination, length);
            this.gear = gear;
        }
        print() {
            super.print();
            console.log(`Bring your ${this.gear.join(" and your ")}`);
        }
    }

## Modules
A module is a piece of reusable code that can be incorporated into other JS code without variable name collisions. Modules export functions or variables. 

### Exporting

- Use export to export anything from a module
- Use export default if the module exports ONE and ONLY ONE thing

Examples

    export const print=(message) => log(message, new Date())
    export const log=(message, timestamp) =>
    console.log(`${timestamp.toString()}: ${message}`)

    export default new Expedition("Mt. Freel", 2, ["water", "snack"]);

### Importing
Use the import statement to import stuff from a module. Destructing works here, and like python import statements, imported stuff can be renamed during the import

Anything export defaulted can only be imported into a single variable

    import { print, log } from "./text-helpers";
    import freel from "./mt-freel";

