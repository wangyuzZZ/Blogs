# node+typescript 搭建项目

## 配置package.json文件

```json
{ 
 "name": "start", 
 "version": "1.0.0", 
 "description": "", 
 "main": "index.js", 
 "scripts": { 
 "test": "echo "Error: no test specified" && exit 1" 
 }, 
 "author": "", 
 "license": "ISC" 
} 
```

>因为要在项目中使用Webpack，所以首先得创建一个package.json文件，我们可以使用npm init来生成

## 开始

我们在项目的根目录创建一个src目录，添加一个main.js和information-logger.js文件，我们先使用Javascript来创建：

```js
// src/information-logger.js 
const os = require('os');
const { name, version } = require('../package.json');
module.exports = {
    logApplicationInformation: () =>
        console.log({
            application: {
                name,
                version,
            },
        }),
    logSystemInformation: () =>
        console.log({
            system: {
                platform: process.platform,
                cpus: os.cpus().length,
            },
        }),
};
```

```javascript
// src/main.js 
const informationLogger = require('./information-logger');
informationLogger.logApplicationInformation();
informationLogger.logSystemInformation(); 
```

>可以运行node main.js（先到src目录下）打印信息

## Webpack

配置Webpack的依赖项，带上 -d，因为我们只在开发环境下

```shell
npm i -D webpack webpack-cli 
```

我们没用到webpack-dev-server,安装完成后我们创建webpack.config.js的配置文件

```js
'use strict'; 
module.exports = (env = {}) => { 
     const config = { 
         entry: ['./src/main.js'], 
         mode: env.development ? 'development' : 'production', 
         target: 'node', 
         devtool: env.development ? 'cheap-eval-source-map' : false, 
     }; 
    return config; 
}; 
```

最开始我们没那么多的配置需要配置。我们要使用它，先改一下package.json

```json
"scripts":{  
     "start":"webpack --progress --env.development"， 
     "start:prod"："webpack --progress"  
 }， 
```

然后我们就可以通过任一命令（npm start）来构建应用程序，它会创建一个dist/main.js，我们可也使用webpack.config.js指定输出不同的名称，现在的目录结构应该如下

```
├── dist
│	└── main.js
├── node_modules
├── src
│   ├── information-logger.js
│   └── main.js
├── package-lock.json
├── package.json
└── webpack.config.js
```

## nodemon

因为无法使用webpack-dev-server，所以可以使用nodemon来解决，它可以在我们开发期间重新启动Node.js的应用程序，一样我们先来安装，依然需要 -d

```shell
npm i -D nodemon-webpack-plugin 
```

然后重新配置webpack.config.js

```js
// webpack.config.js 
'use strict'; 
const NodemonPlugin = require('nodemon-webpack-plugin'); 
module.exports = (env = {}) => { 
         const config = { 
         entry: ['./src/main.js'], 
         mode: env.development ? 'development' : 'production', 
         target: 'node', 
         devtool: env.development ? 'cheap-eval-source-map' : false,  
         resolve: { // tells Webpack what files to watch. 
         modules: ['node_modules', 'src', 'package.json'], 
         },  
         plugins: [] // required for config.plugins.push(...); 
         }; 
    if (env.nodemon) { 
         config.watch = true; 
         config.plugins.push(new NodemonPlugin()); 
     } 
    return config; 
}; 
```

Webpack 监视配置将在我们更改文件时重建应用程序,nodemon在我们构建完成重新启动应用程序，需要重新配置下package.json

```js
"scripts": { 
     "start": "webpack --progress --env.development --env.nodemon", 
     "start:prod": "webpack --progress --env.nodemon", 
     "build": "webpack --progress --env.development", 
     "build:prod": "webpack --progress", 
     "build:ci": "webpack" 
 }, 
```

## 使用TypeScript

```shell
npm i -D typescript ts-loader @types/node@^10.0.0 
```

### ts-loader（ts加载器）

因为要用ts-loader Webpack插件来编译我们的TypeScript，所以得让Webpack知道我们是使用了ts-loader插件来处理TypeScript文件的，更新之前的webpack.config.js

```js
// webpack.config.js 
 'use strict'; 
const NodemonPlugin = require('nodemon-webpack-plugin'); 
module.exports = (env = {}) => { 
     const config = { 
     entry: ['./src/main.ts'], 
     mode: env.development ? 'development' : 'production', 
     target: 'node', 
     devtool: env.development ? 'cheap-eval-source-map' : false, 
     resolve: { 
     // Tells Webpack what files to watch  
     extensions: ['.ts', '.js'], 
     modules: ['node_modules', 'src', 'package.json'], 
     }, 
     module: { 
     rules: [ 
     { 
     test: /.ts$/, 
     use: 'ts-loader', 
     }, 
     ], 
     }, 
     plugins: [], // Required for config.plugins.push(...); 
     }; 
    if (env.nodemon) { 
     config.watch = true; 
     config.plugins.push(new NodemonPlugin()); 
     } 
    return config; 
}; 
```

### tsconfig.json

tsc -init 生成tsconfig.json

```json
// tsconfig.json 
{ 
 "compilerOptions": { 
 "target": "esnext", 
 "module": "esnext", 
 "moduleResolution": "node", 
 "lib": ["dom", "es2018"], 
 "allowSyntheticDefaultImports": true, 
 "noImplicitAny": true, 
 "noUnusedLocals": true, 
 "removeComments": true,  
 "resolveJsonModule": true, 
 "strict": true, 
 "typeRoots": ["node_modules/@types"] 
 }, 
 "exclude": ["node_modules"], 
 "include": ["src/**/*.ts"] 
} 
```

然后更改下之前创建的js文件扩展名

```js
// information-logger.ts 
import os from 'os'; 
import { name, version } from '../package.json'; 
export class InformationLogger { 
 static logApplicationInformation(): void { 
 console.log({ 
 application: { 
 name, 
 version, 
 }, 
 }); 
 } 
static logSystemInformation(): void { 
 console.log({ 
 system: { 
 platform: process.platform, 
 cpus: os.cpus().length, 
 }, 
 }); 
 } 
} 
```

```js
// main.ts 
import { InformationLogger } from './information-logger'; 
InformationLogger.logApplicationInformation(); 
InformationLogger.logSystemInformation(); 
```

目录结构应该是这样的

```
├── dist
│	└── main.js
├── node_modules
├── src
│   ├── information-logger.js
│   └── main.js
├── package-lock.json
├── package.json
├── webpack.config.js
└── tsconfig.json
```

### 运行/打包

```shell
npm start //运行
npm run-script build //打包
```

