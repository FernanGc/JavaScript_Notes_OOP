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

> Objects in Javascript are dynamic, meaning that they can change at any point during code execution. there are two basic ways to create your own objects: using the Object constructor and using an object literal.

**Defining Properties**

````javascript
var person1 = {
  name: "Nicholas"
};

var person2 = new Object();
person2.name = "Nicholas";

person1.age = "Redacted";
person2.age = "Redacted";

person1.name = "Greg";
person2.name = "Michael";
````

**Detecting Properties**

````javascript
console.log("name" in person1);
console.log("age" in person1);
console.log("title" in person1);
````

> Keep. in mind that methods are just properties that reference function, so you can check for the existence of a method in the same way.

````javascript
var person1 = {
  name: "Nicholas",
  sayName: function() {
    console.log(this.name);
  }
};

console.log("sayName" in person1); // true
````

> In some cases, however, you might want to check for the existence of a property only if it is an own property. The in operator checks for both own properties and prototype properties, so you'll need to take a different approach. Enter the hasOwnProperty() method, wich is present on all objects and returns true only if the given property exist and is an own property.

````javascript
// The following code compares the results of using in versus hasOwnProperty().

var person1 = {
  name: "Nicholas",
  sayName: function() {
    console.log(this.name);
  }
};

console.log("name" in person1); // true
console.log(person1.hasOwnProperty("name")); // true

console.log("toString" in person1);	// true
console.log(person1.hasOwnProperty("toString")); // false		0-
````

**Removing Properties**

> You need to use the `delete` operator to completely remove property and calls an internal operation name [[Delete]].

````javascript
var person1 = {
  name: "Nicholas"
}

console.log("name" in person1); // true
delete person1.name;

console.log("name", in person1); // false
console.log(person1.name); // undefined
````

> Whe you *delete* the name property, it completely disappears from person1.

**Enumeration**

> Typically, you would use `Object.keys()` in situations where you want to operate on an array of property name and `for-in` when you don't need an array. You can check whether a property is enumerable by using the `propertyIsEnumerable()` method, which is present on every object:

**for-in**

````javascript
var property;

for (property in object) {
  console.log("Name: " + property);
  console.log("Value: " + object[property]);
}
````



**Object.keys**

````javascript
var properties = Object.keys(object);

// if you want to mimic for-in behavior
var i, len;

for (i = 0, len = properties.length; i < len; i++){
  console.log("Name: " + properties[i]);
  console.log("Value: " + object[properties[i]]);
}

````

**Types of Properties**

> There are two different types of properties: data properties and accessor properties. *Data properties* contain a value, like the name property from earlier examples in this chapter. *Accessor properties* don't contain a value but instead define a function to call when the property is read (called a getter), and a function to call when the property is written to (called a setter). Accessor properties only require either a getter or a setter, though they can have both.

````javascript
// There is a special syntax to define an accesor property using an object literal.

var person1 = {
  _name: "Nicholas",
  
  
  get name() {
    console.log("Reading name");
    return this._name;
  },
  
  set name(value) {
    console.log("Setting name to %s", value);
    this._name = value;
  }
};

console.log(person1.name); // "Reading name" then "Nicholas"

person.name = "Greg";
console.log(person1.name); // "Setting name to Greg" then "Greg"
````

> Getter are expected to return a value, while setter receive the value being assigned to the property as an argument. *You don't  need to define both a getter and a setter; you can choose one or both.*

> *If you define only a getter, then the property becomes read-only, and attempts to write to it will fail silently in nonstrict mode and throw an error in strict mode.*
>
> *If you define only a setter, then the property becomes write-only, and attempts to read the value will fail silently in both stric and nonstrict mode.*

## Property Attributes

**Common Attributes**

By default, all properties you declare on an object are both enumerable and configurable.

 If you want to change property attributes, you can use the  `Object.defineProperty()` method. This method accepts three arguments: the object that owns the property, the property name, and a property descriptor object containing the attibutes to set. So you use `enumerable` to set `[[Enumerable]]` and configurable to set [[Configurable]]

> You can't make a nonconfigurable property configurable again. When JavaScript is running in strict mode, attempting to delete a non configurable property results in an error. In constrict mode, the operation silently fails.

````javascript
var person1 = {
    name: "Nicholas"
};
  
Object.defineProperty(person1, "name", {
    enumerable: false
});
  
console.log("name" in person1); // true
console.log(person1.propertyIsEnumerable("name")); // false

var properties = Object.keys(person1);
console.log(properties.length); // 0

Object.defineProperty(person1, "name", {
    configurable: false
});

// try to delete the Property
delete person1.name;

console.log("name" in person1); // true
console.log(person1.name); // "Nicholas"

Object.defineProperty(person1, "name", { // error!!
    configurable: true
});
````

**Data property Attributes**

Data properties possess two additional attributes that accessors do not. The first is [[Value]], which holds the property value. The second attribute is [[Writable]], which is a Boolean value indicating whether  the property can be written to.

```javascript
var person = {};

Object.defineProperty(person1, "name", {
    value: "Nicholas",
    enumerable: true,
    configurable: true,
    writable: true
});
```

**Accessor Property Attributes**

Accessor properties also have two additional attributes. Because there is not value stored for accessor properties, there is no need for [[value]] or [[Writable]]. Instead, accessors have [[Get]] and [[Set]], which contain the getter and setter functions, respectively. As with the object literal from of getters and setters, you need only define one of these attributes to create the property.

> If you try to create a property with both data and accessor attributes, you will get an error.

````JavaScript
var person1 = {
    _name: "Nicholas",

    get name() {
        console.log("Reading name");
        return this._name;
    },

    set name(value) {
        console.log("Setting name to ", value); 
        this._name = value;
    }
};
````

This code can also be written as follows

````JavaScript
var person1 = {
    _name: "Nicholas"
}

Object.defineProperty(person1, "name", {
    get: function() {
        console.log("Reading name");
        return this._name;
    },
    set: function(value) {
        console.log("Setting name to", value);
        this._name = value;
    },
    enumerable: true,
    configurable: true
});
````

**Defining Multiple Properties**

It's also prossible to define multiple properties on an object simultaneously if you use `Object.defineProperties()` instead of `Object.defineProperty()`. This method accepts two arguments: the object to work on, and an object containing all of the property information.

````JavaScript
var person1 = {};

Object.defineProperties(person1, {
    // data property to store data
    _name: {
        value: "Nicholas",
        enumerable: true,
        configurable: true,
        writable: true
    },

    // accessor property
    name: {
        get: function() {
            console.log("Reading name ");
            return this._name;
        },

        set: function(value) {
            console.log("Setting name to", value);
            this._name = value;
        },
        enumerable: true,
        configurable: true
    }
});
````

**Retrieving Property Attributes**

If you need to fetch property attributes, you can do so in JavaScript by using `Object.getOwnPropertyDescriptor()`. This method accepts two arguments: the object to work on and the property name to retrieve.

````javascript
var person1 = {
  name: "Nicholas"  
};


var descriptor = Object.getOwnPropertyDescriptor(person1, "name");

console.log(descriptor.enumerable);
console.log(descriptor.configurable);
console.log(descriptor.writable);
console.log(descriptor.value);
````

Here, a property called `name` is defined as part of an object literal. The call to `Object.getOwnPropertyDescriptor()` returns an object with `enumerable` `configurable`, `writable` and `value` though these weren't explicitly defined via `Object.defineProperty()`.

## Preventing Object Modification

Objects, just like properties, have internal attributes that govern their behavior. One of these attributes is [[Extensible]], which is a Boolean value indicating if the object itself can be modified.

> All objects you create are *extensible* by default

**Preventing Extensions**

One way to create a nonextensible object is with `Object.preventExtensions()` This method accepts a single argument, which is the object you want to make nonextensible. You can check the value of [[Extensible]] by using `Object.isExtensible()`. The following code shows examples of both methods at work.

```javascript
var person1 = {
    name: "Nicholas"
};

console.log(Object.isExtensible(person1)); // true

Object.preventExtensions(person1);
console.log(Object.isExtensible(person1)); // false

person1.sayName = function() {
    console.log(this.name);
};

console.log("sayName" in person1); // false
```

**Sealing Objects**

The second way to create a nonextensible object is to `seal` the object. A sealed object is nonextensible. You can use the `Object.seal()` method on an object to seal it. When that happened, the [[Extensible]] attribute is set to false, and all properties have their [[Configurable]] attribute set to false. You can check to see whether an object is sealed using `Object.isSealed()` .

````javascript
var person1 = {
    name: "Nicholas"
};

console.log(Object.isExtensible(person1)); // true
console.log(Object.isSealed(person1)); // false

Object.seal(person1);
console.log(Object.isExtensible(person1)); // false
console.log(Object.isSealed(person1)); // true


person1.sayName = function() {
    console.log(this.name); 
};

console.log("sayName" in person1); // false

person1.name = "Greg";
console.log(person1.name); // Greg

delete person1.name;
console.log("name" in person1); // true
console.log(person1.name); // Greg


var descriptor = Object.getOwnPropertyDescriptor(person1, "name");
console.log(descriptor.configurable); // false
````

**Freezing Objects**

A frozen object is a sealed object where data properties are also read-only. Fronzen objects can't become unfrozen, so they reamain in the state they were in when the became frozen. You can freeze an object by using `Object freeze()` and determine if an object is frozen by using `Object.isFrozen()`

```javascript
var person1 = {
    name: "Nicholas"
};

console.log(Object.isExtensible(person1));  // true
console.log(Object.isSealed(person1));  // false
console.log(Object.isFrozen(person1)); // false

Object.freeze(person1);
console.log(Object.isExtensible(person1));
console.log(Object.isSealed(person1));
console.log(Object.isFrozen);

person1.sayname = function() {
    console.log(this.name);
}

console.log("sayName" in person1);  // false

person1.name = "Greg";
console.log(person1.name);  // "Nicholas

delete person1.name;
console.log("name" in person1); // true
console.log(person1.name);  // "Nicholas"

var descriptor = Object.getOwnPropertyDescriptor(person1, "name");
console.log(descriptor.configurable); // false
console.log(descriptor.writable); // false
```

> Frozen object are simply snapshots of an object at a particular point in time. They are of limited use and should be used rarely, As with all nonextensible objects, you should use strict  mode with frozen object.

## Constructor and Prototypes

Constructors are just normal functions that are called with the `new` operator. You can define your own constructors anytime you want to create multiple objects with the same properties. You can identify objects created from constructors using `instanceof` or by accessing their `constructor` property directly.

Every function has a `prototype` property that defines any properties shared by objects created with a particular constructor. Shared methods and primitive value properties are typically defined on prototypes, while all other properties are defined within the constructor. The `constructor` property is actually defined on the prototype because it is shared among object instances.

The prototype of an object is stored internally in the `[[Prototype]]`property. This property is a reference, not a copy. If you change the prototype at any point in the time, those changes will occur on all instances because of the way JavaScript looks up properties. When you try to access a property on an object, that object is searched for any own property is searched. This searching mechanism means the prototype  can continue to change, and object instances referencing that prototype will reflect those changes immediately.

Buit-in objects also have prototypes that can be modified. While it's not recommended to do this in production, it can be helpful for experimentation and proofs of concept for new functionality.

## Inheritance

**Methods Inherited from Object.prototype.**

| Method                   | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| `hasOwnProperty()`       | Determines whether an own property with the given name exist |
| `propertyIsEnumerable()` | Determines whether an own property is enumerable             |
| `isPrototypeOf()`        | Determines whether the object is the prototype of another    |
| `valueOf()`              | Returns the value representation of the object               |
| `toString()`             | Returns a string representation of the object                |

> These five methods appear on all objects through inheritance. The last two are important when you. Need to make objects work consistently in JavaScript, and sometimes you might wnat to define them yourself.

JavaScript supports inheritance though prototype chaining. A prototype chain is create between objects when the `[[Prototype]]` of one object is set equal to another. All generic objects automatically inherit from Object `.prototype`. If you want to create an object that inherits from something else, you can use `Object.create` to specify the value of [[Prototype]] for a new object.

You accomplish inheritance between custom type by creating a prototype chain on the constructor. By setting the constructor's prototype property to another value, you create inheritance between instances of that constructor share the same prototype, so they all inherit from the same object. This technique works very well for inheriting methods from other objects, by you cannot inherit own properties using prototypes.

To inherit own properties correctly, you can use constructor stealing, which is ismply calling a constructor function using `call()` or `apply()`. So that any initialization is done on the subtype object. Combining constructor stealing and prototype chaining is the most common way to achieve inheritance between custom types in JavaScript. This combination is frequently called pseudoclassical inheritance because of tis similarity to inheritance in class-based languages.

You can access method on a supertype by directly accessing the supertype's prototype. In doing so, you must use `call()` or `apply()` to execute the super type method on the subtype object.

## Object Patterns

**The module Pattern**

The *module pattern* is an object-creation pattern designed to create single-ton objects with private data. The basic approach is to use an *immediately invoked function expression (IIFE)* that returns an object. Methods that access private data in this way are called *privileged methods* Here's the basic format for the module patter:

````javascript
var yourObject = (function() {
  // private data variables
  
  return {
    // public methods and properties
  };
}());
````

> Closures are simply functions that access data outside their own scope. For example, whenever you access a global object in a function, such as window in a web browser, that function is accessing a variable outside its own scope.

````javascript
// This code creates the person object using the module pattern.

var person = (function() {
    var age = 25;
    
    return {
      name: "Nicholas",
      
      getAge: function() {
        return age;
      },
      
      growOlder: function() {
        age++;
      }
    };
  }());
  
  console.log(person.name); // "Nicholas"
  console.log(person.getAge()); // 25
  
  person.age = 100;
  console.log(person.getAge()); // 25
  
  person.growOlder();
  console.log(person.getAge()); // 26
````

There is a variation of the module pattern called the `revealing module pattern`, which arranges all variables an methods at the top of the IIFE and simly assigns them to the returned object. You can write the previous example using the revealing module pattern as follows:

````javascript
var person = (function() {
  var age = 25;
  function getAge() {
    return age;
  }
  
  function growOlder() {
    age++;
  }
  
  return {
    name: "Nicholas",
    getAge: getAge,
    growOlder: growOlder
  }
}());
````

**Private Members for Constructors**

````javascript
var Person = (function() {
    // everyone shares the same age
    var age = 25;
    
    function InnerPerson(name) {
      this.name = name;
    }
    
    InnerPerson.prototype.getAge = function() {
      return age;
    };
    
    InnerPerson.prototype.growOlder = function () {
      age++;
    };
    
    return InnerPerson;
  }());

  var person1 = new Person("Nicholas");
  var person2 = new Person("Greg");

  console.log(person1.name); // "Nicholas"
  console.log(person2.getAge()); // 25

  console.log(person2.name); // "Greg" 
  console.log(person2.getAge()); // 25

  person1.growOlder();
  console.log(person1.getAge()); // 26
  console.log(person2.getAge()); // 26
````

## Mixins

> *Mixins* occur when one object acquires the properties of another without modifying the prototype chain. The first object (a *receiver*) actually receives the properties of the second object (the supplier) by copying those properties directly. Traditionally, you create mixins using a function such as this:

````javascript
function mixin(receiver, supplier) {
  for (var property in supplier) {
    if (supplier.hasOwnProperty(property)) {
        receiver[property] = supplier[property]
    }
  }
  
  return receiver;
}


var person = mixin(new EventTarget(), {
  get name() {
    return "Nicholas"
  },

  sayName: function() {
    console.log(this.name);
    this.fire({ type: "namesid", name: name});
  }
});

console.log(person.name); // "Nicholas"

person.name = "Greg";
console.log(person.name); // "Greg"
````

For example, you can add event support to an object through a mixin rather than inheritance. First, suppose you've already defined a custom type for using events:

````javascript
function EventTarget() {
  
}

EventTarget.prototype = {
  constructor: EventTarget,

  addListener: function(type, listener) {
    // create an array if it doesn't exist
    if (!this.hasOwnProperty("_listeners")){
      this._listeners = [];
    }

    if (typeof this._listeners[type] == "undefined") {
      this._listeners[type] = [];
    }

    this._listeners[type].push(listener);
  },

  fire: function(event) {
    
    if (!event.target) {
      event.target = this;
    }

    if (!event.type) { // falsy
      throw new Error("Event object missing 'type' property");
    }

    if (this._listeners && this._listeners[event.type] instanceof Array) {
      var listeners = this._listeners[event.type];
      for (var i = 0, len = listeners.length; i < len; i++) {
        listeners[i].call(this, event);
      }
    }
  },

  removeListener: function(type, listener) {
    if (this._listeners && this._listeners[type] instanceof Array) {
      var listener = this._listeners[type];
      for (var i = 0, len = listeners.length; i < len; i++) {
        if (listeners[i] === listener) {
          listeners.splice(i, 1);
          break;
        }
      }
    }
  }
};

var target = new EventTarget();

target.addListener("message", function(event) {
  console.log("Message is " + event.data);
});

target.fire({
  type: "message",
  data: "Hello world"
});
````

if you want accessor properties to be copied over as accessor properties, you need a different mixin() function, such as:

````javascript
function mixin(receiver, supplier) {
  Object.keys(supplier).forEach(function(property){
    var descriptor = Object.getOwnPropertyDescriptor(supplier, property);
    Object.defineProperty(receiver, property, descriptor);
  });
  return receiver;
}

var person = mixin(new EventTarget(), {
  get name() {
    return "Nicholas"
  },

  sayName: function() {
    console.log(this.name);
    this.fire({type: "namesaid", name: name});
  }
});

console.log(person.name); // "Nicholas"
person.name = "Greg";
console.log(person.name); // "Nicholas"
````

## Scope-Sage Constructors

Many built-in constructors, such as Array and RegExp, also work without new because they are written  to be *scope safe* A scope safe constructor can be called with or without `new`

