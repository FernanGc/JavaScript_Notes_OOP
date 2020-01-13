# The Principles of  JavaScript Object Oriented Javascript | Notes

### Primitive and Reference Types

**Five primitive types**

* strings
* numbers
* booleans
* null
* undefined

Represent simple values stored directly in the variabl object for a given context. *Use `typeof` to identify primitive types with the exception of null*

**Reference types** are the closest thing to classes in JavaScript, and objects are instances of reference types, you can create new objects using the `new` operator or a reference literal.

You can access properties and methods primarily using dot notation, but you can also use bracket notation.

Functions are objects in JavaScript, and you can identify them with the `typeof` operator. You should use `intanceof` with a constructor to identify objects of any other reference type.

To make primitives seem more like references, JavaScript has three primitive wrapper types:

* String
* Number
* Boolean

Javascript creates these objects behind the scenes so that you can treat primitives like regular objects, but the temporary  objects are destroyed as soon as the statement using them is complete.



> A constructor is simply a function that uses `new` to create an object--any function can be a constructor. By convention, constructors in JavaScript begin with a capital letter to distinguish them from nonconstructor functions. 



**Adding or Removing Properties**

````javascript
/*
 * Here myCustomProperty is added to object1 with the value of "Awesome!"
 * That property is also accessible on object2 because both object1 and object 2 point
 * to the same object.
*/
const object1 = new Object();
const object2 = new object1;

object1.myCustomProperty = "Awsome!";
console.log(object2.myCustomProperty);
````

**Property Access**

> Properties are name/value pairs that are stored on an object. Dot notation is the most common wat to access properties in JavaScript. but you can also access properties on JavaScript objects by using bracket notation with a string. The point to remember is that, other than syntax, the only difference  between dot notation and bracket notation is that bracket notation allows you to use special characters in property names.

````javascript
// Dot notation
const array = [];
array.push(12345);

// Bracket notation
const array = [];
array["push"](12345);
````

### Functions

> What distinguishes a function from  any other object is the presence of an *internal property* named [[Call]]. Internal properties are not accessible via code but rather define the behavior of code as it executes.

**Declarations vs Expressions**

> Two literal forms of functions. *function declaration* which begins with the function keyword and includes the name of the function immediately following it.

````javascript
function add(num1, num2) {
  return num1 + num2;
}
````

> The second form is a *functions expression* which doesn't require a name after `function` these functions are considere anonumous because the function object itself has no name. Instead, function expressions are typically referenced via a variable or property, as in this expression.

````javascript
var add = function(num1, num2) {
  return num1 + num2;
}
````

> Note function declarations are `hoisted` to the top of the context (either the function in which the declaration occurs or the global scope). That means you can actually define a function after it is used in code without generating an error.

> Function hoisting happens only for function declarations because the function name is know ahead of time. Function expressions, on the other hand, cannot be hoisted  because the functions can be referenced only through  a variable.

**Object Methods**

You can add a method to an object in the same way that you would add a property. For example, in the following code, the `person` variable is assigned an object literal with a name property and a method called `satName`.

`````javascript
var person = {
  name: "Nicholas",
  sayName: function() {
    console.log(person.name);
  }
};

person.sayName(); // outputs "Nicholas"
`````

**The this Object**

> Every scope in Javascript has a this object that represents the calling object for the fuction. In the global scope, `this` represent the global object  for the function. In the global scope, `this` represents the global object (window in web browsers). When a function is called while attached to an object, the value of `this` is equal to that object by default. So, instead of directly referencing an object inside a method, you can reference `this `instead.

````javascript
function sayNameForAll() {
  console.log(this.name);
}

var person1 = {
  name: "Nicholas",
  sayName: sayNameForAll
}

var person1 = {
  name: "Greg",
  sayName: sayNameForAll
}

var name = "Michael"

person1.sayName(); // outputs "Nicholas"
person2.sayName(); // outputs "Greg"

sayNameForAll(); // outputs "Michael"
````

**The bind() Method**

> The first argument to `bind()` is the `this` value for the new function. All other arguments represent named paramenters that should be permanently set later.

````javascript
function sayNameForAll(label) {
  console.log(label + ":" + this.name);
}

var person1 = {
  name: "Nicholas"
};

var person2 = {
  name: "Greg"
};

// create a function just for person1
var sayNameForPerson1 = sayNameForAll.bind(person1);
sayNameForPerson1("person1"); // outputs "person1:Nicholas"

// create a function just for person2
var sayNameForPerson2 = sayNameForAll.bind(person2, "person2");
sayNameForPerson2();

// attaching a method to an object dosen't change this
person2.sayName = sayNameForPerson1;
person2.sayName("person2");	// outputs "person2:Nicholas"
````

# Understanding Objects





