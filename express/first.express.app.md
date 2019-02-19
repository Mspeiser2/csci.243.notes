## Using express

So far, we've been using the built in HTTP methods to handle everything. Using Express, we can do cool stuff, like change how we handle addresses, using routing. For example, if we wanted to be able to change the ending of a URL or didn't know what exactly it was going to say, we'd normally be out of luck. With Express, we can easily do some cool stuff with our addresses.

To begin using Express, we can install it using NPM. You should first use `npm init` to ensure you have a `package.json` file to store this dependency. Most developers also change their entry point to `app.js`, and you really don't have any reason to do otherwise. Then we can actually install express: `npm install express`.

Once we're all set up, we can create a constant to hold our package:

```javascript
const express = require('expresss');
```
And then we need to create a constant app that utilizes that framework:
```javascript
const express = require('expresss');
const app = express();
```
And, as usual, set our port:
```javascript
const express = require('expresss');
const app = express();
const port = 8000;

app.listen(port);
```
This is all we need to get our server running.

Express makes it easy to configure routes, using the `app` object we created. So, if we wanted to create a route that services a `get` request on our home (/) page:

```javascript
const express = require('expresss');
const app = express();
const port = 8000;

app.get('/', (req, res) => {
  res.send('Home page!');
});

app.listen(port);
```
This is somewhat similar to what we were doing before, except now we are only servicing the `get` requests to that specific address, instead of filtering through them with if statements like before.
