# JavaScript Notes


* [Basics](#basics)
* [Numbers](#numbers)
* [Functions](#functions)
* [OOP](#oop)
* [DOM (Document Object Model)](#dom-document-object-model)
* [Handling events](#handling-events)
* [Pitfalls](#pitfalls)
* [JSON](#json)
* [Asynchronicity](#asynchronicity)
* [References](#references)

## Basics

1. Variables:
    * Global variables live as long as the page.
    * If we forget to declare a variable before using it, it’ll always be global even if we first use it in a function.
    * `let` creates block-level variables.
    * `const` creates constants.
    * `var` is the traditional way of declaring variables.

        ```js
        // i visible here (undefined).
        for (var i = 0; i < 3; i++) {
            // i visible here.
        }
        // i visible here.
        // let i = 0 makes i only visible in the for loop.
        let a = 1, b = 2; // Possible to declare multiple variables in one line.
        ```

2. `undefined` vs `null` vs `isNaN()`:
    * `undefined` is used for:
        * Unassigned/ Uninitiated variables;
        * A missing property for an object;
        * A missing value for an array;
    * `null` is used for uncreated objects (like `.getElementById()`'s returned value;)
    * `NaN` is a number that can't be represented (0/0).

3. `===` is the strict equality check (both the type and value) while `==` is not strict.

4. `===` between two object references will be true only if they refer to the same object.

5. Strings:

    ```js
    // There's no character type in JS.
    'hello'.length; // 5
    'hello'.charAt(1); // 'e'
    'hello'.toUpperCase(); // 'HELLO'
    '1' + 2 + 3; // '123'
    1 + 2 + 3; // '33'
    '' + 2; // '2'. A useful way to convert to string.
    'A' === '\u0041'; // \ is the escape character.
    const foo = 42;
    `The answer is ${foo}`; // Back ticks give us template literals. They also allow multiline strings.
    // JS uses UTF-16, which is not enough for all the symbols. So a single 16-bit code unit is used for common characters but two code units are used for rare ones.
    ```

6. Like Python, JavaScript also has truthy and falsy booleans:
    * false: false, 0, '', NaN, null, undefined.
    * true: all the others.

7. Two other ways of for loops:

    ```js
    let list = [4, 5, 6];

    for (let i in list) {
        console.log(i); // "0", "1", "2". Indices only.
    }

    for (let i of list) {
        console.log(i); // "4", "5", "6". Values
    }
    ```

8. Arrays are a special type of object:

    ```js
    let array = ['dog', 'cat', 'panda'];
    array.length; // 3
    array[100] = 'fox'; // Create a sparse array.
    array.length; // 101.
    typeof array[90]; // undefined.
    array.push('new_item');
    let new_item = array.pop(); // new_item.
    array.forEach(element => console.log(element));
    delete array[1]; // Because arrays are really objects.
    typeof array; // object.
    let foo = array.concat([1, 2, 3]); // ['dog', 'cat', 'panda', 1, 2, 3].
    let bar = array.join('#');
    array.reverse();
    array.sort(function (a, b) {
        return a - b;
    });
    array.slice(1, 3); // Like in Python.
    let squared = array.map(a => a * a);
    let filtered = array.filter(a => a.length > 3);
    ```

9. `/**/` can also occur in regular expression literals, so it's recommended to use `//` instead.

10. `regexp.test(string)` returns true if the string matches. `let re1 = /abc/;` and `let re1 = new RegExp('abc');` are equivalent.

11. Destructuring (similar to Python and Kotlin):

    ```js
    const x = [1, 2];
    const [y, z] = x; // y = 1, z = 2.
    let a = 1, b = 2;
    [a, b] = [b, a]; // Swap like in Python.
    ```

12. Exceptions:

    ```js
    throw new Error('Foo.');
    try {
    } catch (e) {
    }
    ```

13. Maps are better than objects for key-value pairs, as prototype properties are also present in objects. And objects can only have strings as keys.

    ```js
    let ages = new Map();
    ages.set('Bob', 1);
    ages.set('Jack', 10);
    ages.set('Kate', 0);
    ages.get('Bob'); // 1.
    ages.has('Kate'); // True.
    ```

14. `'use strict;'` at the top of a file or a function body enables the strict mode to catch errors.

15. `import ordinal from "ordinal";` is the ES module syntax for importing.

## Numbers

1. Two built-in numeric types: `Number` and `BigInt`. Integers are implicitly floats (64-bit like `Double`): `3 / 2 = 1.5`. This is good as short integer overflows are avoided.

2. `parseInt()` and `parseFloat()`:

    ```js
    parseInt('123'); // 123
    parseInt('11', 2); // 3
    parseInt('hi'); // NaN
    parseFloat('1.2'); // 1.2
    parseFloat('123.2abc'); // 123.2. Parse until invalid char.
    ```

3. `NaN`:

    ```js
    Number.isNaN(NaN); // true. It's only true when the parameter is truly NaN.
    // The global isNaN gives unintuitive behavior, do not use!
    ```

4. `Infinity`:

    ```js
    1 / 0; // Infinity
    - 1 / 0; // -Infinity
    isFinite(Infinity); // false
    isFinite(NaN); // false
    ```

5. `1e2` is the same as 100.

6. `Math.floor()` converts a number to an integer.

## Functions

1. JavaScript has first-class support for functions. Functions are linked to `Function.prototype`, which is self is linked to `Object.prototype`.

2. The named parameters are more like guidelines:
    * Calling a function without enough parameters gives undefined.
    * Calling a function with more parameters than expected ignores the extra parameters.
    * `arguments` is an array-like object holding all the values passed in. The rest parameter syntax `...args` is preferred for ES6. We can also pass in an array with the spread operator `...numbers`.

        ```js
        function foo(...args) {
            for (let arg of args) {}
        }
        ```

3. Anonymous functions:

    ```js
    const foo = function() {}; // Equivalent to function foo().
    // This is called a function expression, which is not hoisted (unlike function declaration).
    ```

4. Arrow function expressions are compact alternatives to traditional functions, but they can't be used in all situations.

    ```js
    const animals = ['cat', 'dog', 'panda'];
    console.log(animals.map(animal => animal.length));
    // Multiple arguments or no arguments require the parentheses.
    (a, b) => a + b;
    () => console.log('No arguments.');
    // Multiline body requires braces and return.
    (a, b) => {
        let foo = 42;
        return a + b + foo;
    }
    let foo = a => a + 100; // Named function.
    ```

5. JS has by default function scopes instead of block scopes, which means a variable defined anywhere in a function is visible everywhere in the function. This also causes it to be better to declare all the variables in a function at the top instead of as late as possible. (ONLY applicable to `var` instead of `let`.)

6. `function.apply(thisArg, argArray)` sets `this` with the first parameter.

## OOP

1. Objects are like Dictionaries in Python and HashMaps in Java.

2. `let obj = {};` is the preferred way to create an empty object.

3. Classic prototype-based example:

    ```javascript
    let foo = {
        name: "bar",
        coding: function() {
            this.name = "coding bar";
            alert.log("I'm coding now.");
        },
        age: 17
    };
    foo.height = 190; // This adds a new property.
    console.log(foo["name"]); // Also works, though the dot notation is preferred.
    delete foo.age; // This deletes the property.
    for (let prop in foo) { // Print all properties.
        console.log(prop + ": " + foo[prop]);
    }
    let bar = foo.bar || "default"; // || gives the default value,
    foo.bar.foobar; // Throw TypeError, as foo.bar is undefined.
    foo.bar && foo.bar.foobar; // Prevent TypeError.
    ```

4. We use functions to define custom objects and prototypes to add methods. A prototype is an object shared by all instances of that class. This is similar to extension functions in Kotlin and Swift. We can do the same for built-in objects (augmenting types).

    ```js
    function Person(first, last) {
        this.first = first;
        this.last = last;
    }
    Person.prototype.fullName = function() {
        return this.first + ' ' + this.last;
    };
    let person = new Person('foo', 'bar');
    ```

5. Delegation is used for prototype chaining. A retrieval looks up the property in the object, then the prototype, then the prototype's prototype, and eventually `Object.prototype`.

6. `typeof` looks up the prototype chain. We can use `hasOwnProperty()` to check if the object has a property without looking up the chain: `flight.hasOwnProperty('constructor')`.

7. Classes are built on prototypes, so they are also special functions.

    ```js
    // Class declarations are not hoisted.
    class Rectangle {
        constructor(height, width) {
            this.height = height;
            this.width = width;
        }
        // Static can't be called with an instance. We must use the class name.
        static displayName = "Foo";
        static bar() {
            console.log("I'm bar");
        }
        get area() {
            return this.calcArea();
        }
        calcArea() {
            return this.height * this.width;
        }
    };
    const square = new Rectangle(10, 10);
    console.log(square.area);
    square.displayName; // undefined.
    Rectangle.displayName; // Correct!
    // Class expressions are hoisted.
    let Rectangle = class {
        constructor(height, width) {
            this.height = height;
            this.width = width;
        }
    };
    class Square extends Polygon {
      constructor(length) {
        super(length, length);
        // Note: In derived classes, super() must be called before you
        // can use 'this'. Leaving this out will cause a reference error.
        this.name = 'Square';
      }

      get area() {
        return this.height * this.width;
      }
    }
    ```

8. `a instanceof b` is the same as Python's `isinstance(a, b)`.

## DOM (Document Object Model)

1. `document.getElementById("foo").innerHTML` gives the content of the html element with the id 'foo'. JavaScript does this by interacting with the DOM.

2. `foo.setAttribute("class", "bar")` sets the attribute and `var text = document.getElementById("bar").getAttribute("alt")` gets the 'alt' attribute.

## Handling events

1. `onclick`:

    ```javascript
    const image = document.getElementById("foo");
    image.onclick = bar();
    function bar() {
        let img = document.getElementById("foo");
        img.src = "new.png"; // We can change the property when we have the element.
    }
    const images = document.getElementsByTagName("img"); // Get a bunch
    for (let i = 0; i < images.length; i++) {
        images[i].onclick = bar();
    }
    ```

## Pitfalls

1. `typeof null` gives `object`. A better null test is simply `foo === null`. `typeof []` also gives `object`. `typeof NaN` gives `number`.

2. `parseInt('16 tons')` returns 16 without warnings.

3. `NaN === NaN` returns false.

4. Always use `===` and `!==` to avoid surprises.

5. Bitwise operations in JS are very slow and far from the hardware, as JS doesn't have integers and have to to conversions.

6. JS declarations are hoisted. This means a variable with `var` can be used before the declaration. Hoisting moves all declarations to the top of the scope.

7. If we forget to use `new`, `this` is bound to the global object and the function will clobber  global variables.

8. Avoid `void`.

## JSON

1. JSON strings use double quotes. All property names are strings. No functions or comments allowed.

2. `JSON.parse()` converts JSON into a JS object. `JSON.stringify()` converts a JS object to a JSON string.

## Asynchronicity

1. `Promise` is like `Future` in Java.

    ```js
    const promise2 = doSomething().then(successCallback, failureCallback);
    // Chaining:
    doSomething()
    .then(result => doSomethingElse(result))
    .then(newResult => doThirdThing(newResult))
    .then(finalResult => {
        console.log(`Got the final result: ${finalResult}`);
    })
    .catch(failureCallback);
    const myPromise = new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('foo');
        }, 300);
    });
    // Promise.all rejects immediately when any input rejects.
    var p1 = Promise.resolve(3);
    var p2 = 1337;
    var p3 = new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve("foo");
        }, 100);
    });
    Promise.all([p1, p2, p3]).then(values => {
        console.log(values); // [3, 1337, "foo"].
    });
    ```

2. `async` and `await` are like in Dart. Async functions always return a promise.

    ```js
    async function foo() { // This returns a different reference.
        return 1;
    }
    // is similar to
    function foo() { // This returns the same reference.
        return Promise.resolve(1);
    }

    async function foo() {
        await 1;
    }
    // is equivalent to
    function foo() {
        return Promise.resolve(1).then(() => undefined);
    }
    // Rewrite chaining:
    function getProcessedData(url) {
        return downloadData(url) // returns a promise
            .catch(e => {
                return downloadFallbackData(url);  // returns a promise
            })
            .then(v => {
                return processDataInWorker(v);  // returns a promise
            })
    }
    async function getProcessedData(url) {
        let v;
        try {
            v = await downloadData(url);
        } catch (e) {
            v = await downloadFallbackData(url);
        }
        return processDataInWorker(v);
    }
    ```

## References

* [A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

* [Head First JavaScript Programming: A Brain-Friendly Guide, 1st Edition](https://www.amazon.com/Head-First-JavaScript-Programming-Brain-Friendly/dp/144934013X/ref=sr_1_1?crid=2NISC4BUYXL03&keywords=head+first+javascript+programming&qid=1562253717&s=gateway&sprefix=head+first+javascript%2Caps%2C402&sr=8-1)

* <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions>

* [JavaScript: The Good Parts by Douglas Crockford](https://www.goodreads.com/book/show/2998152-javascript?from_search=true&from_srp=true&qid=a2NTYmkfEm&rank=1)

* <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes>

* <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises>

* <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function>

