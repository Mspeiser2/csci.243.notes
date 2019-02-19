## Receiving and parsing chunks

We can now use our subscription to listen for requests that have an HTTP message body. We can easily simulate requests using the `postman` tool, which will build and send requests for us.

### Using postman

We can go to the `body` tab and change the type to `x-www-form-urlencoded` which will automatically encode the request in the URL as if it was sent through a form, Then, we can fill out some key-value pairs to test our page. So, let's get our page set up to test:

```javascript
const http = require('http');
let port = 8000;

const server = http.createServer(function (req, res) {
  if(req.method == 'GET' && req.url == '/')
  {
    return res.end('Welcome');
  }
  else if (req.method == 'POST' && req.url == "/info")
  {

  }

});

server.listen(port);
```
if you look at the raw code of the request, you'll notice that the actual message body is after all of the headers, using a Carriage Return Line Feed (two line breaks). So, we need to tell our page when to start reading that message body, We also don't know what size that body is going to be, so we chunk that body up so we don't have to keep them in our RAM. This helps process large requests, because we don't have to handle the whole request at once. We can do this as such:

```javascript
req.on('data', (chunk) => {
  console.log(chunk);
});
```

This should output to the terminal whenever we receive a chunk. If you send this request, Postman should show in it's own terminal that it got the request. If we look at our node terminal we might see something like this:
`<Buffer 66 69 72 73 74 3d 41 6e 64 72 26 6c 61 73 74 3d 42 65 73 6d 65 72>`. This is set up with a buffer because we don't know what type of data it is going to be. So, we an build up these chunks untul we have the whole body. We can only be sure that we have the whole body when we get the end signal from the page. We also shouldn't end our response until we're sure it's over, so we should put our res.end statement outside of where we're recieving our chunks:

```javascript
const http = require('http');
let port = 8000;

const server = http.createServer(function (req, res) {
  if(req.method == 'GET' && req.url == '/')
  {
    return res.end('Welcome');
  }
  else if (req.method == 'POST' && req.url == "/info")
  {
    req.on('data', (chunk) => {
      console.log(chunk);
    });

    req.on('end', () => {
      res.end('Got it!');
    });
  }

});

server.listen(port);
```
This ensures that we get all of the data, and don't end the response until we have the entire body. We can also add an object array to store all of our buffered data:
```javascript
let data = [];

req.on('data', (chunk) => {
  data.push(chunk);
  console.log('chunk', chunk);
});
```
This makes it so we have an array of all of our chunks. We can then add a concat statement to concatonate all this data into our actual message body, then output it to the console:
```javascript
req.on('end', () => {
  data = Buffer.concat(data);
  console.log('Data after concat', data);
  res.end('Got it!');
});
```
Even further, we can turn this into a string and then output it to see the actual body:
```javascript
req.on('end', () => {
  data = Buffer.concat(data);
  console.log('Data after concat', data);
  data = data.toString();
  console.log('Data after serialization', data);
  res.end('Got it!');
});
```
Which should turn our data into somewhat human readable data, that is URL encoded (in this case, since we were sending URL encoded form data).

So, we can then parse it to turn it into an object:

```javascript
const querystring = require('querystring');

req.on('end', () => {
  data = Buffer.concat(data);
  console.log('Data after concat', data);
  data = data.toString();
  console.log('Data after serialization', data);
  data = querystring.parse(data);
  console.log('Data after parsing', data);
  res.end('Got it!');
});
```
And then we can customize our response using the new data object we have:
```javascript
const querystring = require('querystring');

req.on('end', () => {
  data = Buffer.concat(data);
  console.log('Data after concat', data);
  data = data.toString();
  console.log('Data after serialization', data);
  data = querystring.parse(data);
  console.log('Data after parsing', data);
  res.end('Got it' + data.first + '!');
});
```
So, we've looked at the incoming request, built up the body into an array of chunks, combined it into a single concatenation of chunks, turned it into a string, and then parsed it out into the information we want. We can then use that data as we'd like.
