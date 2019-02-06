## NodeJS is an environment for Javascript.
You're still coding in Javascript, Node is just the enviornment in which you're now coding JS.

Instead of writing JS in the DOM, you're doing it in Node. There are some advantages to this, like access to the filesystem 
which you normally would not have on the DOM. The server will pass HTTP requests into your NodeJS app, which will process it 
and return a response, usually in a text/html document. Node can also respond in a binary format.

## Getting started with Node
You can start with a file named ___index.js___ but you don't have to, it's just common.

Running `node index.js` from a console will run your `index.js` file. For example, this is all the code necessary to put in a 
`index.js` file to get a "Hello World" output:
```
file: index.js
```
```
console.log("Hello World");
```

### Using multiple Node files
Normally, your document would pull together all of your node files so that they are in the same ###scope###. But, assuming 
that your `index.js` is your top level file, there are ways to access other Node files. This is done in a few steps:

1. Expose the parts of the Node file you want to access elsewhere.
To make this happen, you can create a ___module___ usually called `module.exports`. This can be done as follows:
```
file: funcs.js
```
```
function add(x,y)
{
    return x+y;
}

function sub(x,y)
{
    return x-y;
}

function mult(x,y)
{
    return x*y;
}

module.exports = {
    sub : sub,
    add : add
};
```
This makes anything put into the exports accessible outside of this file. In this case, `add()` and `sub()` would be 
accessible, but not `mult()`.

2. Pull that data in using a `require()` statement.
```
file: index.js
```
```
let funcs = require('./funcs.js');
```
This is functionally the same as:
```
funcs = {
    add : function(x,y) {
        return x+y;
    },
    sub : function(x,y) {
        return x-y;
    }
```
Except you don't have to have the functions in `index.js`, they're stored in `funcs.js`.


## Using these functions
You can access these functions easily using dot notation:
```
file: index.js
```
```
let funcs = require('./funcs.js');

console.log(funcs.add(5,3));
```
This would correctly output an 8 to the console.

```
file: index.js
```
```
let funcs = require('./funcs.js');

console.log(funcs.sub(5,3));
```
This would correctly output a 2 to the console.

```
file: index.js
```
```
let funcs = require('./funcs.js');

console.log(funcs.mult(5,3));
```
This would cause an error and not output correctly, becuase we did not put `mult()` into our export statement in `funcs.js`, 
so we don't have access to it in `index.js`.

## Using Node core modules
Node provides modules that are considered ___core___, which means they're all built in and you can easily access them. One 
common module is `queryString`, which deals with URL encoded data that you will receive from clients. Core modules can be used 
as such:
```
let qs = require('querystring'); //This pulls the querystring Node module in for use

console.log(qs.parse("first=Andrew&last=Besmer&message=Hi%20there")); //This is using querystring to process URL encoded data
```
Note that because we used `let qs = require('querystring');`, whenever we want to use `querystring` in this file, we would 
call it using `qs` and not `querystring`, as above. This would output:
```
[Object: null prototype] { first: 'Andrew', last: 'Besmer', message: 'Hi there' }
```
