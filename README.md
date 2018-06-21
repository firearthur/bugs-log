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
