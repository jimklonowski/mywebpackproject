# mywebpackproject
example webpack project

Both naive.html and index.html display the same datatable, but index.html utilizes webpack to bundle up all of its resources.

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
