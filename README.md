# Node.js: Getting Started

https://app.pluralsight.com/player?course=nodejs-getting-started&author=samer-buna&name=086f1e0a-2406-497a-8fb1-6cdcde24b8fb&clip=4

Node.js Getting Started
* Getting Started with Node
* Modern Javascript
...
...

# Javascript review
## Variables and block scopes
```javascript
var foo = 0;
let foo = 0;
const foo = 0;
```

## Arrow functions
```javascript
const square = (a) => {
  return a * a;
};

// const square = (a) => a * a;
// const square = a => a * a;

[1, 2, 3, 4].map(a => a * a);
```

## Object literals
```javascript
const obj = {
  p1: 10,
  p2: 20,
  f1() {},
  f2: () => {},
};

```

## Destructuring and rest/spread
```javascript
// const PI = Math.PI;
// const E = Math.E;
// const SQRT2 = Math.SQRT2;

const { PI, E, SQRT2 }  = Math;

// With require
// const { readFile } = require('fs');


// const circle = {
//   label: 'circleX',
//   radius: 2,
// };
//
// const circleArea = ({ radius }) =>
//   (PI * radius * radius).toFixed(2);
//
// console.log(
//   circleArea(circle)
// );

const [first, ...restOfItems] = [10, 20, 30, 40];

const data = {
  temp1: '001',
  temp2: '002',
  firstName: 'John',
  lastName: 'Doe',
};

const { temp1, temp2, ...person } = data;

const newArray = [...restOfItems];

const newObject = {
  ...person,
};

```

## Template strings
```javascript
const greeting = "Hello World";

const answer = 'Forty Two';

const html = `
  <div>
    ${Math.random()}
  </div>
`;

```

## Classes
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(`Hello ${this.name}!`);
  }
}

class Student extends Person {
  constructor(name, level) {
    super(name);
    this.level = level;
  }
  greet() {
    console.log(`Hello ${this.name} from ${this.level}`);
  }
}

const o1 = new Person("Max");
const o2 = new Student("Tina", "1st Grade");
const o3 = new Student("Mary", "2nd Grade");

o3.greet = () => console.log('I am special!');

o1.greet();
o2.greet();
o3.greet();

```

## Promised and async/await
```javascript
const https = require('https');

function fetch (url) {
  return new Promise((resolve, reject) => {
    https.get(url, (res) => {
      let data = '';
      res.on('data', (rd) => data = data + rd);
      res.on('end', () => resolve(data));
      res.on('error', reject);
    });
  });
}

fetch('https://www.javascript.com/')
  .then(data => {
    console.log(data.length);
  });

  (async function read() {
    const data = await fetch('https://www.javascript.com/');

    console.log(data.length);
  })();

```

# NPM: Node Package Manager
Init. Creates `package.json` etc.
```
$ npm init
```

```
$ npm i -D nodemon
```
`-D` means it is a "Dev Dependency" and not needed in production.

Testing frameworks, formatting tools, etc. belong in "dev dependencies".

```
$ npm install --production
```
When only install "non dev".

## Semantic Versioning (SemVer)

`4.2.0`
```
4: Major - Breaking Changes
2: Minor - Backward Compantible
0: Patch - Bug Fixes. No new features. No breaking changes.
```

`~1.2.2`
`~` Update can install most recent `patch` version. So is greater than or equal to:
```
1.2.4
1.2.5
1.2.6
1.2.x
```
But not:
```
1.3.0
```

`^1.2.3`
`^` More relaxed constraint. Get the most recent `minor` version (middle number).
```
1.3.0
1.4.5
```
But not all the way to:
```
2.0.0
```

## Creating and Publishing an NPM Package

Module exports
```javascript
//File path: `wenbert-frame-print/index.js`
module.exports = function print(msg) {
  console.log('**********');
  console.log('msg');
  console.log('**********');
};
```

```javascript
//File path: `index.js`
const print = require('../fromt-print');
print('Hello NPM!');
/*
Expected Output:

**********
Hello NPM!
**********
*/
```

By default, Node looks up `node_modules` but if it can't find it, it will look-up the relative path.

The goal is to do:
```
$ npm install wenbert-frame-print
```

We put a prefix `wenbert` so that it will be unique in npmjs.com. Create an account in npmjs.com

```
$ npm login
Username: wenbert
Password:
Email: (this IS public) wenbert@gmail.com
Logged in as wenbert on https://registry.npmjs.org/.
```

Then create a package.json file.
```
$ npm init
```
Enter `wenbert-frame-print` or some unique string. Follow instructions / keep everything as default.

Then finally:
```
$ npm publish
```

Now that you have package published, you can:
```
$ npm install wenbert-frame-print
```

It will download the package and place it inside the `node_modules` directory.

### Useful NPM commands
```
$ npm ls
```
And
```
$ npm outdated
```
This will not update the package. 
`Current` is what you have.
`Wanted` is what you'll get when you `npm update`
`Latest` is the latest version.

Be careful, because even when it is only a patch level update, it might still introduce bugs in your code.

# Next: Modules and Concurrency
```javascript
function dynamicArgsFunction() {
  console.log(arguments);
}

dynamicArgsFunction(3, 7, 5, 4);

// OUTPUT
// $ node arguments.js
// [Arguments] {'0': 3 ... '3':4}
```

Take this file with 1 line. And we run the file in Node. The output would not be "undefined". 
```javascript
// arguments.js
console.log(arguments);
```
*Important*: Node internally wraps every file in a function, you would get some output here. As opposed to running this in a browser - you would get some kind of "undefined" message.

Node does something like this:
```javascript
function (exports, module, require, __filename, __dirname) {
  // Your code in arguments.js
  console.log(arguments);

  // So if you use exports.a = 42
  // You are just using the param inside the wrapping function.
  // That is why if you do:
  let foo = 2;
  // That does not become a global variable. It is only in the 
  // scope of the wrapping function. 
  // If that was done in a browser, that `foo` will be a global one.

  exports.a = 42;
  module.exports.b = 37;

  exports = () => {} // NOT OK
  module.exports = () => {}; // OK

  // Returns for every file in the "wrapper". Node will always return
  // this function. 
  return module.exports;
}
```

So if we do:
```javascript
const moduleApi = require('./arguments.js');
// The result of the "require" call is really the "module.exports"
// of "arguments.js" - which is what Node will always return in the background.
console.log(moduleApi);
```

### Examples of Module APIs

#### Object example
```javascript
// object.js
// Top-level API is a simple object (no need to use module.exports)
exports.language = 'English';

exports.direction = 'RTL';

exports.encoding = 'UTF-8';
```

Then we use it like:
```javascript
// use-objects.js
const api = require('./object.');
console.log(api.language, api.direction, api.encoding);
```

We run it, output is:
```
$ node use-object.js
English RTL UTF-8
```

#### Array example
```javascript
// array.js
// Top-level API is an array
module.exports = [2, 3, 5, 7];
```

Then we use it:
```javascript
// use-array.js
console.log(require('./array.js'));
```

Run it:
```
$ node use-array.js
[2, 3, 5, 7]
```

#### String example
We can also return a string.
```javascript
// string.js
module.exports = `
  <html>
  <head>A title</head>
  <body>Foo bar!</code>
  </html>
`;
```

We use it like so:
```javascript
// use-string.js
const myTemplate = require('./string.js');
console.log(myTemplate);
```

Output:
```
$ node use-string.js
<html>
<head>A title</head>
<body>Foo bar!</code>
</html>
```

Let say you want an HTML template but you want to pass in variables.
You do it like this - by exporting a function.

```javascript
// string2.js
module.exports = title = `
  <html>
  <head>${title}</head>
  <body>Foo bar!</code>
  </html>
`;
```

We use it like so:
```javascript
// use-string2.js
const templateGenerator = require('./string.js');
const myTemplate = templateGenerator('Hello World!');
console.log(myTemplate);
```

Output:
```
$ node use-string2.js
<html>
<head>Hello World!</head>
<body>Foo bar!</code>
</html>
```

