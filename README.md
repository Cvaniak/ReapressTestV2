https://reapresstestv2.herokuapp.com/

***

`npx create-react-app client`

`npm init`
  enters

`npm install express --save`

create `server.js`
```js
    const express = require('express');
    const app = express();
    const port = process.env.PORT || 5000;

    // console.log that your server is up and running
    app.listen(port, () => console.log(`Listening on port ${port}`));

    // create a GET route
    app.get('/express_backend', (req, res) => {
    res.send({ express: 'YOUR EXPRESS BACKEND IS CONNECTED TO REACT' });
    });

    if (process.env.NODE_ENV === 'production') {
      // Exprees will serve up production assets
      app.use(express.static('client/build'));

      // Express serve up index.html file if it doesn't recognize route
      const path = require('path');
      app.get('*', (req, res) => {
        res.sendFile(path.resolve(__dirname, 'client', 'build', 'index.html'));
      });
    }
```
add in `client/package.json` below `"scripts":{"cos tam"}`
```js
  "proxy": "http://localhost:5000/"
```

in `client/scr/App.js` change code to:
``` js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
    state = {
        data: null
      };

      componentDidMount() {
          // Call our fetch function below once the component mounts
        this.callBackendAPI()
          .then(res => this.setState({ data: res.express }))
          .catch(err => console.log(err));
      }
        // Fetches our GET route from the Express server. (Note the route we are fetching matches the GET route from server.js
      callBackendAPI = async () => {
        const response = await fetch('/express_backend');
        const body = await response.json();

        if (response.status !== 200) {
          throw Error(body.message)
        }
        return body;
      };

      render() {
        return (
          <div className="App">
            <header className="App-header">
              <img src={logo} className="App-logo" alt="logo" />
              <h1 className="App-title">Welcome to React</h1>
            <p className="App-intro">{this.state.data}</p>
          </header>
          </div>
        );
      }
    }

    export default App;
```
open two terminals: one in main directory and type `node ./server.js` on second
go too /client and type `npm start`

That build should work on localhost:3000

better way is too install nodemon `npm install nodemon` and start with `nodemon ./server.js`

***

Befor adding git - delete .git from /client and make sure that .gitignore ignores node_module

THE MOST IMPORTANT in main directory `package.json`
``` json
  "scripts": {
    "test": "echo \"Error: no test specified",
    "client-install": "npm install --prefix client",
    "start": "node server.js",
    "server": "nodemon server.js",
    "client": "npm start --prefix client",
    "dev": "concurrently \"npm run server\" \"npm run client\"",
    "heroku-postbuild": "NPM_CONFIG_PRODUCTION=false npm install --prefix client && npm run build --prefix client"
  },
```


Now add git:
    create new repo on github
``` bash
    git init
    git add .
    git commit -m "first commit"
    git remote add origin 'name of repo'
    git push origin master
```
Must be something on Github before next step!

Now Heroku and Travis:
    Heroku and Travis connection with Github:
    follow this (step 3 and 4)[note that on github its in section webhooks | if there is no your repo in travis - refreash in left upper corner] https://medium.com/@felipeluizsoares/automatically-deploy-with-travis-ci-and-heroku-ddba1361647f

After that login to Heroku:
```bash
    heroku login
    heroku git:remote -a "herokuAppName"
```

And add Travis file:
  in main directory create .travis.yml
  with:
``` yml
    language: node_js
    node_js:
    - stable
    cache:
      directories:
      - node_modules
    deploy:
      provider: heroku
      api_key:
        secure: "EncryptedAPIKey"
        app: herokuNameAPp
      on:
        repo: NickFromGit/RepoName
    script:
    - echo "skipping tests"
```
in `EncryptedAPIKey` change it for Encrypted APIKey (also change app and repo) :p  
for that in command line `travis encrypt $(heroku auth:token)`  
there may be some errors but should work with them

***
If any error on heroku use `heroku logs --tail`  
travis setup heroku --org -r <repo_slug>, if your project is running in travis-ci.org  
or  
travis setup heroku --pro -r <repo_slug>, if your project is running in travis-ci.com  
