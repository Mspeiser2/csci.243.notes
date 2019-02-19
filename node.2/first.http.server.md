## Creating your first server

First, you can make this task easier by using some of node's core modules: in particular, the `http` modules. We do this by making a const that holds that module as such:
```javascript
const http = require('http');
```

Then, we need to create a port to host our server on, this is usually 8000 or 8080. We use these because they're free, and we don't need admin access to bind to them. We do this as such:
```javascript
let port = 8000;
```
We can assign it to a variable instead of hard coding it to make it easier to debug or change.

We then can create our server, by assigning to a constant our createServer method from the HTTP modules:
```javascript
const server = http.createServer(function (req, res) {

});
```

We then need to tell the server to listen, and the port it needs to listen on. Since we assigned port to a variable, we can do this as such:
```javascript
server.listen(port);
```
If you run the server using `node index.js` the process will hang until you kill it. This is because you set the server to listen, so it will sit there until further instruction. Because the server can receive requests, but doesn't know how to respond, if you were to try and visit the server, it won't send you anything back. The browser will then hang and wait for a response. A simple example response could be set up like this:

```javascript
const http = require('http');
let port = 8000;

const server = http.createServer(function (req, res) {
  res.end('It worked.');
});

server.listen(port);
```
This will end the request and send a response back to the browser. The browser will show just the text "It worked.", because that's all it recieved. So, we can use this to return HTML code and build pages.

If we examine the `createServer` function, we can see that there's `req` (request) and `res` (response) objects. We can dig deeper into these at a later point and use them to do some more useful stuff.

If we were to use `node --inspect-brk index.js` to inspect the program with breakpoints, we can set a breakpoint on the res.end call and then  release the program and make an HTTP request by refreshing the page. The debugger should then drop into our `createServer()` call and we can inspect everything about the response. There are a few important elements we can look at:

`req.url`: This is the URL the request was looking for, We can use this to decide where to send the request after we receive it.

So, using `req.method` and `req.url` we can direct the user appropriately:

```javascript
if (req.method == 'GET' && req.url == '/')
{
  res.end('Welcome to the homepage.');
}
else if (req.method == 'GET' && req.url == '/about')
{
  res.end('Welcome to the about page');
}
else
{
  res.end('Sorry, page not found.');
}
```

This code will correctly direct the requests to the appropriate pages. However, if we go attempt to go to the `/test` page, we will get our "Sorry, page not found" page. However, if we check the status code of this page it shows `200`, not `404`, which means that everything is fine. This is because it's now our job to change the status code, the server doesn't know by default to configure those codes. We can set the status code ourselves by changing parts of the `res` object.

```javascript
if (req.method == 'GET' && req.url == '/')
{
  res.end('Welcome to the homepage.');
}
else if (req.method == 'GET' && req.url == '/about')
{
  res.end('Welcome to the about page');
}
else
{
  res.statusCode = 404;
  res.statusMessage = "Not found!";
  res.end('Sorry, page not found.');
}
```
Now, if we visit a nonexistent page, we actually get a 404 error and the error message. This conveys to both the user and the browser that the request failed and appropriately returns a failed status code for the browser.
