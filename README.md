# Creating a React App
There are a couple of hurdles to starting a React app. The first is that node can't process all of the syntax (such as import/export and jsx). The second is that you'll either need to build your files or serve them somehow during development for your app to work. we can handle these issues with Babel and Webpack.


### Setup
To get started, create a new directory for your React app. Then, initialize your project with `npm init` and `git init`. 
In your new project folder, create the following structure
```js
.
+-- public
+-- src
```
Our `public` directory will handle any static assets. It includes `index.html`

~~~~html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <title>React Starter</title>
</head>

<body>
  <div id="root"></div>
  <noscript>
    You need to enable JavaScript to run this app.
  </noscript>
  <script src="../dist/bundle.js"></script>
</body>

</html>
~~~~

### Babel
`npm install --save-dev @babel/core@7.1.0 @babel/cli@7.1.0 @babel/preset-env@7.1.0 @babel/preset-react@7.0.0`

In the project root, create a file called `.babelrc`
```json
{
  "presets": ["@babel/env", "@babel/preset-react"]
}
```

### Web pack
`npm install --save-dev webpack@4.19.1 webpack-cli@3.1.1 webpack-dev-server@3.1.8 style-loader@0.23.0 css-loader@1.0.0 babel-loader@8.0.2`
Create a new file at the root of the project called `webpack.config.js`. This file exports an object with webpack’s configuration.

```js
const path = require("path");
const webpack = require("webpack");

module.exports = {
  entry: "./src/index.js",
  mode: "development",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /(node_modules|bower_components)/,
        loader: "babel-loader",
        options: { presets: ["@babel/env"] }
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  resolve: { extensions: ["*", ".js", ".jsx"] },
  output: {
    path: path.resolve(__dirname, "dist/"),
    publicPath: "/dist/",
    filename: "bundle.js"
  },
  devServer: {
    contentBase: path.join(__dirname, "public/"),
    port: 3000,
    publicPath: "http://localhost:3000/dist/",
    hotOnly: true
  },
  plugins: [new webpack.HotModuleReplacementPlugin()]
};
```

### React
`npm install --save react@16.5.2 react-dom@16.5.2 `

Create a file called index.js in your src directory.

```js
import React from "react";
import ReactDOM from "react-dom";
import App from "./App.js";
ReactDOM.render(<App />, document.getElementById("root"));
```

Now, create another file in src called App.js
```js
import React, { Component} from "react";
import "./App.css";

class App extends Component{
  render(){
    return(
      <div className="App">
        <h1> Hello, World! </h1>
      </div>
    );
  }
}

export default App;
```

Let’s add a really simple stylsheet to the src directory. Create App.css in src directory.
```css
.App {
  margin: 1rem;
  font-family: Arial, Helvetica, sans-serif;
}
```

project structure should look like the following.
```js
.
+-- public
| +-- index.html
+-- src
| +-- App.css
| +-- App.js
| +-- index.js
+-- .babelrc
+-- .gitignore
+-- package-lock.json
+-- package.json
+-- webpack.config.js
```

## React hot loader
`npm install --save react-hot-loader@4.3.11`

Changes to the code should update the client immediately after saving.
Now, import react-hot-loader in App.js
```js
import React, { Component} from "react";
import {hot} from "react-hot-loader";
import "./App.css";

class App extends Component{
  render(){
    return(
      <div className="App">
        <h1> Hello, World! </h1>
      </div>
    );
  }
}

export default hot(module)(App);
```

## Scripts
Add these scripts in package.json
```json
"scripts": {
    "start": "webpack-dev-server --mode development",
    "build": "webpack --mode production",
    "test": "echo \"Error: no test specified\" && exit 1"
},
```