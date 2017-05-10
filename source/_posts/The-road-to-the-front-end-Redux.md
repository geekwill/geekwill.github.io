---
title: 前端沉淀之路 - Redux
date: 2017-05-10 15:13:18
tags: Redux React
---
该来的总会来的，前段时间看了ReactJS，还没来得及记录学习的笔记呢（有时间补上），就该到了数据流的处理，今天的主角：Redux，最近火的不要不要的单向数据流框架，知道他的存在也同样是因为React。话说每学一个东西之前，是不是都需要先知道这东西是干什么的，或者说他存在的目的，同样我在学之前我也弄清楚Redux是干啥的，能干啥？
>_Redux 是 JavaScript 状态容器，提供可预测化的状态管理。_

对，这句话是官方中文文档介绍的第一句话，其实，我看这就话还是很不懂（不知道多少人跟我一样🐷），所以我有google一下，懂了但最后又不懂啦，有没有绝世武功快要炼成的感觉。
很幸运的是，我找到了一个更通熟易懂的介绍，来之知乎的 @Wang Namelos 大神，感谢你，[如果通熟易懂的理解Redux](https://www.zhihu.com/question/41312576?sort=created)，里边是结合React来讲的Redux，也正是我想要的，因为我就是把Redux用来配合React的，很推荐大家也看看。

<!-- 开始之前，我们需要一个实例项目，所以我图简单，就使用了React的[create-react-app](https://github.com/facebookincubator/create-react-app)新建一个React项目，然后调整一下我们需要的结构，项目我会放到GitHub上的，下载可以直接执行。

有了项目，那我们接下来就来慢慢拆分理解它们。 -->
<!-- more -->

### Action
先来看下官方的定义
> + Action 是把数据从应用传到 store 的有效载荷。它是 store 数据的唯一来源。
> + Action 本质上是 JavaScript 普通对象。我们约定，action 内必须使用一个字符串类型的 type 字段来表示将要执行的动作。

这两句话很好理解，action是用来操作数据的来源，换句话说就是应用的所有数据都应该通过action这个普通的对象去操作，那我就会问，你说action操作数据，那它怎么操作的？我那么多数据，我怎么知道操作那个啊？所有第二句话就给了答案，action就是个普通的对象，但是约定的这个对象里必须要有一个type这个字段，就是用来标识着，你要操作哪条数据，或者哪个动作。

``` js
function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}
```
以上代码很容易看明白，一个普通的函数，接受一个text字段，这个字段可以是任何类型的数据，同时也可以有多个，最主要的是retrun返回的对象才是我们真正需要的action，里边的type也是必须的，告诉我们，我这里是用来操作增加一条todo数据的，这些也都应正我们之前说的。那return出来的对象action，有啥作用，难道就这样就操作数据啦？不慌不慌，接下来就是reducer啦。

### Reducer
惯例，先看看官方的定义
> + Action 只是描述了有事情发生了这一事实，并没有指明应用如何更新 state。而这正是 reducer 要做的事情。

咱先别管啥state，看这句话，很明白的，告诉我们，action告诉我们，我要操作某条数据，比如：我要增加一条todo，那我要怎么操作呢？操作成啥样子呢？所以reducer是来解决疑问的，那怎么操作，操作成啥样就需要reducer来告诉我们啦。同样的，reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。这里需要提醒下，保持 reducer 纯净非常重要。永远不要在 reducer 里做这些操作：
+ 修改传入参数
+ 执行有副作用的操作，如 API 请求和路由跳转；
+ 调用非纯函数，如 Date.now() 或 Math.random()

至于为什么，这里官方给出的是：之所以称作 reducer 是因为它将被传递给 [Array.prototype.reduce(reducer, ?initialValue)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce?v=example) 方法。保持 reducer 纯净非常重要，Array.prototype.reduce是什么意思，这个这里不说啦，自行MDN吧。

``` js
//这里初始化state
const initialState = []

function todos(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return [
          ...state.todos,
          {
            text: action.text,
            //这里默认completed为false
            completed: false
          }
        ]
    default:
      return state
  }
}
```

上边代码使用了ES6的语法，如果看不懂自行google，推荐雪峰大神的书[ECMAScript 6 入门](http://es6.ruanyifeng.com/)，我有买字纸的书，代码描述了初始化了一个空的todos数组作为旧state结构，然后通过获取action里type判断操作类型，即增加一个todo，然后通过新建一个数组并且合并旧state里的数据来达到不能修改传入参数的目的，同时也将操作的新todo的数据也合并其中，最后如果没有任何何用操作，返回旧的state，这一步很重要，以上就是一个很简单的reducer，前边说啦，action是告诉我们操作，现在reducer也操作成我想要的数据啦，那再问？难道这样操作完了就没事啦？都知道，redux是把数据放在store里啦，而且文章之前也说啦，那现在reducer操作完啦，也没看见store啊？是啊，在学之前，我这里也很不明白，那现在就说Store在哪？怎么来的？

### Store
惯例
> + 在前面的章节中，我们学会了使用 action 来描述“发生了什么”，和使用 reducers 来根据 action 更新 state 的用法。
> + 再次强调一下 Redux 应用只有一个单一的 store。当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store。

我们先来看看store创建的代码：
``` js
import { createStore } from 'redux'
import todoApp from './reducer/todoApp.js'
let store = createStore({todoApp})
```
从代码可以看出，createStore方法是redux暴露给我们的，用来创建store的，但是参数，咦...为啥参数是我们自己写的reducer呢？现在似乎明白了些什么，原来怪不得reducer可以操作数据呢，原来store数据容器就是他造出来的，这样就很明白为什么reducer能操作store啦，但是在我自己写demo的时候，我遇到一个问题？就是store里的数据是啥样的？都不知道啥样的，我在reducer里也没法获取数据啊？其实这个很简单，我们可以把store想象成一个空的容器，然后里边用来放我们reducer操作的所以数据。比如：我们add_todo里init的state是这样的：
``` js
const initialState = []
```
那其实我们那个大容器里边就是这样的：
``` js
{
  todos: []
}
```
那如果有多个reducer，操作不一样的多种数据怎么办？这其实也好办，redux都替我们想好啦，给我们暴露了一个叫combineReducers的方法，比如：我们还有一个筛选的操作呢。
``` js

const initialState = "SHOW_ALL";

function visibilityFilter(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}
```
上边代码的意思，我们还有一个筛选todos的操作，根据传入的字符串，判断是否显示全部的数据，那要是这样的话，store容器里的数据格式又是什么样子呢？那就需要我们使用combineReducers方法啦，用法很简单，其意思就是合并reducer而且。
``` js
import { combineReducers } from 'redux';

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

```
上边代码的意思是不是有点像把我们写的两个reducer放在了一个对象容器里边呢，然而就是这样的，其实在store里格式就是这样的：
``` js
  {
    visibilityFilter: 'SHOW_ALL',
    todos: []
  }
```
有木有很熟悉，其实就是将我们初始化的两个state放在了一个对象容器里，好吧，你知道了吧，那我们在从reducer取数据的时候是不是就明白了很多。
好吧，看样子是否已经大功告成啦，但是还有一个问题，那就是我们咋调用action去执行操作啊？那我们先看看官方对store的职责介绍：
> Store 有以下职责：
> + 维持应用的 state
> + 提供 getState() 方法获取 state
> + 提供 dispatch(action) 方法更新 state
> + 通过 subscribe(listener) 注册监听器
> + 通过 subscribe(listener) 返回的函数注销监听器

1，2就不说啦，看着就明白啦，我们先看第三个，提供 dispatch(action) 方法更新 state，dispatch的参数就是我们的action啦，比如我们增加todo的操作：
``` js
store.dispatch(addTodo('Learn about actions'))
```
然而subscribe就是创建一个监听，可以监听state的改变状态，比如：
``` js
//创建监听，store改变时，输出state的值
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
)

// 发起一系列 action
store.dispatch(addTodo('Learn about actions'))

// 停止监听 state 更新
unsubscribe();

```
以上个人理解，或许对你有帮助，接下来我还会记录react的学习，及配合react-redux，我会将demo整理放到github上。
