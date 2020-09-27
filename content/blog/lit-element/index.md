---
title: Setting Up Lit Element Project!
date: "2020-09-25T12:11:44.970Z"
description: Lit Element is a simple library for creating fast, lightweight web components that work in any web page with any framework. In this article we are going to setup a Lit Element project using webpack.
---

**Lit Element** is a simple library for creating fast, lightweight web components that work in any web page with any framework.

LitElement uses _lit-html_ to render into [Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) , and adds API to manage properties and attributes. Properties are observed by default, and elements update asynchronously when their properties change.

##### Getting Started

Create a Project Folder and Open it in VS code.

Open console in project directory.

Type `npm init`

Enter Project name and press enter enter enter ...

This will create a _Package.json_ file.

Now We need to install some npm packages.

Type `npm install lit-element`

We will use webpack to bundle our project.

Type `npm install webpack webpack-cli html-webpack-plugin clean-webpack-plugin webpack-dev-server --save-dev`

Now we will start creating app components

Create _src_ folder and create _App.js_ file inside it.
Copy below code in _App.js_ file

```Javascript
import { LitElement, html, css } from "lit-element";
class App extends LitElement {
  static get styles() {
    return [css``];
  }
  render() {
    return html`
      <div>Loading Application Component Successfully</div>
    `;
  }
  constructor() {
    super();
  }
}

customElements.define("app-container", App);
```

Now Create another file _header.js_ and copy below code

```Javascript
import { LitElement, css, html } from "lit-element";
class Header extends LitElement {
  static get styles() {
    return [css``];
  }
  render() {
    return html` <div>This is a header in shadow dom</div>`;
  }
  static get properties() {
    return {
      eg: {
        type: String,
      },
    };
  }
  constructor() {
    super();
  }
}

customElements.define("app-header", Header);
```

Import _app-header_ into _App.js_

Type `import ./header.js`

Now inject header component App

Type `<app-header></app-header>` in render function

```Javascript
  render() {
    return html`
      <app-header></app-header>
      <div>Loading Successfully</div>
    `;
  }

```

Now create two files in base directory of your project

_webpack.dev.js_ , _webpack.prod.js_ and copy below code respectively

```Javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
  mode: "development",
  entry: {
    index: "./src/App.js",
  },
  devtool: "inline-source-map",
  plugins: [
    new CleanWebpackPlugin({ cleanStaleWebpackAssets: false }),
    new HtmlWebpackPlugin({
      template: "./index.html",
    }),
  ],
};
```

```JavaScript
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  mode: "production",
  entry: {
    app: "./src/App.js",
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: "./index.html",
    }),
  ],
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
};
```

Add following scripts to _package.json_ file

`"start": "webpack-dev-server --hot --inline --config webpack.dev.js",`
`"build": "webpack --config webpack.prod.js",`

Now type `npm start` in console.

This will start dev server and by default will run on port 8080

Now lets add some css to our project

Create _App.styles.js_ file in src folder and copy following code

```Javascript
import { css } from "lit-element";
export const AppStyles = css`
  .app-container {
    border: solid 2px black;
  }
  h1 {
    text-align: center;
  }
  .app-body {
    text-align: justify;
  }
`;
```

Import this file to _App.js_
`import { AppStyles } from "./App.styles";`
and add this to styles array

```JavaScript
static get styles() {
    return [css``, AppStyles];
  }
```

Add made some changes to html of app js

```Javascript
render() {
    return html`
      <div class="app-container">
        <app-header></app-header>
        <h1 class="app-title">App title</h1>
        <p class="app-body">
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Magnam
          numquam in ipsa optio cumque architecto quisquam, at repellendus
          facere iusto consequatur animi inventore possimus quas, sint sunt
          quidem maxime libero?
        </p>
      </div>
    `;
  }
```

Now if you see in browser you will notice that styles of `App` component will not effect `app-header` component.
This is because `app-header` is attached to Shadow DOM of `App` and thus styles will not pass to it.

This is great for creating reusable independent components which we can directly plug to any part of our application without worrying about styles conflicting with other components.

Finally we have a working lit element project.

In next part we will restructure our project and add routing on it.

<a href="https://github.com/iamvishal345/lit-element-initial_setup" target="_blank">Git Hub Project</a>
