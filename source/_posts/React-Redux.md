---
title: React-Redux初试
date: 2016-07-12 13:30:36
categories: react
tags:
    - react
    - redux
---

昨天把学习Redux的理解整理了一下{% post_link Redux 《Redux初试》 %}，大家都知道，Redux能火的原因，一部分也有ReactJs的原因，所以我也逃脱不了配合使用ReactJs的命运，SO今天就总结下React-Redux的用法，其实React-Redux不是很难的，说容易点，其实他本身就是个组件，可以友好的连接React和Redux，达到ReactJs使用Redux的目的。
<!-- more -->

看了下官方的 [API](https://github.com/reactjs/react-redux/blob/master/docs/api.md#api) 主要的就两个要点，Provider 和 connect, 那我们就看看这两个主要的接口喽。

## Provider

>Makes the Redux store available to the connect() calls in the component hierarchy below. Normally, you can’t use connect() without wrapping the root component in `<Provider>`.
翻译：
>使得 Redux store 可以让下面的组件结构能够使用 connect() 调用，通常情况下，你不能够使用 connect() 链接到没有用 `<Provider>` 当作根的组件！

好吧，别介意，使用有道翻译的，用白话说就是，如果你想让你的组件可以使用 connect() 操作 redux store 的话，你必须把 `<Provider>` 当作你的根组件。可想而知，Provider 就是为了让你的 ReactJs 可以使用 Redux 管理状态而存在的，这样的话，就需要你把项目的根组件修改为 `<Provider>`。

```js
ReactDOM.render(
    <Provider store={store}>
        <MyRootComponent />
    </Provider>,
    document.getElementById('root')
)
```
好吧，这个`<Provider>`就是如此的简单，可能我还没学到深的，那接下来就来说说 connect() ，看上去像个函数。

## Connect

> Connects a React component to a Redux store.
翻译
> 链接一个 React 组件到一个 Redux store

意思就是通过 connect 链接 Redux 和 ReactJs 可以让 ReactJs 使用 Redux 的状态。这个 connect 有四个参数：
### mapStateToProps
这个是用于自动监听 Redux 的 state ，然后将 Redux 监听到的数据传入到组件的 props 里，以供组件可以使用。
```js
function mapStateToProps(state) {
  return { todos: state.todos }
}
```
### mapDispatchToProps
这个是把 Dispatch 即我们所写的 action 里的方法，传入组件的 props 里供组件可以直接使用，而不需要在手动的引入 action 。
```js
function mapDispatchToProps(dispatch) {
  return { actions: bindActionCreators(actionCreators, dispatch) }
}
```
### mergeProps
看名字就可以猜到，mergeProps 是用来合并 props 的，可以把我们上两个参数的值 mapStateToProps mapDispatchToProps 以及组件本身的 props 合并在当前组件的 props 里，让组件可以同时访问。
```js
function mergeProps(stateProps, dispatchProps, ownProps) {
  return Object.assign({}, ownProps, {
    todos: stateProps.todos[ownProps.userId],
    addTodo: (text) => dispatchProps.addTodo(ownProps.userId, text)
  })
}
```
### options
配置项，有两个配置可以使用 pure ： 默认为 true 改变状态时，深度对比数据，以便发生不必要的组件更新渲染。withRef ：默认为 false ,缓存 store 以供组件可以通过    getWrappedInstance 获取他，显然官方是不推荐的，因为默认值是 false 。

可能你会觉得 connect 的写法很理解不了，为什么会有两个括号，其实你可以这样理解，第一个括号返回了一个函数，然后第二个括号有直接调用的，也就是自执行函数一个道理。
第一个函数传入对应的 state 和 action 的 dispatch 然后执行完之后，返回的一个函数在传入对应的 Component ,这样就可以让组件就可以使用 props 里的值啦。

自此，React-Redux 就差不多这些啦，有疑问可以留言说哟，只是自己学习的想法，不喜勿喷。
