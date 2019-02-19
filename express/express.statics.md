## Express statics

Previously we've created individual statics for every page we wanted to service. Express makes it so we don't need to do this by hand, by handling the requests in a smarter way.

Let's start by trying to add a stylesheet to this page:

```javascript
const express = require('expresss');
const app = express();
const port = 8000;

app.get('/', (req, res) => {
  res.set('Content-Type', 'text/html');
  res.write('<!DOCTYPE html>');
  res.write('<html>');
  res.write('<head><title>Homepage</title>');
  res.write('<link rel="stylesheet" type="text/css" href="/css/layout.css">');
  res.write('</head>');
  res.write('<body><h1>Homepage</h1></body>');
  res.write('</html>');
  res.send();
});

app.listen(port);
```
This correctly links to our CSS page, but we need to create the folders and then the actual CSS file. First, let's create a `public` folder to house all of our public data. Then, inside of that folder, we can create a `css` folder and inside of that our `layout.css` file. To allow express to know where this file is, we need to create a `static`, which will hold the location of our file so that Express knows where it is. This is called `middleware`, and will be used more later. So, we can register this middleware as such: `app.use(express.static('public'));`

Putting this into our page should now properly load our CSS file onto the page:

```javascript
const express = require('expresss');
const app = express();
const port = 8000;

app.use(express.static('public'));

app.get('/', (req, res) => {
  res.set('Content-Type', 'text/html');
  res.write('<!DOCTYPE html>');
  res.write('<html>');
  res.write('<head><title>Homepage</title>');
  res.write('<link rel="stylesheet" type="text/css" href="/css/layout.css">');
  res.write('</head>');
  res.write('<body><h1>Homepage</h1></body>');
  res.write('</html>');
  res.send();
});

app.listen(port);
```
This is effectiely telling express to look inside of our public folder for our css folder before it does anything else. This also means that we can create something like an `about.html` page and include that as a static page using Express statics, because express now knows it's there. This saves us from having to use a ton of `res.write()` statements. Common practice is to have separate folders in public for css, images, and javascript.

We can also change the prefix of our statics: `app.use('/assets', express.static('public'));`
This aliases public as `/assets`. So, to link back to our CSS, we'd have to chage our URL to `/assets/css/layout.css`.
