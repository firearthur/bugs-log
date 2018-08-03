# Bugs Log and Solutions

This is a log of bugs that I've encountered and the solutions that I found on the internet or came up with.

The log might get long. I'll try to keep it organized but the best way to find what you're looking for is CTRL + F/CMND + F and looking up keywords that you think might be relevant.

Also, Please feel free to point out/correct any typos or if you have any solutions to add to a bug please do.


## 1- Angular(2+) routes not working after refresh:
**Bug Description**: In an Angular/Express app, you serve your static and then user clicks on a link wich is handled by the Angular router. Now if you go the address bar and refresh. You get a 404 error.

**Bug Cause**: The express server tries to serve up whatever Angular router was routing to. For example, if your anchor tag routes to `/history`, your express is going to try to look for `BASE URL/history` and this won't be handled by the Angular router.

**Bug Fix**: 2 second version, use a wildcard to reroute to your index.html like so:
```
app.get('/*', (req, res) => {
  res.sendFile(path.join(__dirname, '../client/dist/index.html'));
});
```
This will magically make it work (at least it did for me).

Full version, try looking at this Stack Overflow [post](https://stackoverflow.com/questions/31415052/angular-2-0-router-not-working-on-reloading-the-browser).

## 2- Mysql sequelize giving "Client does not support authentication protocol":

**Bug Description**: I think Ubuntu users trying to use sequelize to connect their mysql db have this error `Client does not support authentication protocol` and can't connect their db.

**Bug Cause**: It's because newer versions of mysql use different auth system.

**Bug Fix**:
* Run mysql client: `sudo mysql -u root -p`
* Enter password
* Use the mysql db: `use mysql;`
* Update the auth to the old auth system:
```
update user set authentication_string=password(''), plugin='mysql_native_password' where user='root';
```
* make sure you restart the mysql service: `sudo service mysql restart`

Full version, try looking at this Stack Overflow [post](http://stackoverflow.com/a/36234358/1431224).


## 3- Node server won't start and give `port error listen EADDRINUSE [portNumber]`:

**Bug Description**: Your app was working fine and all of a suddent you can't run it anymore. Sometimes, if you wait a couple of minutes it may run but then you try again it crashes.

**Bug Cause**: I think this happens because the port you're trying to run your server on is already occupied. Usually it happes when you accidently close a terminal window without shutting down the server process properly and it keeps the server running in the background.

**Bug Fix**:
* Fast solution: change the port number in your Node/Express.
* Not as fast solution. Restart your computer.


## 4- Node server won't start and give `port error listen EADDRINUSE [portNumber]`:

**Bug Description**: Your app was working fine and all of a suddent you can't run it anymore. Sometimes, if you wait a couple of minutes it may run but then you try again it crashes.

**Bug Cause**: I think this happens because the port you're trying to run your server on is already occupied. Usually it happes when you accidently close a terminal window without shutting down the server process properly and it keeps the server running in the background.

**Bug Fix**:
* Fast solution: change the port number in your Node/Express.
* Not as fast solution. Restart your computer.


## 4- MongoDB gives `connection error: MongoNetworkError: failed to connect to server`:

**Bug Description**: You try to run your app and it gives you an error with MongoDB connection.

**Bug Cause**: It's because the mongod service is not running.

**Bug Fix**:
* Run `sudo systemctl status mongod` to check if it's running or not.
* Run  `sudo systemctl start mongod` to run it.


## 5- How to append a string to an img src in Angular (2+):

**Bug Description**: Not much of a bug more of an interesting syntax that's hard to remember (at leaset for me).

**Bug Cause**: N/A.

**Bug Fix**:
* This way have worked fine for me with Angular 6: `<img [src] = "data.imagePath"+".jpg">`
* There is another way that's mentioned in this post below.

Credits go to this Stack Overflow [post](https://stackoverflow.com/questions/46791107/appending-string-to-img-src-in-angular-4).

## 6- How to create an SSH key to push/pull to Github without inputting credentials:

**Bug Description**: Not much of a bug more of a process that could be tricky if it's your first time. This is especially handy if you have two factor verification (2FA) because there's no way to input the verification code through the command line when using HTTPS to pull and push.

**Bug Cause**: N/A.

**Bug Fix**:
* Here is the official Github help [tutorial](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) on how to do this.

**High-level steps**:
* First you gotta create an SSH key on your machine or use an exsiting one.
* You'll noticed the key comes as a public (one that ends with .pub) private pair.
* cat or nano into the public key and copy all of its content.
* You'll need to paste that into your Github SSH textarea as the tutorial above shows you.
* You're pretty much set now. However you wanna make sure you're pushing and pulling through SSH. Check out this Stack Overflow [post](https://stackoverflow.com/questions/14762034/push-to-github-without-password-using-ssh-key) which shows you how to do it.



## 7- How to build full-stack app with ES6 Node for back-end and create-react-app with CSS-Moduels in the front-end and using `.env` variables:

**Bug Description**: Exactly what it sounds like. These are steps to get a full-stack app running with some convenient set-up.

**Bug Cause**: N/A.

**Bug Fix**:

### For using CSS-Modules do the following. Otherwise, skip this step: (Why would you not use it?!)
* Create a new app with CRA (create-react-app).
* Eject using the command `yarn eject`.
* Find the file `webpack.config.dev.js` and replace this code:
```
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
  },
},

```
* With this code: 
```
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    modules: true,
    localIdentName: "[name]__[local]___[hash:base64:5]"  
  },
},
```

* Next, find this file `webpack.config.prod.js` and change the following code:
```
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    minimize: true,
    sourceMap: true,
   },
},
```
* With the following:

```
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    modules: true,
    minimize: true,
    sourceMap: true,
   },
},
```
* This makes use of CSS-Modules on both, the production build and the development one.
* If you'd like to use a nicer (which I do) syntax than your old `style.className` you can take a look at the final section of this [post](https://blog.pusher.com/css-modules-react/) as it thaught me how to do this.

### For using ES6 in Node do the following: (take this part with a grain of salt)
* Create a `.bablerc` file in your root directory (I think this should be adjacent to your package.json. Not too sure though) and paste the following code in there:
```
{
  "presets": ["env", "es2015"]
}
```
* This adds the configs to your directory. Next we need to have a build script.
* Add this script to your `package.json` file: `"build-server": "rm -rf dist/ && babel ./ --out-dir dist/ --ignore readme.md,yarn.lock,./node_modules,./.babelrc,./package.json,./npm-debug.log --copy-files",`.
* If you run `yarn build-server` or `npm run build-server` you should see a `dist` directory with the transpiled code in it.
* For having hot reloading with nodemon when in development add the following command to your scripts at `package.json` file: `"server": "nodemon --exec yarn babel-node -- ./index.js",`.
* **Note** You would need to change the path to your server file after `--exec yarn babel-node --`. Mine happens to be `./index.js`. Also, if you don't want to use `yarn` you can replace it with `npm run`.
* This should allow you to run nodemon without having issues with ES6.

**High-level steps**:
* 
