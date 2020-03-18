```JS - INTERVIEW PREPARATION```

Those are basic concepts that every developer should know. I've got this material from [dev-bits](https://medium.com/dev-bits/a-perfect-guide-for-cracking-a-javascript-interview-a-developers-perspective-23a5c0fa4d0d)


```BIND```

Use `.bind()` when you want that function to later be called with a certain context, useful in events.

```CALL OR APPLY```

Use `.call()` or `.apply()` when you want to invoke the function immediately, with modification of the context.

example:

Lets assume that we have a function which find the area and circumference of the circle

```
var mathLib = {
    pi: 3.14,
    area: function(r) {
        return this.pi * r * r;
    },
    circumference: function(r) {
        return 2 * this.pi * r;
    }
};
mathLib.area(2);
/// 12.56

```

But for some reason, you now need to instead of using pi as `3.14`, you now need to use pi with `5 decimals` precision. How could you do it without changing the code? By using `.call()` you can change the pi value. See example:

```
mathLib.area.call({pi: 3.14159}, 2);
/// 12.56636
```

```What just happened?```

the `.call()` method received two arguments `.call(context, fnParams)`, where context is the object that replaces `this` keyword inside the function context, and the arguments are passed to the function arguments.

```
var cylinder = {
    pi: 3.14,
    volume: function(r, h) {
        return this.pi * r * r * h;
    }
};
```

The `.apply()` method is the same as `.call()` but `.apply()` expect an array as second argument;
```
/// .call()

cylinder.volume.call({pi: 3.14159}, 2, 6);
/// 75.39815999999999

/// .apply()

cylinder.volume.apply({pi: 3.14159}, [2, 6]);
/// 75.39815999999999
```

```BIND```

The `.bind()` method, attaches a brand new `this` to a given function. In bind's case, the function is not executed instantly like `.call()` or `.apply()`.

```
var newVolume = cylinder.volume.bind({pi: 3.14159}); // This is not instant call

// After some long time, somewhere in the wild

newVolume(2,6);

// Now pi is 3.14159
// note that is a new function and cylinder remains the same as before
```


```JS SCOPES```

JavaScript has 2 scopes, `Global`, `Local`. Ok, but what are those things?

Scope is referred to the current context of the code, and determines how acessible a variable is. How it works?

```Global scope```

See the example below:

```
x = 10; // due to the variable creation happend outside a function context, it was created globally, so any piece of the code has access to it

function foo() {
  console.log(x);
}

foo();
/// 10

function bar() {
  console.log(x);
}

bar();
/// 10
```


```Local Scope```

```
pi = 3.14; // pi is defined globally

function circumference(radius) {
     pi = 3.14159; // but here is overwritten due to the local pi variable
     console.log(2 * pi * radius);
}
circumference(2);
/// Prints "12.56636" not "12.56"
```

And with the introduction of ES6, there is a `Block Scope` which limits a variable's scope to a given parenthesis block.

```
var a = 10;

function Foo() {
  if (true) {
    let a = 4; // the scope referred to the `let` a, remains inside the parenthesis, outside is unexistent
  }

  alert(a); // alerts '10' because the 'let' keyword
}
Foo();
```

After a function run, its scope is destroyed and we don't have access to it anymore. If you need to persist to have access, you will need a `closure`.

```CLOSURE```

What is a closure? Closure is a function which returns another function, but also keeps the parent scope alive, even after it runs, you still can have access to its scope.

```
function counter(num) {
  var count = num; // count is defined in the parent scope
  var addCount = function () {
    count++; // child scope increments count
    console.log(count)
  }
  return addCount;
}

var fnCounter = counter(1); // outter call to a function local scope
```

```THIS```

We could assume that `this` plays different roles on JavaScript. Why? Because depends on its context, `this` will have a diffenrent value. For example:

If you open console and type

```
this === window

/// true
```

It will return `true` because `this` by this execution time is related to `window` which is the browser's global context.

Although if we use `this` inside a function, now this reference is the function local scope:

```

function greeting() {
  console.log(`${this.name}, welcome`); // this here is related to function's context
}

let person = {name: 'John'};

greeting.call(person); // check .call() explanation above

```

And what about this one?

```
function greeting() {
  console.log(this)
}

/// ????

```

What do you think it will be logged? If you said `window`, you're right. Why? Because whatever the parent is, it will be always inherited by the child, and in this example, function's parent scope is `window`.

And for the last type of `this` comes `Object`.

```
let animal = {
  breed: 'German Sheperd'
  pedigree: true,
  get specie() {
    return {breed: this.breed, pedigree: this.pedigree}
  }
}

console.log(animal.specie);

/// { breed: 'German Sheperd, pedigree: true }
```


```OBJECTS```

What a object is? Object it is a map that stores pairs of key (a.k.a property) and value. Although JavaScript object has a special power. It can store anything as value, that means, you can store a list, a function or even another object.

We can create a new object by:

```
let obj = {};

// or

let obj2 = new Object();
```

To get a list of keys you just need to:

```
let obj = {
  property: 'value',
  property2: 'value 2,
  property3: 'value 3'
}

console.log(obj.keys());

/// ['property','property2', 'property3']
```

But if you need to get just the values, you can use:

```
...
console.log(obj.values());

/// ['value','value 2', 'value 3']
```

Objects `getter` and `setter` are accessors that allows you to get and given object property value. It was used in a previous example.

```getter```

It allows you to get an exposed property value, like example bellow:

```
let cat = {
  name: 'Kitty',
  gender: 'female',
  age: 4,
  get information() {
    return { name: this.name, gender: this.gender }
  }
}

console.log(cat.information)
// { name: 'Kitty', gender: 'female'}
// Since we just defined name and gender as exposed properties
// just those 2 properties are returned from the getter
```

```setter```

The `setter` in the opposite way of the getter, allows you to set the value of a give property.


```
let dog = {
  name: 'Bobby',
  breed: ''
  set kind(param) {
    this.breed = param;
  }
}

// set breed by:

dog.kind = 'Pit Bull';
console.log(dog);

/**
{
  name: 'Bobby',
  breed: 'Pit Bull'
}
**/
```

```prototype```

Prototype is where object inherits from. By that I mean, `object.keys()` is inherited from `prototype`, so we could say that `object.__proto__.keys()` is a valid way to access the `key` function, although is not necessary. But prototype gives you the power to create new function and make it available on object scope, for example:

```
let person = {
  name: 'John',
  age: 30,
  nationality: 'American'
}

person.__proto__.who = function () {
  return `Name is ${this.name}, by the age of: ${this.age}, an ${this.nationality} citizen`
}

person.who()
/// "Name is John, by the age of: 30, an American citizen"
```

To check all the functions available for objects, please check out this link: [Object Methods - JS](https://developer.mozilla.org/pt-PT/docs/Web/JavaScript/Reference/Global_Objects/Object)


```Arrays```

What arrays are? Arrays are lists, for each item in the list you have one single value, this value can be almost everything