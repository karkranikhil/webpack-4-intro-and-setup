### Webpack -
 It has become one of the most important tools for modern web development.
 Webpack is a module bundler which packs all modules with dependencies – js, styles, images, etc. into static assets .js, .css, .jpg , .png, etc.

-----------------
### Goals of Webpack
* Split the dependency tree into chunks loaded on demand
* Keep initial loading time low
* Every static asset should be able to be a module
* Ability to integrate 3rd-party libraries as modules
* Ability to customize nearly every part of the module bundler
* Suited for big projects

---------------
###  webpack 4 -
The main pain point in earlier versions of webpack was the configuration file.

#### key change in webpack 4 - 
* It doesn’t need a configuration file by default!
* There is no need to define the entry point: it will take ./src/index.js as the default!
* There is no need to define the output file: it will spit out the bundle in ./dist/main.js as the default!
* it introduces production and development mode.
------------------
### Webpack production and development modes

Usually project contains two configuration file one for dev and another for production

* dev config file - It is used for defining webpack dev server and other stuff
* Production config file - It is used for defining UglifyJsPlugin, Sourcemaps, minified, etc.

    ** Webpack introduces production and development mode.

* Define 'mode' option to development or production to enable default for this environment.

* <b>Production mode -</b>  enables all sorts of optimizations out of the box. Including minification, scope hoisting, tree-shaking and more. 
* <b>Development mode -</b>  It provides an un-minified code
-----------------------
### Transpiling - 
Older browser don't know how to deal with ES6 and above. The transformation of es6 into vanila js(understandable by older browsers).

* Webpack has loaders that helps in transformation
* <b>babel-loader </b>is the webpack loader for transpiling ES6 and above, down to ES5.
-------------------
### Steps to setup

1) initialize a package.json

        npm init -y
2) install a webpack 4

        npm i webpack --save-dev

3)  Install webpack cli - It is a CLI tool for providing a flexible set of commands for developers to increase speed when setting up a custom webpack project. As of webpack v4, webpack is not expecting a configuration file but often, developers want to create a more custom webpack configuration based on their use-cases and needs.

        npm i webpack-cli --save-dev

4)  open up package.json and add a build script:      

        "scripts": {
            "build": "webpack"
        }

5) Create src/index.js

        console.log("hello webpack 4")        

6)  Run below command and it generate the dist folder

            npm run build

7) Define mode to script in package.json

        "scripts": {
            "dev": "webpack --mode development",
            "build": "webpack --mode production"
        }

8) now run below command and it run in production mode and you will see ./dist/main.js is a minified bundle.

        npm run dev

9) setup loader for transpiling

        npm i @babel/core babel-loader @babel/preset-env --save-dev

10) create config file for babel names as .babelrc and add below code

        {
            "presets": [
                "@babel/preset-env"
            ]
        }

 11) To use loader we need to create webpack config file  webpack.config.js    

            module.exports = {
                module: {
                    rules: [
                    {
                        test: /\.js$/,
                        exclude: /node_modules/,
                        use: {
                        loader: "babel-loader"
                        }
                    }
                    ]
                }
            };

12) add some piece of ES6 code to src/index.js

            const greetings=()=>{
                return "Hello world"
            }
            console.log(greetings());

13) run below command and you will see the transpiled code in ./dist/main.js file

        npm run build

14) To use babel-loader without a configuration file add below script to package.json

        "scripts": {
            "dev": "webpack --mode development --module-bind js=babel-loader",
            "build": "webpack --mode production --module-bind js=babel-loader"
        }    

 #### Lets setup webpack 4 in react

 1) install react and babel-preset for react

               npm i react react-dom --save-dev
               npm i @babel/preset-react --save-dev

 2) set preset in .babelrc        

            {
            "presets": ["@babel/preset-env", "@babel/preset-react"]
            } 

 3) Lets configure loader in webpack.config.js to read .jsx Add test as below

            test: /\.(js|jsx)$/,

 4) create a react component in ./src/App.js


            import React from "react";
import ReactDOM from "react-dom";
            
            const App = () => {
                return (
                    <div>
                    <p>React here!</p>
                    </div>
                );
            };
            export default App;
            ReactDOM.render(<App />,document.getElementById("app"));
            

5) import the component in index.js

        import App from "./App";
    
6) create ./src/index.html

        <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="utf-8">
                <title>webpack-4-intro-and-setup</title>
            </head>
            <body>
                <div id="app">
                </div>
            </body>
            </html>
7)Webpack need to additional things

* <b>html-webpack-plugin</b> - This is a webpack plugin that simplifies creation of HTML files to serve your webpack bundles
* <b>html-loader</b> - It exports HTML as string and HTML is minimized when the compiler demands.

install these two HTML webpack plugin

        npm i html-webpack-plugin html-loader --save-dev

8) update the webpack configuration as below
    
    * add one more rule for html-loader

            {
                test: /\.html$/,
                use: [
                {
                    loader: "html-loader",
                    options: { minimize: true }
                }
                ]
            }
    * add html webpack plugin to config file as mentioned below

            plugins: [
                new HtmlWebPackPlugin({
                template: "./src/index.html",
                filename: "./index.html"
                })
            ]
and import the plugin 

        const HtmlWebPackPlugin = require("html-webpack-plugin");

9) Run below command you will see dist/index.html

        npm run build

10) open index.html in browser and you will see "React with webpack 4 is here!" and in console of browser you will what ever we have defined in index.js

11) <b>Extracting css to file </b>webpack doesn’t know to extract CSS to a file.
* we will use loader called css-loader
* we will use the plugin called mini-css-extract-plugin 

* use the below command to install the loader and plugin

        npm i mini-css-extract-plugin css-loader --save-dev

12) Create ./src/main.css file

        body {
            background:black;
            color:red;
            font-size:30px;
        }

13) configure css loader in webpack.config.js by adding below code in rules

        {
            test: /\.css$/,
            use: [MiniCssExtractPlugin.loader, "css-loader"]
        }

14) configure css plugin in webpack.config.js by adding below code under plugin array

        new MiniCssExtractPlugin({
            filename: "[name].css",
            chunkFilename: "[id].css"
        })
and import the plugin 

        const MiniCssExtractPlugin = require("mini-css-extract-plugin");

15) Import css file in index.js

        import style from "./main.css";

16) run the below commmand

        npm run build     
17) open index.html in browser and you will see "React with webpack 4 is here!" and in console of browser you will what ever we have defined in index.js and css got applied with background color black and color red.
----------


### Thanks

* https://webpack.js.org
* https://www.valentinog.com/blog/webpack-tutorial/
