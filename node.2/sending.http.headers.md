## Sending HTTP headers

If we include headers in our serer's response, we can change the response type to respond with more than just plaintext. If we look at the response, we can use `res.setHeader()` to set these headers. `setHeader()` is going to take in two arguments: First the content type, second the actual value of that header. So, if we set the `Content-Type` to `text/html`, we can respond with an HTML page. We could do this inside of `res.end`, but we only get to call that once. So, we should keep it at the end and we can leave it empty just to end the response. Instead, we can use `res.write()` to write as many lines of HTML as we'd like:

```javascript
const http = require('http');
let port = 8000;

const server = http.createServer(function (req, res) {
  if (req.method == 'GET' && req.url == '/')
  {
    res.setHeader('Content-Type', 'text/html');
    res.write('<!DOCTYPE html>');
    res.write('<html>');
    res.write('<head><title>Homepage</title></head>');
    res.write('<body><h1>Homepage</h1></body>');
    res.write('</html>');
    res.end();
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
});

server.listen(port);
```

### The other way

We can also use `res.writeHead()` to set our headers. Say for example that we want to set the status code to 200. `res.writeHead` takes in two arguments as well, but a little differently. First, you pass in your status code. Then, you pass in an object with as many headers as you'd like. This can be advantageous if you have a lot of headers to set:

```javascript
res.writeHead(200, {
  'Content-Type' : 'text/html'
});
```

## Using return statements

We didn't use return stateents here, which is fine, but can cause issues. Without return statements, the code will continue to run past your res.end statements. This sometimes wont cause an issue, as above, but can cause issues in bigger amounts of code. It also immediately exits the function call. The easiest way to do this is to return with your `res.end()` statement as follows:

```javascript
const http = require('http');
let port = 8000;

const server = http.createServer(function (req, res) {
  if (req.method == 'GET' && req.url == '/')
  {
    res.setHeader('Content-Type', 'text/html');
    res.write('<!DOCTYPE html>');
    res.write('<html>');
    res.write('<head><title>Homepage</title></head>');
    res.write('<body><h1>Homepage</h1></body>');
    res.write('</html>');
    return res.end();
  }
  else if (req.method == 'GET' && req.url == '/about')
  {

    res.writeHead(200, {
      'Content-Type' : 'text/html'
    });
    res.write('<!DOCTYPE html>');
    res.write('<html>');
    res.write('<head><title>About page</title></head>');
    res.write('<body><h1>Welcome to the about page.</h1></body>');
    res.write('</html>');
    return res.end();
  }
  else
  {
    res.statusCode = 404;
    res.statusMessage = "Not found!";
    return res.end('Sorry, page not found.');
  }
});
```
