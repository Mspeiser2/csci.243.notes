## NPM and Nodemon

### Using NPM scripts

You can create custom scripts for Node to make several things much easier in development. When using the `npm init` command and going through the initializer, you could specify scripts to run for your program. These scripts can be used for whatever you can do with commands. To create one, you can either specify it during the initializer, or edit your `package.json`:

```json
{
 "name": "csci243",
 "version": "0.0.1",
 "description": "This app loads indefinitely and puts out funnies while doing it.",
 "main:": "index.js",
 "scripts": {
   "test": "echo \"Error: no test specified\" && exit 1",
   "clean-logs": "rm-rf logs/*"
 },
 "author": "Speiserm",
 "license": "ISC"
}
```

As you can see, in the `scripts` item we specified a new script called `clean-logs` which runs the command `rm-rf logs/`. This is functionally the same as running `rm-rf logs/` from the command line, which would delete the directory `logs` and any subdirectories or files within it. To run these commands, you have Node execute it as follows:

`npm run <script>`

Or in our case:

`npm run clean-logs`

A task as simple as deleting a directory isn't something you really need to add as a script, but more complicated tasks that need to be done repeatedly are prime targets to be turned into scripts. Adding script commands to your program can make it much easier to set variables, testing, and to get your program set up to run.

## Special scripts

There is a special script called `start`. This is a script which you want to run, you guessed it, at the start of your program, or to start the program itself. This is typically used to start your program because NPM allows the command `npm start` to execute this script, whereas you could not do `npm clean-logs` because that is not a specific NPM command. You could also use this to install certain dependencies, such as our next favorite tool, `nodemon`.

## A new challenger approaches

`Nodemon` can be installed and easily installed and accessed in the same exact way we install any other package. However, we really don't want `nodemon` in our production version of our program. So, we only want to install it on our development version. To do this, we use a modified version of the `npm install` command: `npm install --save-dev`. Running `npm install --save-dev nodemon` creates a new, separate dependency area in our `package.json` file: `devDependencies`. The new `devDependencies` item in our json looks like this:

```json
"devDependencies": {
  "nodemon": "^1.18.9"
}
```

Nodemon is installed, but kept separate from our other dependencies thanks to NPM. This way, if we are running a development version of our program, NPM knows to install `nodemon`. Otherwise, it leaves it out.

## Using Nodemon

If we go back to our scripts, we can now take advantage of `nodemon`. We can create a special `start` script which starts our program with `nodemon` instead of regular `node`. To do this, we add the script like this:

```json
{
 "name": "csci243",
 "version": "0.0.1",
 "description": "This app loads indefinitely and puts out funnies while doing it.",
 "main:": "index.js",
 "scripts": {
   "test": "echo \"Error: no test specified\" && exit 1",
   "clean-logs": "rm-rf logs/*",
   "start": "nodemon index.js"
 },
 "author": "Speiserm",
 "license": "ISC"
}
```

Our `start` script is basically running `node index.js` for us, but instead of running `node`, it's running `nodemon` for us. This makes development really easy for us because we can now run `npm start` and `nodemon` will run our program.

## Developing with Nodemon

The nice thing about Nodemon (and the reason we want to use it) is because it watches our files for us, and restarts the program whenever they're changed. This means that you can leave your program running while you're developing, and continue to edit and fix your program and see instant results.
