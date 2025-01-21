---
title: 使用Typescript, Node, React, Webpack搭建全栈项目中遇到的问题
date: 2025-01-02 17:33:40
categories: 学习笔记
tags: 
- TypeScript
- Node.js
- Express.js
- MongoDB
- React.js
- Webpack
- Redux
- Storybook
---

这篇笔记录了我在使用Node.js, TypeScript 和 React.js 搭建全栈项目所遇到的一系列问题和我的解决方案. 很多方案目的只是解决面临的问题, 很可能不是最优策略, 仅供参考.

<!-- more -->
<!-- toc -->

## 该项目技术栈

该项目使用以下技术栈进行搭建: 

- Typescript
- Node.js
- Express.js
- MongoDB
- React.js
- Redux:  用于管理React状态
- Storybook: 用于协同, 隔离和可视化前端组件开发和测试, 以及自动生成文档



## 配置后端Typescript, 启用ES6模块, 以及模块导入

在`tsconfig.json`中启用如下设置: 
```json
{
    "compilerOptions": {
    
    "target": "ESNext",
    "module": "nodenext",                             	
    "rootDir": "./src",
    "moduleResolution": "nodenext",                     /* 启用严格导入后缀检查 */
    "allowImportingTsExtensions": true,          		/* 允许导入TS模块 */       
    "rewriteRelativeImportExtensions": true,            /* 在编译后, 将ts后缀编译为js后缀 */
    "allowArbitraryExtensions": true,                   /* 声明后可以导入任意后缀文件 */
    "outDir": "./dist",                                 
    
    "verbatimModuleSyntax": true,                        /* 不对 import 和 export 语法（未标记为仅类型的）进行任何转换或省略，确保在输出文件中保留这些语法，并根据 module 配置的格式生成。*/
    "esModuleInterop": true,                             /* 增加额外的 JavaScript 代码来更好地支持 CommonJS 模块导入 */
    "forceConsistentCasingInFileNames": true,            /* 强制文件名的大小写一致。例如，如果文件实际名称是 Example.ts，则不能以 example.ts 访问它。*/
    "strict": true,                                      
    "skipLibCheck": true                                
  }
}
```

并且在导入模块时使用完整后缀名: 
```typescript
import ExpenseModel from '../model/expense.model.ts'
import type { Expense } from '../interface/expense.ts'
```

> 我在后端项目中选择了这么配置, 但是由于前端使用webpack打包而后端使用tsc 打包, 两者在设置上有一些不同, 导致模块导入的写法在前后端不一致, 应该可以有一个更好的解决方法



## Node连接不了云端的Mongo Atlas数据库, 显示无法找到primary cluster

在链接Mongo Atlas 时, 即使使用了正确的链接串也无法使用node链接, 但是无论是shell和MongoDB Compass 都可以正常连接.

这可能是由于无论是Node MongoDB Driver 还是  mongoose, 都不默认开启tls. 在配置中显式启用tls 即可.

```Typescript
import mongoose from 'mongoose';
import { DATABASE_URI } from './environment.ts';

const mongooseOptions: mongoose.ConnectOptions = {
  serverSelectionTimeoutMS: 10000,
  socketTimeoutMS: 45000,
  family: 4,
  tls: true,   // 显示开启tls
  dbName: 'SEM-MONEY-TRACKER',
};

const connectDB = async () => {
  try {
    await mongoose.connect(DATABASE_URI, mongooseOptions);
    let db = mongoose.connection;
    console.log(`MongoDB ${db.name} connected`);
  } catch (error) {
    console.error('MongoDB connection error:', error);
    
    process.exit(1);
  }
};

export default connectDB;
```



## .env 变量无法正确被读取

在.env 定义变量之后, ts编译阶段无法被读取. 原因为dotenv需要再最开始就调用来读取环境变量, 在启动应用的其他部分

在网上找到一些解决方案, 比如在应用入口最开始导入dotenv: 

```typescript
// index.ts
import dotenv from "dotenv";

dotenv.config();
```

但是不知为什么, 这对我无效.

我的解决方案是单独创建一个模块用于导入环境变量, 然后其他模块导入这个模块来获得环境变量: 

```ts
// config/envronment.ts

import dotenv from "dotenv";

dotenv.config();

export const DATABASE_URI = process.env.DATABASE_URI as string;
export const PORT = parseInt(process.env.PORT as string);
```



## 导入模块css后得到undefined

配置webpack: 

首先, 在`resolve`处添加`.css`后缀来允许webpack 解析css文件: 
```js
resolve: {
        extensions: ['.ts', '.tsx', '.js', '.jsx', '.css'],
    },
```

在确保已经下载`css-loader` 和 `style-loader`后, 在webpack 中配置rules: 

```ts
module: {
        rules: [
        {
            test: /\.(ts|tsx)$/, 
            use: 'ts-loader',
            exclude: /node_modules/, 
        },
        {
            test: /\.module\.css$/, 
            use: [
                'style-loader',
                {
                    loader: 'css-loader',
                    options: {
                        modules: true, // 启用模块CSS
                    },
                },
            ],
        },
        {
            test: /\.css$/, 
            exclude: /\.module\.css$/, 
            use: ['style-loader', 'css-loader'],
        },
        {
            test: /\.(js|jsx)$/, 
            exclude: /node_modules/, 
            use: {
                loader: 'babel-loader',
            },
        },
    ],
    },
```

确保`.module.css`的规则设置在`.css`前, 以避免模块css的规则被覆盖.

在项目根目录, 也就是在`tsconfig.json` 中设置在`include:`选项中的文件夹, (一般是`src`文件夹)中添加声明文件: 

```ts
// declaration.d.ts

declare module '*.module.css' {
  const classes: { [key: string]: string };
  export = classes;
}
  
```

最后, 对导入的写法从直接导入改为命名空间导入: 

```ts
// 直接导入写法 (这种写法在我的项目里无法识别)
import styles from "xxx.module.css"

// 命名空间写法 (这种写法最终解决了我的问题)
import * as styles from "xxx.module.css"
```



## 在storybook中启用模块css

在上面我们成功在webpack 启用模块css 之后, 由于我们仅仅更改了我们项目的webpack配置, 而storybook 使用自己的webpack, 所以我们也需要对storybook 进行配置. 

在`.storybook`路径下的`main.ts`中进行如下配置: 

```ts
import type { StorybookConfig } from "@storybook/react-webpack5";

async function supportCssModules(config) {
  config.module.rules.find(
    (rule) => rule.test.toString() === '/\\.css$/'
  ).exclude = /\.module\.css$/

  config.module.rules.push({
    test: /\.module\.css$/,
    use: [
      'style-loader',
      {
        loader: 'css-loader',
        options: {
          modules: true,
        },
      },
    ],
  })

  return config
}


const config: StorybookConfig = {
  stories: [
    "../src/components/**/*.stories.tsx"
  ],

  addons: [
    "@storybook/addon-webpack5-compiler-swc",
    "@storybook/addon-onboarding",
    "@storybook/addon-essentials",
    "@chromatic-com/storybook",
    "@storybook/addon-interactions",
    "@storybook/addon-mdx-gfm",
    "@storybook/addon-viewport",
  ],

  framework: {
    name: "@storybook/react-webpack5",
    options: {},
  },
  

  docs: {},

  typescript: {
    reactDocgen: "react-docgen-typescript"
  },
  webpackFinal: supportCssModules, // 将上面的配置应用到storybook的webpack中
};
export default config;

```

## 应用初始化时, 应用整体被挂载两次

期初发现这个bug 的原因是因为我的页面组件的`useEffect` 钩子总是在页面加载后连续触发两次, 一开始我以为是React的组件生命周期问题, 在网络上查找后, 发现这一现象的普遍原因是因为React 18 后 `React.StricMode`带来的, 只在开发环境中出现. 但是经过我的测试, 我发现在我的应用中, 该现象也会在生产构建后的应用中出现. 

我的下一步调试, 是在`App.tsx`中添加`useEffect` 钩子, 以及在`index.tsx`中添加调试代码, 来判断具体的双重挂载到底是发生在那一层中, 最后发现, 我的所有应用从最顶层都会执行两次, 这就和网络上大多数人遇到相同表现的问题本质不同了.

最后, 我的问题出在我的index.html 模板中: 

在`webpack.config` 中, 我添加了插件`HtmlWebpackPlugin`: 
```js
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
        })
    ],
```

其作用就是以我提供的`html`文件作为模板, 在打包的时候注入我的TypeScript脚本, 也就是说, 该插件会主动注入 `<script src="bundle.js"></script>`, 而我的`index.html`模板中, 我手动编写了`<script src="bundle.js"></script>`, 这就导致最后脚本会运行两遍

解决方法: 
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>React App</title>
  </head>
  <body>
    <div id="root"></div>
    <!-- 不要手动加入 script 标签，HtmlWebpackPlugin 会自动注入 bundle.js -->
  </body>
</html>
```

