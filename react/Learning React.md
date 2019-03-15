# Learning React

## 基本概念

- **Element**
	- Unlike browser DOM elements, **React elements are plain objects** 
	- React elements are **immutable**. Once you create an element, you can’t change its children or attributes. An element is like **a single frame** in a movie: it represents the UI at a certain point in time.
	- two types of  react elements
		- "**regular" HTML DOM elements** (simple, stateless, and immutable)
		- **instantiations of a React Class** (i.e. Component)(don't have a "type" because they don't represent a typical DOM element.)
- **Component**
	- 组件类的第一个字母必须大写
	- 组件类只能包含一个顶层标签
	-  所有组件类都必须有自己的 render 方法
	- **this.props**
		- 组件的用法与原生的 HTML 标签完全一致，可以任意加入属性。组件的属性可以在组件类的 this.props 对象上获取，比如 name 属性就可以通过 this.props.name 读取
	- **this.children** 
		- 表示组件的所有子节点 
	- **ref**
		- ref 是组件元素的属性，映射到真是DOM 节点的名称
		- this.refs.[refName] 就会返回真实的 DOM 节点
	- **this.state**
		- 组件看成是一个状态机
		- 状态变化触发重新渲染 UI
		- this.props 和 this.state 都用于描述组件的特性。简单的区分是，this.props 表示那些一旦定义，就**不再改变**的特性，而 this.state 是会随着用户互动而产生**变化**的特性。
## JSX 语法
- JSX 的基本语法规则：遇到 HTML 标签（以 < 开头），就用 HTML 规则解析；遇到代码块（以 { 开头），就用 JavaScript 规则解析
- JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员。
## Flux

### Overview
![enter image description here](https://hulufei.gitbooks.io/react-tutorial/content/image/flux-overview.png)
 - Flux 是一种设计模式，类比于MVC。Flux 之于Web application （或React） 是一种选择而不是必须。
 - Flux provides a way to provide the data that React will use to create the UI.
 - Flux 和React 是解耦的。**Flux 只是一种设计模式，发力点在于易用，易扩展** 是为复杂应用而设计，解决的问题是：
	- 代码组织
	- 组件之间的通信
 - Redux 是Flux 目前最好的实现
## Redux
	
 - http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html
 - 	http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html
	 - https://juejin.im/post/5acc8c7cf265da2393776859 这篇文章介绍了中间件的设计思想（Redux 和KOA都是使用中间件来解决问题的）
 - 	http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html
 - https://github.com/kenberkeley/redux-simple-tutorial
 - https://www.zhihu.com/question/47686258
### 概念
- Store
	- 每应用**唯一**实例，可以理解为 model instance。
	- 每个store 对应一个reducer （集中处理状态变迁）。
	- API
		- store.getState()
		- store.dispatch(action)
		- store.subscribe(listener)
- State
	- 某一时刻，store 中数据的**快照**。（既然是快照，就意味着不能修改。）
- Action
	- 是一个**对象**。描述当前发生的事情。传递数据到store，驱动state 迁移。
- Reducer
	- **纯**函数。它接受 Action 和当前 State 作为参数，返回一个新的 State。
		- 不允许有副作用，不能调用有副作用的函数。
	- 为什么这个函数叫做 Reducer 呢？因为它可以作为数组的reduce方法的参数
### 工作流程
![enter image description here](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)
## 工程化开发

React 的设计思路还是基于组件的模式来分治问题。丰富的组件库是很重要的一环：

- [ant design](https://ant.design/) 
- https://github.com/brillout/awesome-react-components
- https://blog.bitsrc.io/11-react-component-libraries-you-should-know-178eb1dd6aa4
- 


Redux 的简单教程：http://www.cnblogs.com/zhongchao666/p/9478008.html

很简单的方式对比了Vue 和React：https://blog.csdn.net/HTX_HelloWorld/article/details/75402938

不错的一本书：http://huziketang.mangojuice.top/books/react/
       

## Ref
1.  https://stackoverflow.com/questions/40121680/get-html-tag-name-from-react-element
2. http://www.ruanyifeng.com/blog/2015/03/react.html
3. React 设计思想 https://github.com/react-guide/react-basic
4. http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUwNzM4MDczOSwtOTQzNzk1NzYsMTkxMj
QwNTYxNV19
-->