---
title: 从0打造一个 React的TS模板
date: 2019-02-15 16:24:14
tags: [react,typescript]
categories: 技术分享
---

# 从 0 打造一个React的TS模板

## 前言

最近导师安排了一个任务，将一个已配置好的React、Webpack4。。。等具有市场上常见功能的模板添加上TypeScript。俗话说：环境配置三小时，Coding不足5分钟。配置环境真的非常浪费时间，因此写个文章记录下，希望能对各位要配置TS的前端们有所帮助。

## 模板目录

先放上完整配置好的项目地址：
https://github.com/dcison/React-Typescript-Template

```
├── .babelrc 
├── .eslintrc 
├── .gitignore 
├── index.html 
├── package.json
├── tsconfig.json 
├── src
│   ├── reducers 
│   ├── components 
│   ├── actions.ts
│   ├── global.styl
│   ├── index.tsx
│   └── store.ts
└── static
```

## 文章目录

1. Babel、Webpack 基本配置
2. TypeScript配置
3. Eslint 校验配置，基于Vscode
4. React 
5. 样式相关
6. React-Router
7. Code Splitting
8. webpack 热更新
9. Redux
10. Git 提交校验

## Babel、Webpack

1. 首先在 src 中创建入口文件 index.js, 随便写点代码，比如
``` javascript
// src/index.js
import type from './components/Apple'  
console.log(type)

// src/components/Apple.js
export default {
    type: 'apple'
}
```
2. 配置webpack.config.babel.js
``` javascript
import path from 'path';
export default {
	mode: 'development',
	devtool: 'eval',
	entry: {
		index: './src/index.js',
	},
	output: {
		publicPath: '/',
		filename: 'js/[name].js',
		chunkFilename: 'js/[name].js',
		path: path.resolve(__dirname, 'dist')
	},
	module: {
		rules: [
			{
			  test: /\.js$/,
			  exclude: /node_modules/,
			  use: {
				loader: 'babel-loader'
			  }
			}
		]
	},	
	devServer: {
		port: 3000
	},
	resolve: {
		extensions: [
			'.jsx',
			'.js',
			'.ts',
			'.tsx'
		]
	}
};
```
3. 编写.babelrc 
```json
{
    "presets": [
        "@babel/preset-env"
    ],
    "plugins": [

    ]
}
```
4. 编写package.json中的 "scripts" 与 "devDependencies"
```json
{
  "scripts": {
    "start": "webpack-dev-server --content-base"
  },
  "devDependencies": {
    "@babel/cli": "^7.2.3",
    "@babel/core": "^7.2.2",
    "@babel/preset-env": "^7.3.1",
    "@babel/register": "^7.0.0",
    "babel-loader": "^8.0.5",
    "webpack": "^4.29.0",
    "webpack-cli": "^3.2.1",
    "webpack-dev-server": "^3.1.14"
  },
}
```
5. 运行 yarn 或者 npm i 安装依赖后 yarn start 或者 npm start，能看到success就表示Babel、Webpack配置成功了。 


## TypeScript

1. 把刚才2个JS文件后缀改为tsx
2. 配置webpack.config.babel.js,包括rules，入口文件的后缀：改为tsx。
``` javascript
entry: {
		index: './src/index.tsx',
},
{
   test: /\.tsx?$/,
   exclude: /node_modules/,
   use: {
   	loader: 'ts-loader'
   }
}
```
3. 在根目录下创建一个 tsconfig.json
```javascript
{
    "compilerOptions": {
      "module": "esnext",
      "target": "es5",
      "lib": ["es6", "dom"],
      "sourceMap": true,
      "jsx": "react",
      "noImplicitAny": true,
      "allowJs": true,
      "moduleResolution": "node"
    },
    "include": [
      "src/*"
    ],
    "exclude": [
      "node_modules"
    ]
  }
```
4. 安装依赖 yarn add -D ts-loader typescript
5. yarn start 如果依旧 success 即表示成功

## Eslint

1. 添加.eslintrc 配置文件,如下：
``` json
{
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "ecmaFeatures": {
          "experimentalObjectRestSpread": true,
          "jsx": true
        },
        "sourceType": "module"
    },
    "parser": "typescript-eslint-parser",
    "plugins": [
		"react",
        "typescript"
    ],
    "rules": {
        //... 自行添加规则
    }
}
```
2. 安装依赖 yarn add -D eslint eslint-plugin-typescript typescript-eslint-parser eslint-plugin-react                           
3. 配置Vscode：首选项 -> 设置(即setting.json),找到eslint.validate，没有就自己写，加入typescript与typescriptreact，分别用于监听ts与tsx文件，如下：
```json
"eslint.validate": [
  "javascript",
  "javascriptreact",
  {
      "language": "typescript",
      "autoFix": true
  },
  {
      "language": "typescriptreact",
      "autoFix": true
  }
]
```

## React
1. 使用配好 html-webpack-plugin 插件 ,yarn add -D html-webpack-plugin
2. 在webpack.config.babel.js中配置下
``` javascript
import HtmlWebpackPlugin from 'html-webpack-plugin';

plugins: [	
		new HtmlWebpackPlugin({
			title: '模板',
			hash: false,
			filename: 'index.html',
			template: './index.html',
		})
]
```
3. 在根目录下写一个 html 模板
``` html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title><%= htmlWebpackPlugin.options.title || ''%></title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
```
4. 安装React相关依赖: yarn add react @types/react react-dom @types/react-dom @babel/preset-react
5. .babelrc 中添加一条规则
```json
"presets": [
        "@babel/preset-env",
        "@babel/preset-react" // 添加这条
],
```   
6. 修改src/index.tsx
``` typescript
import * as React from 'react';
import * as ReactDOM from "react-dom";
class SomeComponent extends React.Component<{}, {}> {
	render () {
		return <div>hello</div>;
	}
}
ReactDOM.render(
	<SomeComponent/>,
	document.getElementById('root')
);
```
7. 在根目录运行yarn start ，在界面上看到hello就表示React的环境配好了

## CSS 及 样式预处理 （以Stylus为例）

1. 安装CSS相关loader：yarn add -D css-loader style-loader
2. 配置webpack.config.babel.js 中的rules
```javascript
{
	test: /\.css$/,
	use: [
		'style-loader',
		{
			loader: 'css-loader',
			options: {
				localIdentName: '[path][name]__[local]--[hash:base64:5]'
			}
		}
	]
}
```
1. 测试：在src下建一个css文件，然后在index.tsx中引用
```typescript
import './global.css';
```
4. 能看到样式被应用就表示CSS配置正常了，接下来配置Stylus
5. 安装相关依赖: yarn add -D stylus-loader stylus
6. 继续配置webpack.config.babel.js 中的rules
``` javascript
{
	test: /\.styl$/,
	use: [
		'style-loader',
		{
			loader: 'css-loader',
			options: {
				modules: true,
				import: true,
				importLoaders: 1,
				localIdentName: '[path][name]__[local]--[hash:base64:5]'
			}
		},
		{ loader: 'stylus-loader' },
	]
},
```
7. 更改刚才CSS后缀文件为styl，index引用中的文件也改为相应的Styl文件，如果样式依旧有改动表示样式预处理器配置成功。

追加： 添加typings-for-css-modules-loader 
1. 安装依赖 yarn add -D typings-for-css-modules-loader css-loader@^1.0.1 (css-loader 2.x版本以上有问题)
2. 修改webpack.config.babel.js 中的rules（此处只修改stylus）
``` javascript
{
	test: /\.styl$/,
	use: [
		'style-loader',
		{
			loader: 'typings-for-css-modules-loader',
			options: {
				modules: true,
				namedExport: true,
				camelCase: true,
				minimize: true,
				localIdentName: "[local]_[hash:base64:5]",
				stylus: true
			}
		},
		{ loader: 'stylus-loader' },
	]
},
```
3. 使用方式：
``` typescript
import * as styles from './apple.styl';

<div className={styles.redFont}>测试文字</div>
```
4. [坑](https://www.jianshu.com/p/99435f17ace8)
  
   1. > 这里我们遇到一个坑，所有的css-moduels/typings-for-css-modules-loader教程并不会告诉你这里include配置不写会造成什么影响。当我们此前没有写这句话的时候，d.ts因为索引速度不够快，或者说没有在内部自动建立联系(这里原因比较迷)，会导致我们命令行和窗口直接报未找到类型的错误，需要手动重新编译一次，效率极低，当我们加上include后就可以随便折腾了(迷)include:path.resolve('src/')

		不知为何，添加inclue配置后，依旧会报未找到对应的样式模块错（但此时对应的.d.ts声明文件已经生成）。由于开启了热更新，不需要再次重新编译，改动一个小内容触发一下webpack的热更新即可正常使用。
	
	2. 第二个坑，如果你只引用，而不用，会无法生成对应的.d.ts文件，因此会打包失败


## React-Router

1. 安装依赖：yarn add react-router-dom @types/react-router-dom
2. 修改下webpack.config.babel.js中的devServer，添加historyApiFallback
```javascript
devServer: {
   historyApiFallback: true
}
```
3. 在Component中的apple.tsx与xiaomi.tsx修改一下代码，如下
``` typescript
// src/components/Apple.tsx
import * as React from 'react';
class Apple extends React.Component<any, any> {
    state = {
    	text: 'iphone XR'
    }
    handleChange = () => {
    	this.setState({
    		text: 'iphone8'
    	});
    }
    render () {
    	return <>
            <button onClick={this.handleChange}>点我换机子</button>
            {this.state.text}
        </>;
    }
}
export default Apple;
// src/components/xiaomi.tsx 
// 代码结构类似，就只改了下text的内容，所以不放了
// text: 'xiaomi 8'
```
4. 修改index.tsx中的代码，如下
``` typescript
import * as React from 'react';
import * as ReactDOM from "react-dom";
import { BrowserRouter, Route, Redirect, Switch, Link } from 'react-router-dom';
import Apple from './components/Apple';
import Xiaomi from './components/XiaoMi';
import './global.styl';
const Header = () => (
	<ul>
		<li><Link to="/">苹果</Link></li>
		<li><Link to="/phone/xiaomi">小米</Link></li>
	</ul>
);
ReactDOM.render(
	<div>
		<BrowserRouter>
			<>
			<Header />
			<Switch>
				<Route path="/phone/apple" component={Apple} />
				<Route path="/phone/xiaomi" component={Xiaomi} />
				<Redirect to="/phone/apple" />
			</Switch>
			</>
		</BrowserRouter>
	</div>,
	document.getElementById('root')
);
```
5. 启动你的yarn start 进行测试吧

## code splitting
1. 在Component目录下新增一个文件，asyncComponent.tsx
```typescript
import * as React from "react";
export default function asyncComponent (importComponent: any) {
	class asyncComponent extends React.Component<any, any> {
		constructor (props: any) {
			super(props);
  
			this.state = {
				component: null
			};
		}
  
		async componentDidMount () {
			const { default: component } = await importComponent();
  
			this.setState({
				component: component
			});
		}
  
		render () {
			const C = this.state.component;
  
			return C ? <C {...this.props} /> : null;
		}
	}
  
	return asyncComponent;
}
```
2. 在index.tsx引用的地方改为以下内容
```typescript
import asyncComponent from "./components/asyncComponent";
const Apple =  asyncComponent(() => import("./components/Apple"));
const XiaoMi =  asyncComponent(() => import("./components/XiaoMi"));
```
3. 如果出现下图情况则表示成功了
![pic1][pic1]

## webpack 热更新
1. 安装依赖 yarn add -D @types/webpack-env
2. 开启热更新，修改webpack.config.babel.js的plugins、devServer ,添加两个属性
```javascript
import webpack from 'webpack';
plugins: [	
		new webpack.HotModuleReplacementPlugin()
],
devServer: {
		hot: true
},
```
3. 在入口文件中加入以下代码，更多详情可参考[官网](https://webpack.js.org/guides/hot-module-replacement/)
```typescript
if (module.hot) {
	module.hot.accept();
}
```
4. 随意修改一些文字内容，如果有下图类型情况出现表示配置成功
  
![pic2][pic2]

## Redux

1. 安装依赖：yarn add @types/react-redux react-redux redux redux-thunk
2. 在src/下添加一个actions.ts，用于存放所有actions
```typescript
export const initPhone = (data: object) => {
	return {
		type: 'INIT_PHONE',
		data
	};
};
export const setPhoneMoney = (data: object) => {
	return {
		type: 'SET_MONEY',
		data
	};
};
```
3. 在src/下新建一个 reducers 目录，存放各业务reducer
```typescript
// src/reducers/Phone.ts
var initState = {
	name: '',
	money: 0
};
export default function (state = initState, action: any) {
	switch (action.type) {
			case 'INIT_PHONE':
				return {
					...state,
					name: action.data.name,
					money: action.data.money
				};
			case 'SET_MONEY':
				return {
					...state,
					money: action.data.money
				};
			default:
				return state;
	}
}
```
4. 在src/下建store.ts
```typescript
import { createStore, applyMiddleware, combineReducers } from 'redux';
import thunkMiddleware from 'redux-thunk';
import Phone from './reducers/Phone';
const rootReducer =  combineReducers({
	Phone
});
const createStoreWithMiddleware = applyMiddleware(
	thunkMiddleware,
)(createStore);
function configureStore (initialState?: any) {
	return createStoreWithMiddleware(rootReducer, initialState);
}
export default configureStore();
```
5. 修改组件中的代码，这里以Component/Apple.tsx为例
```typescript
import * as React from 'react';
import { connect } from 'react-redux';
import * as action from '../actions';
class Apple extends React.Component<any, any> {
	componentDidMount () {
		this.props.initPhone({ name: '苹果8', money: 10000 });
	}
    handleChange = () => {
    	this.props.setPhoneMoney({ money: this.props.money - 20 });
    }
    render () {
    	return <>
            <button onClick={this.handleChange}>点我降价</button>
            {this.props.name} 现在仅售价 {this.props.money}
        </>;
    }
}
function mapStateToProps (state: any) {
	return {
		name: state.Phone.name,
		money: state.Phone.money
	};
}
export default connect(mapStateToProps, action)(Apple);
```
6. 修改入口的代码（src/index.tsx）
```typescript
import { Provider } from 'react-redux';
import store from './store';

<Provider store={store}> //用该容器包裹一下
		<BrowserRouter>
			<>
			<Header />
			<Switch>
				<Route path="/phone/apple" component={Apple} />
				<Route path="/phone/xiaomi" component={XiaoMi} />
				<Redirect to="/phone/apple" />
			</Switch>
			</>
		</BrowserRouter>
	</Provider>
```

## Husky 与 lint-staged

1. 安装依赖 yarn add -D lint-staged husky
2. 在package.json中添加以下代码即可
```json
"scripts": {
    "precommit": "lint-staged"
  },
"lint-staged": {
 "*.{ts,tsx}": [
   "eslint --fix",
   "git add"
 ]
}
```
3. 自行提交测试下吧~


----

### 参考

* [Code Splitting](https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html)
* [Eslint集成Typescript提示React代码](http://react-china.org/t/eslint-typescript-react/16711)
* [用 husky 和 lint-staged 构建超溜的代码检查工作流](https://segmentfault.com/a/1190000009546913)
* [关于CSS模块化](https://github.com/parcel-bundler/parcel/issues/1320)


[pic1]: https://user-gold-cdn.xitu.io/2019/2/1/168a7b60cf9133fc?imageView2/0/w/1280/h/960/format/webp/ignore-error/1
[pic2]: https://user-gold-cdn.xitu.io/2019/2/1/168a7b6abd186f37?imageView2/0/w/1280/h/960/format/webp/ignore-error/1