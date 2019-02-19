## NodeJS EventEmitter

We can use the NodeJS `EventEmitter` class to subscribe to custom events and create custom events to allow people to subscribe to. This class is used to listen for parts of the message body and recreate it once we have all the parts.

First, we need to pull in the `EventEmitter` class in from the nodejs core modules:
`const EventEmitter = require('events').EventEmitter;`
And then we can create a new EventEmitter object: `let someObj = new EventEmitter();`.
Then we can emit to that object: `someObj.emit('customEvent');`
If we run this, nothing happens. That's because the object is being emitted, but nothing is listening for it. So, we can create the code that runs when the event is omitted by creating the subscription:

```javascript
someObj.on('customEvent', function() {
  console.log("Got the event.");
});
```

This subscription has to be written before the object is emitted. However, once we create this code, we can emit the object as many times as we like and the same code should still execute:

```javascript
const EventEmitter = require('events').EventEmitter;
let someObj = new EventEmitter();

someObj.on('customEvent', function() {
  console.log("Got the event.");
});

someObj.emit('customEvent');
someObj.emit('customEvent');
someObj.emit('customEvent');
```

Would putput `Got the event.` 3 times. Also take note that when we emit from `someObj` we are specifically emitting `customEvent`, this is important to match, otherwise it won't emit this event properly.
