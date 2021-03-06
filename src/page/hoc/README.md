# 概念
* 高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。(代码复用)
* 高阶组件是参数为组件，返回值为新组件的函数。

* 在这个范式下，代码通过一个类似于 装饰器（decorator） 的技术进行共享。首先，你的一个组件定义了大量需要被渲染的标记，之后用若干具有你想用共享的行为的组件包裹它。因此，你现在是在 装饰 你的组件，而不是混入你需要的行为
* 高阶组件会返回一个增强（E）的组件。高阶组件让我们的代码更具有复用性，逻辑性与抽象特。它可以对props和state进行控制，也可以对render方法进行劫持...

# 用处
> 需要一个抽象，允许我们在一个地方定义这个逻辑，并在许多组件之间共享它。这正是高阶组件擅长的地方。
> 包裹（wrapped）被传入的React组件
> 相同模式、操作，进行抽象(抽象同样的state操作) 获得一个相对增强(enhanced)的组件
> 把原组件抽象为展示型组件，分离内部状态，搞成无状态组件
* 数据加载 loading的过渡、request触发、sourceData作为prop传入
* 权限校验 disabled的state的提升
* 添加事件监听(交易场景的统一事件添加)
* 给生命周期添加middleware

# 和函数组件、普通组件的区别
* 目的是增强被包裹组件功能
* 组件是将 props 转换为 UI，而高阶组件是将组件转换为另一个组件。
* 组件是 React 中代码复用的基本单元。但你会发现某些模式并不适合传统组件。
* 在一个大型应用程序中，这种订阅 DataSource 和调用 setState 的模式将一次又一次地发生。我们需要一个抽象，允许我们在一个地方定义这个逻辑，并在许多组件之间共享它。这正是高阶组件擅长的地方。
```
CommentList 和 BlogPost 不同 - 它们在 DataSource 上调用不同的方法，且渲染不同的结果。但它们的大部分实现都是一样的：
* 在挂载时，向 DataSource 添加一个更改侦听器。
* 在侦听器内部，当数据源发生变化时，调用 setState。
* 在卸载时，删除侦听器。
```
* 您可能已经注意到 HOC 与容器组件模式之间有相似之处。容器组件担任分离将高层和低层关注的责任，由容器管理订阅和状态，并将 prop 传递给处理渲染 UI。HOC 使用容器作为其实现的一部分，你可以将 HOC 视为参数化容器组件。

# 方式
* 属性代理
    * 公共逻辑的抽离  修改 props,抽象 state, 调取 refs
* 反向继承
    * 渲染劫持

# 突出问题
* 不要在 render 方法中使用 HOC：每次调用 render 函数都会创建一个新的 EnhancedComponent，重新挂载组件会导致该组件及其所有子组件的状态丢失。
    * 在极少数情况下，你需要动态调用 HOC。你可以在组件的生命周期方法或其构造函数中进行调用。
* 务必复制静态方法：新组件没有原始组件的任何静态方法
* Refs 不会被传递：虽然高阶组件的约定是将所有 props 传递给被包装组件，但这对于 refs 并不适用。那是因为 ref 实际上并不是一个 prop - 就像 key 一样，它是由 React 专门处理的。如果将 ref 添加到 HOC 的返回组件中，则 ref 引用指向容器组件，而不是被包装组件
* 在高阶组件包裹后，应当保留其原有名称, 便于调试，增加getDisplayName函数以及静态属性displayName，
