## 2. JavaScript

### 2.1 Variable definition

- **var**: old way of defining a variable

- **let**: "new var". use if you want to create a regular variable

- **const**: constant value

### 2.2 Arrow Functions

Normal way of creating a JS function

```js

//old way
function myFnc() {

}

//new way
const myFnc = () => {

}
```

It helps with the **this** keyword. Parenthesis are optional only if there is one argument.

if we just return one argument we can use it in "lambda" syntax

```js
const multiply = (number) => {
    return multiply * 2;
}

// equivalent to:

const multiply = number => number * 2;
```

### 2.3 Exports & Imports (Modules)

Export code over multiple files. Inside a JS file we can import content from another file.

```js
//person.js
const person = {
    name: 'Max'
}

export default person

//utility.js
export const clean = () => {}

export const baseData = 10;

//app.js
import person from './person.js'
import prs from './person.js'

import { baseData } from './utility.js'
import { clean } from './utility.js'
```

**Default export**: we export that specific variable. We don't need to mark the scope specifically in the further ```import``` and it doesn't matter how do we want to call it (person or prs). Always person object wil lbe exported. 

**Named export**: With the other example ```export const``` we need to define what do we need to import specifically ```import {foo)```. We can also use aliases ```import {baseData as bd}``` and import all variables as one as ```import * as foo```


### 2.4 Classes

Very similar to any OOP language

```js
//definition
class Person {
    constructor() {
        this.name = 'Max';
    }

    printMyName() {
        console.log(this.name);
    }

}

//usage
const myPerson = new Person();
myPerson.printMyName();
```

We can also use inheritance

```js
class Human {
    constructor() {
        this.gender = 'male';
    }

    printGender() {
        console.log(this.gender);
    }
}

class Person extends Human {
    constructor() {
        super(); //always required on inheritance
        this.name = 'Max';
        this.gender = 'female';
    }

    printMyName() {
        console.log(this.name);
    }

}

//usage
const person = new Person();
person.printMyName();
person.printGender(); //female
```

There is also a new syntax in ES7. We can skip construction call where we can add the property directly on the class and it will be used as constructor and the same with methods. Super doesn't need to be invoked either.

```js
class Human {
    gender = 'male';

    printGender = () => {
        console.log(this.gender);
    }
}

class Person extends Human {
    name = 'Max';
    gender = 'female';

    printMyName = () => {
        console.log(this.name);
    }

}

//usage
const person = new Person();
person.printMyName();
person.printGender(); //female
```

### 2.5 Spread & Rest Operators

Operator is 3 dots ```...```

- Spread mode: Used to split up array elements or object properties. We pick the old array/object and copy all their properties/functions into the new one, adding whatever we want appended.

```js
const numbers = [1, 2, 3];
const newNumbers = [...numbers, 4];

console.log(newNumbers); // [1, 2, 3, 4]. If we didn't add ..., result would be [[1, 2, 3], 4]

const person = {
    name: 'Max'
};

const newPerson = {
    ...person,
    age: 28
}

console.log(newPerson);
/*
[object Object] {
    age: 28,
    name: "Max"
}
*/
```

- **Rest mode**: Used to merge a list of function arguments into an array

```js
const filter = (...args) => {
    return args.filter(el => el === 1);
}

console.log(filter(1, 2, 3)); //1
```

### 2.6 Destructuring

Easily extract array elements or object properties and store them in variables. Spread extracts all properties or elements and keep them in the same object. Destructure extracts specific properties / functions and store as we want to.

```js
//Array destructuring
const numbers = [1, 2, 3];
[num1, num2] = numbers;
console.log(num1, num2); //1, 2
[num1, , num3] = numbers;
console.log(num1, num3); //1, 3

//Object destructuring
const person = {
    name: 'Max',
    age: 28
}

{name} = person;
console.log(name); //"Max"
console.log(age); //undefined
```

### 2.7 Reference & Primitive types

Numbers, strings, booleans... are primitive types. If we reassign or store a variable in another variable they will store the value.

Objects & arrays are reference types. If we update them, their references are updated as well once we reassign them. If we don't want this effect, we should make use of spread operator

```js
const person = {
    name: 'Max'
};

const secondPerson = person;
const thirdPerson = {
    ...person
}

person.name = 'Manu';
console.log(secondPerson.name); //Manu
console.log(thirdPerson.name); //Max
```

### 2.8 Array functions

Although each one has their own goal, all those methods applied to an array take a function and execute on each element in an array. **map** maps the resulting elements into a new array. There are more (sort, filter, etc).

```js
const numbers = [1, 2, 3];

const doubleNumArray = numbers.map((num) => {
  return num * 2;
});
console.log(numbers); // [1, 2, 3]
console.log(doubleNumArray); // [2, 4, 6]
```