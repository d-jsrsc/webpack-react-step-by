# webpack react step by

可以说是使用 webpack 一步步构建 esModule 项目。由于使用 create-react-app 习惯了，猛的让从头一步步用 webpack 构建起 React ，还真有些步骤要花些时间。_如果是小项目，我更推荐 Gulp 构建，虽然 Gulp 也忘了。_

初始化一个项目

```bash
mkdir webpack-step-by && cd webpack-step-by
npm init -y
```

然后安装 React webpack

```bash
npm i react react-dom
npm i webpack -D
```

然后建好一个 src 目录，新建 webpack.config.js，去 webpack 官网 copy 一个最小配置。这样的去 copy 就好了，也不要记。配置类的事情，记住了也没什么用，而且还容易记错。不如直接去 copy。

```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

根据 copy 的配置文件，在 src 新建一个 index.js 并写入

```javascript

import ReactDOM from "react-dom";

ReactDOM.render(<h1>Hello, world!</h1>, document.getElementById("root"));
```

在 package.json 的 script 加入，build 命令。

```json
...
  "scripts": {
    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
...
```

执行一下

```bash
❯ npm run build  
> webpack-react-step-by@1.0.0 build
> webpack

CLI for webpack must be installed.
  webpack-cli (https://github.com/webpack/webpack-cli)

We will use "npm" to install the CLI via "npm install -D webpack-cli".
Do you want to install 'webpack-cli' (yes/no): 
```
推荐安装 webpack-cli。敲入 yes，安装。

```bash

We will use "npm" to install the CLI via "npm install -D webpack-cli".
Do you want to install 'webpack-cli' (yes/no): yes
Installing 'webpack-cli' (running 'npm install -D webpack-cli')...

added 48 packages in 16s

3 packages are looking for funding
  run `npm fund` for details
assets by status 360 bytes [cached] 1 asset
./src/index.js 109 bytes [built] [code generated] [1 error]

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value.
Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/

ERROR in ./src/index.js 3:16
Module parse failed: Unexpected token (3:16)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
| import ReactDOM from "react-dom";
| 
> ReactDOM.render(<h1>Hello, world!</h1>, document.getElementById("root"));
| 

webpack 5.65.0 compiled with 1 error and 1 warning in 95 ms
```

事情并没有想象中顺利，安装过程中爆了一些错误。碰到错误修复错误就好了。**我们遇到的问题都不是问题，因为肯定有人已经遇到过了**。

首先是 WARNING 说在 configuration 立没有配置 mode。那配置上就好了，还是去 webpack 官网，搜一下 mode。

然后是需要 loader 去解析 react。这样的一 google 就出来了。**还是这种配置类的，能搜索的就搜索，不要去记在脑子里，如果有面试官非要一些配置类的东西，还要求你记住的话，那这个面试官就够水的。开卷有益。要留着精力去解决更重要的事情：比如写出更好维护的代码，写出更优雅的算法**

这样就是需要再安装 

```bash
npm install -D babel-loader @babel/core @babel/preset-env @babel/preset-react webpack 
```

然后再 build

```bash
❯ npm run build 

> webpack-react-step-by@1.0.0 build
> webpack

asset main.js 1010 KiB [emitted] (name: main)
runtime modules 274 bytes 1 module
modules by path ./node_modules/ 974 KiB
  modules by path ./node_modules/scheduler/ 26.3 KiB
    modules by path ./node_modules/scheduler/*.js 412 bytes 2 modules
    modules by path ./node_modules/scheduler/cjs/*.js 25.9 KiB
      ./node_modules/scheduler/cjs/scheduler.development.js 17.2 KiB [built] [code generated]
      ./node_modules/scheduler/cjs/scheduler-tracing.development.js 8.79 KiB [built] [code generated]
  modules by path ./node_modules/react-dom/ 875 KiB
    ./node_modules/react-dom/index.js 1.33 KiB [built] [code generated]
    ./node_modules/react-dom/cjs/react-dom.development.js 874 KiB [built] [code generated]
  modules by path ./node_modules/react/ 70.6 KiB
    ./node_modules/react/index.js 190 bytes [built] [code generated]
    ./node_modules/react/cjs/react.development.js 70.5 KiB [built] [code generated]
  ./node_modules/object-assign/index.js 2.06 KiB [built] [code generated]
./src/index.js 147 bytes [built] [code generated]
webpack 5.65.0 compiled successfully in 477 ms

```

似乎还缺一个 index.html，新建文件 public/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id='root'></div>
</body>
</html>
```

再去 webpack 官网。去搜：index.html，然后跳到对应的页面找一下：html-webpack-plugin

安装后配置下，webpack 官网配置跳到了 github，那就在 github 上抄一下配置，这里主要抄 template。

再 build 下，发现 dist 目录下有 index.html，而且 index.html 还引入了 main.js
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
<script defer src="main.js"></script></head>
<body>
    <div id='root'></div>
</body>
</html>
```

在浏览器里打开文件 index.html。发现内容并没有被渲染，这肯定是错了。打开控制台，看一下。

没有 React，那就再 import 下 React。

```javascript
// src/index.js
import React from "react";
import ReactDOM from "react-dom";

ReactDOM.render(<h1>Hello, world!</h1>, document.getElementById("root"));
```

再 build 下。再在浏览器看一下 Hello, world! 被渲染出来了。这样，一个 webpack 打包的 esModule 项目的初始化算是完了。

最后的 webpack.config.js

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist"),
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: { presets: ["@babel/env", "@babel/preset-react"] },
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "public/index.html",
    }),
  ],
};

```

当然还有 webpack-dev-server，css-loader 之类的其它的配置，这些 Google 下就能得到答案。所以如果没特殊要求和情况，还是建议使用 `create-react-app` 来起项目。**除非需要定制，又或者你认为自己的能力能比得上社区里顶尖的开发者，不然你配的肯定比不过 `create-react-app` 好！** 。









