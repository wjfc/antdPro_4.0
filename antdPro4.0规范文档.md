# 目录

1. 前言
2. 基础规范
3. 项目初始化
4. 项目目录结构
5. 开发配置
6. 其他注意点

## 前言

****后台界面框架基于 Ant Design Pro 4.0版本开发。

技术栈基于 ES2015+、React、UmiJS、dva、g2 和 antd，
##### 框架简介
dva 首先是一个基于 redux 和 redux-saga 的数据流方案，然后为了简化开发体验，dva 还额外内置了 react-router 和 fetch，所以也可以理解为一个轻量级的应用框架。
umi 是蚂蚁金服的底层的一个可插拔的企业级 react 应用框架，与 dva 融合。
umi 以路由为基础的，roadhog + 路由,支持[类 next.js 的约定式路由](https://umijs.org/zh/guide/router.html)，以及各种进阶的路由功能，并以此进行功能扩展，
g2 图表框架 
antd 蚂蚁金服ui组件库
点击下方链接了了解相关知识。

[antd pro 官网文档地址](https://pro.ant.design/docs/getting-started-cn)

[dva 官网文档地址](https://dvajs.com/guide/)

[umijs 官网文档地址](https://umijs.org/zh/guide/getting-started.html)

[g2官方文档](https://antv.alipay.com/zh-cn/g2/3.x/index.html)

[antd 官方文档](https://ant.design/docs/react/introduce-cn)
## 基础规范

1. 尽量使用 es6 进行开发，提高开发效率。
2. 组件 jsx 文件中，必须严格使用 jsx 语法。样式表文件中，使用less语法。
3. 组件文件统一使用大驼峰命名法，例如 HeaderCompoent.js；
4. 除了公共 less 文件，统一放到当前组件目录下，便于查找和修改。
5. 用到类名的地方全部使用 className 代替 class。需要用到多个class类名的时候，请参考classnames这个库的写法。
```
var classNames = require('classnames');
classNames('foo', 'bar'); // => 'foo bar'
```
6. 遇到比较大的组件，使用懒加载的形式，提供页面渲染效率。比如：`const IntroduceRow = React.lazy(() => import('./IntroduceRow'));`
7. 页面所有菜单均要在 config/config.js 文件中进行配置。
8. 路由跳转方式
```
// 采用umi封装的路由跳转的两种方法。需分别引用‘umi/link’，‘umi/router’
// 法一、声明式，类似于a链接。 法二、命令式。
  <Link to="/list">Go to list page</Link> //方法一

  router.push('/powerManage/analysis');//方法二
```

## 项目初始化

npm install & npm start

## 项目目录结构

```

├── config # umi 配置，包含路由，构建等配置
├── mock # 本地模拟数据
├── public
│ └── favicon.png # Favicon 公共资源文件
├── src
│ ├── assets # 本地静态资源
│ ├── components # 业务通用组件
│ ├── e2e # 集成测试用例
│ ├── layouts # 通用布局
│ ├── models # 全局 dva model
│ ├── pages # 业务页面入口和常用模板
│ ├── services # 后台接口服务
│ ├── utils # 工具库
│ ├── locales # 国际化资源
│ ├── global.less # 全局样式
│ └── global.js # 全局 JS
├── tests # 测试工具
├── README.md
└── package.json

```

## 开发配置

### 代理配置
-在 config.js 文件中配置开发环境下的接口代理
格式如下：
```
  proxy: {
 '/apis/': {
   // 匹配所有以/apis/为开头的接口
   target: 'http://129.28.200.56:8081', // 后端服务器地址
   changeOrigin: true, 
 },
},
```
#### 页面所有菜单配置
- config.js 文件导出的routes中配置全菜单信息
  ```
  name: 展示的菜单名称,名称统一使用英文，并在国际化资源中提前配置好。
  path:为访问的路径；
  components:对应路径展示的react组件；
  routes：子路由，如果该路由下有子路由则配置，没有则忽略。
  authority：用来配置这个路由的权限，如果配置了将会验证当前用户的权限，并决定是否展示，值需是一个数组。  ['admin', 'user']；
  hideChildrenInMenu： 用于隐藏不需要在菜单中展示的子路由。
  hideInMenu： 可以在菜单中不展示这个路由，包括子路由。
  Routes：umi 的权限路由是通过配置路由的 Routes 属性来实现。
  ```
### 组件化开发
- assets 资源目录，图标之类可放置在这里。
- components 公共组件目录。
- e2e 
-layouts 通用页面板式布局。
- pages 页面入口组件存放目录（上面菜单栏配置的所有路径，都是这里写好的），每个页面都有独立的models，其他页面是不能访问到。
- models 是全局的 dva model目录，所有组件都能访问到。
- services 目录放置公共 api，所有的 api 在这里维护。不同类型的接口，需要单独建立文件夹，保证接口配置的可读性。
- locales 国际化资源目录。antdpro很多地方需要使用到，开发时逐个添加即可，不可重复定义。
utils 常用的工具目录。
- defaultSettings 默认配置，比如标题，框架的主题色之类。
- document.ejs    html模版

### 其他注意点
- antd中用到日期组件的部分，需要用moment这个库进行转换，否则会有警告提示。
- 项目用引入了lodash这个库，可以方便快捷的操作数组、对象。
- memoize-one  因为react每一次render都要过滤一遍数据，有时候会有大量的计算，这个js库可以缓存一个实例。不同的数据会覆盖上一次，相同的数据返回上次的结果。
- Numeral.js 是一个用于格式化和数字四则运算的js 库。
- [Bizcharts](https://github.com/alibaba/BizCharts) 基于g2又做了一层封装。[demo](https://bizcharts.net/products/bizCharts/demo)
- withRouter 可以在不是经过路由切换的组件中，将 history、location、match 三个对象传入props对象上，灵活使用即可。
- axios 接口请求统一使用axios这个库，umi封装的request库，不能获取到响应头，所以摒弃掉。
- 动态路由配置 在BasicLayout组件中，修改menuDataRender的返回值。
后台应返回给前端标准的菜单格式，如下：
```
var menuData = [
	{
		path: '/messageCenter',
		name: 'messageCenter',
		icon: 'bell',
		children: [
			{
				path: '/messageCenter/station',
				name: 'station',
				exact: true
			},
			{
				path: '/messageCenter/all',
				name: 'all',
				exact: true
			},
			{
				path: '/messageCenter/unread',
				name: 'unread',
				exact: true
			},
			{
				path: '/messageCenter/readed',
				name: 'readed',
				exact: true
			}
		]
	}
];
```
