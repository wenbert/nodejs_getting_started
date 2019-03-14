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
... to do 