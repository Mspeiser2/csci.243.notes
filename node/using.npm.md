## Using NPM

You can use `npm init` to initialize NPM and have it run you through everything you need. Alternatively, you could create your own `package.json` file and set it up manually, but using `npm init` is probably easier.

The following commands are the ones you'll probably use most frequently:
 - `npm init` runs the initializer to create your `package.json` and sets it up in a way which will probably (hopefully) work for you.

 - `npm help json` is the help file for NPM and will give you a more detailed description of all the available comands.

 - `npm install <pkg>` installs a package for you and makes it a dependency in your package.json file so it will continue to be tracked by NPM from then on.

 - `^C` (or CTRL C) at any time will quit the initializer.


 The initializer will first ask for a package name, which you can make whatever you'd like. It will then ask for a version number which you can decide for yourself. You can then enter a description for your program. The ___entry point___ is where your program's starting Javascript file is, usually `index.js` but you can change this to whatever you made your starting file. The test command and git repository are both optional, but helpful for testing and versioning. Keywords are used if you plan on sharing this package with others, so that they can search for and find it. The author is you or whatever company you're working for. License is whatever license you'd like to include with your software. The initializer will then confirm with you what information you entered before it writes it to your `package.json`. You can look over this information and then press enter to confirm. A typical `package.json` file would look like this:

 ```Json
{
  "name": "csci243",
  "version": "0.0.1",
  "description": "This app loads indefinitely and puts out funnies while doing it.",
  "main:": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Speiserm",
  "license": "ISC"
}
```
As you can see, it looks like a pretty standard `json` file and contains some information that we were asked about in the initializer. You could just as easily create this file by hand, without the initializer.

## Finding packages

The official place to look for packages is [Npmjs](npmjs.com). This is the official site of NPM, and you can find a ton of stuff on there. However, there is very little to no moderation, so the quality of these packages can be questionable at times. Searching for packages brings up packages which have tags similar to your search. Searching "funnies" brings up the exact package we're looking for for this project.

When you find the package you're looking for, you should first evaluate the quality of that package. Looking at the info on the right side of the page, you can see information that will help you evaluate this. Checking waht license it uses to see if you're allowed to use it for your purposes is important. You can also check weekly downloads to see if other people are commonly using that package. More importantly, you can check how many open issues the software has and determine if these will be a detriment to your program. You can also check the last publish date to see if the developer is still working on it.

## Using packages

You can install a package using `npm install <pkg>`. So, to use our funnies package, we would use `npm install funnies`. Package names in NPM have to be unique and contain no spaces, so there is no other `funnies` package. Because of this, the package name is always the unique identifier which you can use to pull down and install that package. This also means that you have to get the package name right, as `funnies` and `funny` are two completely different packages.

NPM will then download and install your package. Notably, it will also edit your `package.json` file to include your dependencies. If we compare the `package.json` file from earlier to the updated one after installing our `funnies` package:

```json
{
 "name": "csci243",
 "version": "0.0.1",
 "description": "This app loads indefinitely and puts out funnies while doing it.",
 "main:": "index.js",
 "scripts": {
   "test": "echo \"Error: no test specified\" && exit 1"
 },
 "author": "Speiserm",
 "license": "ISC",
 "dependencies": {
   "funnies": "^2.0.0"
 }
}
```

You can see that we now have a new `dependencies` entry in our package. This is where NPM keeps track of all of your dependencies and what version of them we're using. And, since we just installed `funnies`, that means that we now have access to it in our project. To use it, we pull it in just like we pulled in our functions in the node intro:

`file: index.js`
```javascript
let Funnies = require('funnies').Funnies;
let funnies = new Funnies();

setInterval(() => {
  console.log(funnies.message());
}, 2000);
```

We appropriately pulled in our `funnies` package and called it based on how the package's developer said we should. This program should now output a funny loading message every 2 seconds. It seems to be broken though, because none of it is funny.

## Other Node stuff

If you look at your package folder now, you may notice it looks a little different. Node has probably created a `node_modules` folder, which is where it stores all of the dependencies for your program, and their dependencies. When we installed `funnies`, we didn't just get `funnies` in our `node_modules` folder, we also got other modules, like `lodash`, `dom-helpers`, and `js-tokens`. This is because `funnies` has those modules set as dependencies for itself. NPM then went ahead and downloaded them and manages those for `funnies` so you don't have to. Say, for example, that the developer of `funnies` was using version 1.2.3 of `lodash` instead of version 2.0.0. Instead of having to download and install version 1.2.3 of `lodash` yourself, NPM has automatically downloaded it and will keep it at version 1.2.3, unless you update `funnies`, and the developer of `funnies` specifies in that update that he is now using `lodash` 2.0.0.

Node has also created a `package-lock.json` file. This package lock file is what keeps the information of what versions of packages you're using. Because of this file, you don't need to keep track of your `node_modules` folder. If you look at this file, you'd see a lot of json, grouped around the different packages:

```json
"funnies": {
  "version": "2.0.0",
  "resolved": "https://registry.npmjs.org/funnies-2.0.0",
  "integrity": "sha1-tz3mdRuPjLFXje8CKOPuwXuqIUE=",
  "requires": {
    "lodash": "^4.13.1",
    "react-addons-css-transition-group": "^15.1.0"
  }
}
```

This is where NPM tracks each version of each dependency and it's dependencies. The same goes for the "Sub-dependencies" required by this one, and so on until the last package no longer has any dependencies. Because of this `package-lock` file, you could delete any part of or your entire `node_modules` file, and recover it using NPM. If you were to do this, at first your app would not run. Running `npm install` will use your lock file to recreate the exact `node_modules` file that you had before to make your program run. Because of this, you should not add your `node_modules` file to your Git repository. Whenever you want to run your program on a new machine, you should run `npm install` instead of downloading the `node_modules` file. To make sure you don't add it to your git repository and waste time and server space, you can create a `.gitignore` file within your repository and add `node_modules/` to it. Git will completely ignore the folder from then on.
