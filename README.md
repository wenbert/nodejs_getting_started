- [Node.js: Getting Started](#nodejs--getting-started)
  * [Javascript review](#javascript-review)
    + [Variables and block scopes](#variables-and-block-scopes)
    + [Arrow functions](#arrow-functions)
    + [Object literals](#object-literals)
    + [Destructuring and rest/spread](#destructuring-and-rest-spread)
    + [Template strings](#template-strings)
    + [Classes](#classes)
    + [Promises and async/await](#promises-and-async-await)
  * [NPM: Node Package Manager](#npm--node-package-manager)
  * [Semantic Versioning (SemVer)](#semantic-versioning--semver-)
  * [Creating and Publishing an NPM Package](#creating-and-publishing-an-npm-package)
    + [Useful NPM commands](#useful-npm-commands)
  * [Next: Modules and Concurrency](#next--modules-and-concurrency)
  * [Examples of Module APIs](#examples-of-module-apis)
    + [Object example](#object-example)
    + [Array example](#array-example)
    + [String example](#string-example)
  * [Global Object](#global-object)
  * [Event Loop](#event-loop)
  * [Error vs. Exceptions](#error-vs-exceptions)
  * [Node Clusters](#node-clusters)
  * [Node's Asynchronous Patterns](#node-s-asynchronous-patterns)
    + [Synchoronous](#synchoronous)
    + [Callback function](#callback-function)
    + [Nested Callback function](#nested-callback-function)
    + [Promisify](#promisify)
    + [Async await](#async-await)
  * [Event Emitter](#event-emitter)
  * [Working with Web Servers](#working-with-web-servers)
    + [Monitoring Files for Changes](#monitoring-files-for-changes)
    + [The "req" and "res" Objects](#the--req--and--res--objects)
  * [Node Web Frameworks](#node-web-frameworks)
    + [ExpressJS Web Framework](#expressjs-web-framework)
    + [Template Languages](#template-languages)
  * [Working with the Operating System](#working-with-the-operating-system)
    + [Debugging:](#debugging-)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


# Node.js: Getting Started

https://app.pluralsight.com/player?course=nodejs-getting-started&author=samer-buna&name=086f1e0a-2406-497a-8fb1-6cdcde24b8fb&clip=4

Node.js Getting Started
* Getting Started with Node
* Modern Javascript
...
...

## Javascript review
### Variables and block scopes
```javascript
var foo = 0;
let foo = 0;
const foo = 0;
```

### Arrow functions
```javascript
const square = (a) => {
  return a * a;
};

// const square = (a) => a * a;
// const square = a => a * a;

[1, 2, 3, 4].map(a => a * a);
```

### Object literals
```javascript
const obj = {
  p1: 10,
  p2: 20,
  f1() {},
  f2: () => {},
};

```

### Destructuring and rest/spread
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

### Template strings
```javascript
const greeting = "Hello World";

const answer = 'Forty Two';

const html = `
  <div>
    ${Math.random()}
  </div>
`;

```

### Classes
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

### Promises and async/await
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

## NPM: Node Package Manager
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

## Next: Modules and Concurrency
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

## Examples of Module APIs

### Object example
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

### Array example
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

### String example
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

## Global Object
```javascript
// dir.js
// console.dir() is like console.log() but only shows top-level items
// We use console.dir() because `global` is a big object
console.dir(global, {depth: 0});
```
Output is:
```
$ node dir.js
Object [global] {
  DTRACE_NET_SERVER_CONNECTION: [Function],
  DTRACE_NET_STREAM_END: [Function],
  DTRACE_HTTP_SERVER_REQUEST: [Function],
  DTRACE_HTTP_SERVER_RESPONSE: [Function],
  DTRACE_HTTP_CLIENT_REQUEST: [Function],
  DTRACE_HTTP_CLIENT_RESPONSE: [Function],
  global: [Circular],
  process: [process],
  Buffer: [Function],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function],
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function] }
```

You can add/set something like:
```javascript
global.answer = 42;
```
LOL These are bad. Avoid global objects.

## Event Loop
...

## Error vs. Exceptions
```javascript
const path = require('path');
const fs = require('fs');

const files = ['.bash_profile', 'kjkjhh', '.npmrc'];

files.forEach(file => {
  try {
    const filePath = path.resolve(process.env.HOME, file);
    const data = fs.readFileSync(filePath);
    console.log('File data is', data);
  } catch (err) {
    if (err.code === 'ENOENT') {
      console.log('File not found  ');
    } else {
      throw err;
    }
  }
});
```

## Node Clusters
* Master process can restart other workers when then have problems (?)
* PM2 Tool for Production

## Node's Asynchronous Patterns

### Synchoronous
```javascript
const fs = require('fs');
const data = fs.readFileSync(__filename);
console.log('File data is', data);
console.log('TEST');
```

### Callback function
And
```javascript
const fs = require('fs');
fs.readFile(__filename, function cb(err, data) {
  console.log('File data is', data);
});
console.log('TEST');
```
Output:
```
TEST
File data is: ....
```
That is first pass is `fs.readFile`, then `console.log()`. Then when the callback function is done, it calls the internal `console.log()` inside.

### Nested Callback function
```javascript
const fs = require('fs');

fs.readFile(__filename, function cb1(err, data) {
  fs.writeFile(__filename + '.copy', data, function cb2(err) {
    // Nest more callbacks here...
  });
});

console.log('TEST');

```

### Promisify
```javascript
const fs = require('fs');
const util = require('util');

const readFile = util.promisify(fs.readFile);

async function main() {
  const data = await readFile(__filename);
  console.log('File data is', data);
}

main();

console.log('TEST');

```

Using built-in libraries for some libraries:
```javascript
const { readFile } = require('fs').promises;

async function main() {
  const data = await readFile(__filename);
  console.log('File data is', data);
}

main();

console.log('TEST');
```

Promises are better than callbacks. Easier, etc.

### Async await
```javascript
const fs = require('fs').promises;

async function main() {
  const data = await fs.readFile(__filename);
  await fs.writeFile(__filename + '.copy', data);
  // More awaits here...
}

main();
console.log('TEST');

```

## Event Emitter
```javascript
// example.js
const EventEmitter = require('events');

// Streams are Event Emitters
// process.stdin, process.stdout

const myEmitter = new EventEmitter();

// This will not output anything. We "emit" before we Subscribe
myEmitter.emit('TEST_EVENT');

// If we want to do "emit" before the Subscription we can use 
// setImmediate()
setImmediate(() => {
  myEmitter.emit('TEST_EVENT');
});

// Subscribe
myEmitter.on('TEST_EVENT', () => {
  console.log('TEST_EVENT was fired');
});

// This will "emit" the event so the callback in TEST_EVENT (Subscribe)
// will get called.
myEmitter.emit('TEST_EVENT');
```

Output:
```
$ node example.js 
TEST_EVENT was fired
TEST_EVENT was fired
```
This is because `A` does not do anything. We emit before `Subscribing`.
`B` outputs the first `TEST_EVENT was fired` - due to `setImmediate()`
`C` is we `Subscribe` to `TEST_EVENT` - this gets called when we `emit` something - in this case `TEST_EVENT`.
`D` outpus the second `TEST_EVENT was fired` - we emit after we `Subscribed` to `TEST_EVENT`.

**Very simple yet very powerful. Because it allows modules to work together without depending on any APIs!**

## Working with Web Servers
The built-in HTTP module
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.end('Hello World\n');
});

server.listen(4242, () => {
  console.log('Server is running...');
});
```

Let's make that previous code snippet a bit more readable.
```javascript
const http = require('http');

const requestListener = (req, res) => {
  res.write('Hello World\n');
  res.end();
};

// See previous topic about "Event Listeners" - this is exactly it!
const server = http.createServer();
server.on('request', requestListener);

server.listen(4242, () => {
  console.log('Server is running...');
});
```

### Monitoring Files for Changes
Because the event loop is constantly running, when you change something in the file above, the changes will not reflect unless you reload the script. ie: Stop then start.

To go around this, we use a library called Nodemon. 

```
$ npm -i nodemon
$ nodemon hello-world.js
```

Now, it will monitor the files for changes and then nodemon will reload the file/script.

### The "req" and "res" Objects
`console.log()` or `console.dir()` these lol!
Keywords: `IncomingMessage` and `ServerReponse`

`req` and `res` are streams! 
`req` object is readable stream while the `res` object is a writable one.
That means we can SUBSCRIBE to them!

## Node Web Frameworks
### ExpressJS Web Framework
First steps:
```
$ npm init
$ npm install express
```

```javascript
// index.js
const express = require('express');

const server = express();

// We don't define a single request listener.
// We define MANY listeners - per URL

server.get('/', (req, res) => {
  res.send('Hello Express');
});

server.get('/bar', (req, res) => {
  res.send('Hello Express');
});


server.listen(4242, () => {
  console.log('Express Server is running...');
});
```

```
$ nodemon index.js
...
Express Server is running...
```
Then go to browser port 4242. And also `/bar`.

### Template Languages
Pug, Handlebars, EJS.
```
$ npm install express
$ npm install ejs
```
```javascript
// index.js
const express = require('express');

const server = express();

// THIS!
server.set('view engine', 'ejs');

server.get('/', (req, res) => {
  res.render('index');
});

server.get('/about', (req, res) => {
  res.render('about');
});

server.listen(4242, () => {
  console.log('Express Server is running...');
});
```

Then inside `views` directory.
```javascript
// views/index.ejs
<!DOCTYPE html>
<html lang="en">
<head>
  <title>INDEX</title>
</head>
<body>
  <h2>Hello EJS</h2>
</body>
</html>
```
And 
```javascript
// views/about.ejs
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>INDEX</title>
</head>
<body>
  <h2>About...</h2>
</body>
</html>

```

## Working with the Operating System
The `os` module
```javascript
const os = require('os');
console.log('OS platform:', os.platform());
console.log('OS CPU architecture:', os.arch());
console.log('# of logical CPU cores', os.cpus().length);
console.log('Home directory for current user', os.homedir());
console.log('line 1' + os.EOL + 'line 2' + os.EOL + 'line 3');
```

The `fs` module
```javascript
/*

  readFile(path[, options])
  createReadStream(path[, options])

  writeFile(file, data[, options])
  createWriteStream(path[, options])

  appendFile(path, data[, options])
  copyFile(src, dest[, flags])
  stat(path[, options])
  access(path[, mode]), chmod(path, mode), chown(path, uid, gid)
  link(existingPath, newPath), unlink(path)
  truncate(path[, len])

  mkdir(path[, mode])
  readdir(path[, options])
  rmdir(path)
  rename(oldPath, newPath)

*/
```

The `cp` module
```javascript
const { spawn } = require('child_process');

// Print Working Directory
const pwd = spawn('pwd');
pwd.stdout.pipe(process.stdout);

// Read content of a file
const { HOME } = process.env;
const cat = spawn('cat', [`${HOME}/.bash_profile`]);
cat.stdout.pipe(process.stdout);

// List files
const ls = spawn('ls', ['-l', '.']);
ls.stdout.pipe(process.stdout);

// Use Shell Syntax
const shell = spawn('ls -al ~ | wc -l', { shell: true });
shell.stdout.pipe(process.stdout);
```

### Debugging:
```
$ node --inspect-brk file.js
```
Then go to: `chrome://inspect` in your Chrome browser.
Then you will see the node process inside "Targets". Then click on the "Inspect" link.

You should see something like:
```javascript
function convertArrayToObject(arr) {
  return arr.reduce((curr, acc) => {
    acc[curr[0]] = curr[1];
    return acc;
  }, {});
}

const obj = convertArrayToObject([
  [1, 'One'],
  [2, 'Two'],
  [3, 'Three'],
  [4, 'Four'],
  [5, 'Five'],
]);

console.log(obj);
/* Should output:

  {
    1: 'One',
    2: 'Two',
    3: 'Three'
    4: 'Four',
    5: 'Five'
  }

*/
```

Put a `breakpoint` then click on `Play` button. You can hover on variables, etc.
