npx create-react-app client

npm init
    enters

npm install express --save

create server.js
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
      app.get('\*', (req, res) => {
        res.sendFile(path.resolve(\__dirname, 'client', 'build', 'index.html'));
      });
    }

add in client/package.json below "scripts":{"cos tam"}
  "proxy": "http://localhost:5000/"

in client/scr/App.js change code to:
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

open two terminals: one in main directory and type 'node ./server.js' on second
go too /client and type 'npm start'

That build should work on localhost:3000

better way is too install nodemon "npm install nodemon" and start with 'nodemon ./server.js'

Befor adding git - delete .git from /client and make sure that .gitignore ignores node_module

Now add git:
    create new repo on github
    git init
    git add .
    git commit -m "first commit"
    git remote add origin 'name of repo'
    git push origin master
Must be something on Github before next step!

Now Heroku and Travis:
    follow this (step 3 and 4)[note that on github its in section ] https://medium.com/@felipeluizsoares/automatically-deploy-with-travis-ci-and-heroku-ddba1361647f
