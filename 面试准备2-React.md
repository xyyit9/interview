---
title: 面试准备2-React
tags: 面试
categories: 面试
---

https://segmentfault.com/a/1190000005151182#articleHeader13

### 创建一个 React 组件

调用 React.createClass 方法，传入的参数为一个对象，对象必须定义一个**render方法**，render 方法返回值为组件的渲染结构，也可以理解为一个组件实例（React.createElement 工厂方法的返回值）， **返回值有且只能为一个组件实例**，或者返回 null/false，当返回值为 null/false 的时候，React 内部通过 <noscript/> 标签替换。

### 组件状态 State

* getInitialState：获取组件的初始状态，在组件加载的时候会被调用一次，返回值赋予 **this.state** 作为初始值
* setState 方法支持按需修改，如 state 有两个字段，仅当 setState 传入的对象包含字段 key 才会修改属性
* 每次调用 setState 会导致重渲染调用 render 方法
* 直接修改 state 不会重渲染组件

###  组件属性 Props

* getDefaultProps: 获取默认属性对象，会被调用一次，当组件类创建的时候就会被调用，返回值会被缓存起来，当组件被实例化过后如果传入的属性没有值，会返回默认属性值
* this.props.children：所有子节点的属性
* propTypes: 属性类型检查

### React数据流

数据的流动管道就是 props，流动的方向就是组件的层级自顶向下的方向。所以 **一个组件是绝对不能修改自身的props**，组件的属性一定是通过父组件传递而来（或者默认属性）。

### 什么是调和？

将页面分割成一个个组件，一个组件还可能嵌套更小的组件，每个组件有自己的数据（属性/状态）;当某个组件的数据发生变化时，更新该组件部分的视图。更新的过程是由数据驱动的，新的数据自该组件顶层向下流向子组件，每个组件调用自己的`render`方法得到新的视图，并与之前的视图作**diff**-比较差异，完成更新。这个过程就叫作**reconciliation-调和**。

### React中使用key的必要性

https://www.jianshu.com/p/0218ff2591ec

发现原来的两个li元素都与新v-dom中对应位置上的两个li元素不同，就会对其修改，并向真实dom树中插入新的second节点。实际上，我们可能只是进行了在first之前插入新zero节点的操作，而现在进行了额外的修改操作。

[React官方文档](https://link.jianshu.com?t=https://facebook.github.io/react/docs/reconciliation.html#keys)提示我们应该使用**key**属性来解决上述问题。key是一个字符串，用来唯一标识同父同层级的兄弟元素。当React作diff时，只要子元素有key属性，便会去原v-dom树中相应位置（当前横向比较的层级）寻找是否有同key元素，比较它们是否完全相同，若是则复用该元素，免去不必要的操作。

### 什么是React diff算法？

https://zhuanlan.zhihu.com/purerender/20346379

### 为什么要使用虚拟DOM？

https://www.jianshu.com/p/f75c1f0af3f0

![浏览器工作流](https://res.cloudinary.com/hashnode/image/upload/v1472288564/wvbwscn7oadykroobdd3.png)

浏览器渲染引擎工作原理：

1、接收字节,处理HTML,构建DOM树

2、处理CSS,构建CSSOM树

3、将DOM树和CSSOM树合并构成渲染树

4、根据渲染树来布局,为每个节点分配固定坐标

5、在屏幕中渲染像素,绘制节点

浏览器什么时候重新渲染页面？

页面绘制到屏幕上之后，根据js脚本，还可能有重绘和重排的操作，重绘是指没有对任何dom节点的形状（大小）进行改变，只改变了背景颜色等样式;重排是指改变了dom节点的形状，需要重新计算生成整个渲染树，重排的开销比重绘大的多。

只要你在这过程中进行一次DOM更新，整个渲染流程都会重做一遍。尤其是创建渲染树，它需要重新计算所有元素上的所有样式。Virtual DOM这个抽象层真正的闪光点正在于此：每当你想对视图进行一次更新，那些本该直接作用于真实DOM的改动，都会先作用于Virtual DOM，然后再将要改动的部分通知到真实DOM。这样可以大幅减少DOM操作带来的重计算步骤。

### React函数组件和类组件的区别

https://segmentfault.com/a/1190000010320951

1. 函数组件的代码量比类组件要少一些，所以函数组件比类组件更加简洁。
2. **函数组件中，你无法使用State，也无法使用组件的生命周期方法，**这就决定了函数组件都是展示性组件（Presentational Components），接收Props，渲染DOM，而不关注其他逻辑。
3. 函数组件中没有`this`。而在类组件中，你依然要记得绑定`this`这个琐碎的事情。
4. 函数组件更容易理解。当你看到一个函数组件时，你就知道它的功能只是接收属性，渲染页面，它不执行与UI无关的逻辑处理，它只是一个纯函数。而不用在意它返回的DOM结构有多复杂。
5. 性能。目前React还是会把函数组件在内部转换成类组件，所以使用函数组件和使用类组件在性能上并无大的差异。
6. 函数组件迫使你思考最佳实践。这是最重要的一点。组件的主要职责是UI渲染，理想情况下，所有的组件都是展示性组件，每个页面都是由这些展示性组件组合而成。如果一个组件是函数组件，那么它当然满足这个要求。所以牢记函数组件的概念，可以让你在写组件时，先思考这个组件应不应该是展示性组件。更多的展示性组件意味着更多的组件有更简洁的结构，更多的组件能被更好的复用。

### 组件生命周期和方法

- Mounting：已插入真实 DOM
- Updating：正在被重新渲染
- Unmounting：已移出真实 DOM


- **componentWillMount** 在渲染前调用,在客户端也在服务端。
- **componentDidMount** : 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异部操作阻塞UI)。
- **componentWillReceiveProps** 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。
- **shouldComponentUpdate** 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。 
  可以在你确认不需要更新组件时使用。
- **componentWillUpdate**在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。
- **componentDidUpdate** 在组件完成更新后立即调用。在初始化时不会被调用。
- **componentWillUnmount**在组件从 DOM 中移除的时候立刻被调用。

### React 组件、元素以及实例

https://www.jianshu.com/p/52d0097e065b

http://www.oschina.net/translate/react-components-elements-and-instances

创建一个类来声明一个 `Button` 组件，在页面中可能会有这个组件的多个实例，每一个实例都有自己的 propertities（属性）和 local state（本地状态）。

每一个组件必须保留对其 DOM 节点和 子组件实例的引用，并在适当的时候创建、更新、销毁它们。代码的规模也会随着组件状态复杂化而增大，并且父组件能够直接访问子组件的实例，导致未来难以对它们解耦。

元素的引入就是为了解决上述问题。元素就是一个用语描述出现在页面中的 DOM 节点或者 React 组件的纯对象。元素可以在自己的属性中包含其它元素。创建一个元素的成本很低，一旦元素被创建之后，就不再发生变化。它只包含组件类型（例如 `Button`）、相关属性（例如 `color`）以及内部的子元素。元素并不是一个真正的实例，而是一种用来告诉 React 你希望哪些东西显示在页面中的方式。你不能调用元素里的任何方法，因为它一个不可变的描述对象，包含两个属性：`type(string | ReactClass)` 和 `props(Object)`。

### React技术栈相关概念

[Babel](https://babeljs.io/)是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。

webpack配置文件：entry、output、devServer、module、 plugins。webpack可以看做是**模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。

 [CSS Modules](https://github.com/css-modules/css-modules) 加入了局部作用域和模块依赖，可以保证某个组件的样式，不会影响到其他组件。

[React-Router](https://github.com/reactjs/react-router) 路由库它过管理 URL，实现组件的切换和状态的变化，开发复杂的应用几乎肯定会用到。

Flux 是一种架构思想，专门解决软件的结构问题。它跟MVC 架构是同一类东西，但是更加简单和清晰。

Redux 将 Flux 与函数式编程结合一起

[`Mocha`](https://mochajs.org/)（发音"摩卡"）诞生于2011年，是现在最流行的JavaScript测试框架之一，在浏览器和Node环境都可以使用。通过它，可以为JavaScript应用添加测试，从而保证代码的质量。

[Istanbul](https://github.com/gotwarlost/istanbul) 是 JavaScript 程序的代码覆盖率工具









