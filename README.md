# mywebpackproject
example webpack project

Both naive.html and index.html display the same datatable, but index.html utilizes webpack to bundle up all of its resources, while naive.html manually manages the <link> and <script> tags for each dependent library.

## Follow Along ##
* Install [visual studio code](https://code.visualstudio.com/)
* Install [Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
* Open Visual Studio Code and open terminal with Ctrl+Shift+`
* Initialize an npm project with default settings:
```
mkdir myproject
cd myproject
npm init -y
```
* Install webpack as devDependency
```
npm install webpack webpack-cli --save-dev
```
* Install our needed libraries as dependencies
```
npm install jquery bootstrap popper.js --save
npm install datatables.net datatables.net-bs4 datatables.net-buttons datatables.net-buttons-bs4 --save
```
* Install necessary webpack loaders for handling css/scss, etc.
```
npm install sass-loader style-loader css-loader postcss-loader autoprefixer mini-css-extract-plugin --save-dev
```
* Create webpack.config.js at project root
```
const path = require('path');
module.exports = {
  entry: './src/app.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
};
```
* Create postcss.config.js as project root
```
module.exports = {
  plugins: {
    'autoprefixer': {}
  }
};
```
* Create a src/ folder at project root
* Add an app.js to src/
```
// app.js:
// bootstrap
import 'bootstrap';

// datatables
import 'datatables.net';
import 'datatables.net-buttons';
import 'datatables.net-buttons-bs4';
import 'datatables.net-buttons/js/dataTables.buttons.js';
import 'datatables.net-buttons/js/buttons.html5.js';
import 'datatables.net-buttons-bs4/js/buttons.bootstrap4.js';
import 'datatables.net-bs4/css/dataTables.bootstrap4.css';
import 'datatables.net-buttons-bs4/css/buttons.bootstrap4.css';
```
* Add webpack build script to package.json
```
  "scripts": {
    "build": "webpack"
  },
```
* When you try `npm run build`, you should see a dist/ folder get created with bundle.js inside.
* Create src/base.scss which will be the entry point for our custom styles. Also add _variables.scss and _custom-bootstrap.scss
```
//base.scss:
// import variable defs
@import 'variables';

// bootstrap customizations
@import 'custom-bootstrap';

// import bootstrap
@import '~bootstrap/scss/bootstrap.scss';

// import other components
// ...

// any other style defs
body {
    color: $primary;
}
```
* Import base.scss into app.js
```
// app.js:
// bootstrap
import 'bootstrap';

// datatables
import 'datatables.net';
import 'datatables.net-buttons';
import 'datatables.net-buttons-bs4';
import 'datatables.net-buttons/js/dataTables.buttons.js';
import 'datatables.net-buttons/js/buttons.html5.js';
import 'datatables.net-buttons-bs4/js/buttons.bootstrap4.js';
import 'datatables.net-bs4/css/dataTables.bootstrap4.css';
import 'datatables.net-buttons-bs4/css/buttons.bootstrap4.css';

// stylesheets
import './scss/base.scss';
```
* Update webpack.config.js by adding new [s]css loaders and jquery resolution:
```
const path = require('path');
const webpack = require('webpack');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
// https://medium.com/@karisabine/webpack-the-basics-2712a7ad640b
module.exports = {
    entry: './src/app.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            {
                test: require.resolve('jquery'),
                use: [
                    {
                        loader: 'expose-loader',
                        options: 'jQuery'
                    },
                    {
                        loader: 'expose-loader',
                        options: '$'
                    }
                ]
            },
            {
                test: /\.(sa|sc|c)ss$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            url: false,
                            sourceMap: true,
                            importLoaders: 2
                        }
                    },
                    'postcss-loader',
                    {
                        loader: 'sass-loader',
                        options: {
                            sourceMap: true
                        }
                    }
                ]
            }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: 'style.css'
        }),
        new webpack.ProvidePlugin({
            $: 'jquery',
            jQuery: 'jquery'
        })
    ]
};
```
* `npm run build` should now generate `dist/bundle.js` and `dist/style.css`
* Add an `index.html` that references `dist/bundle.js` and `dist/style.css` in the `<head>`, and includes a proper html `<table class='table'>`
* Initialize the datatable after the bundle has been referenced
```
$(function(){
  $('#myTable').dataTable({
    dom: 'Bflrtip',
    buttons: ['copy','csv']
  });
});
```
