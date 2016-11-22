---
title: Redux初试
date: 2016-07-11 17:40:07
categories: redux
tags:
    - redux
    - reactjs
---

### 什么是redux？
> Redux is a predictable state container for JavaScript apps.

翻译过来：
> Redux 是 JavaScript 状态容器，提供可预测化的状态管理。

什么是状态容器，我的理解是类似于reactjs的state容器，即存储数据结构的对象，可以通过更改容器里的数据，实现更改页面的显示。为什么是可预测的状态管理？这就归功于redux的单向数据流模式啦。
<!-- more -->
什么叫做单向数据流，说白话就是（来了，弄完了，走了，在也没有啦！）有没有一夜情的赶脚，然而，redux也是，但你需要改变状态时，你必须使用action来指挥我，只有action说话啦，我在决定听不听你的，这个听从指挥的就是reducer，如果你action指挥reducer想找个女朋友的话，action需要告诉reducer要什么样的，比如你想要个漂亮的，那reducer就会去找一个（女朋友是不可能有重复了哟，如果真有这事，那全世界的男人会爽死的，你懂的），有的时候你的要求很多的话，那就需要分别把需求告诉其他的reducer，这样就可以更快的找到啦，如果reducer没找到的话，他就会把你默认的原配在给你（好吧，叹气中...），然后有人就会想？reducer到哪找去啦？好吧，其实reducer有一手的，因为他跟store是兄弟，他跟store他俩找去啦。其实吧，reducer跟store（在一起的话）肯定会找到的。

#### Action
action即是需要找女朋友的那个『物体』，在代码的实现中，本身为普通函数，需返回必须带有type属性的对象，这个type参数的作用，即是告诉reducer找什么样子的，也可以把你的需求，即想要个漂亮的，性感的，25岁以下的，通过参数的方式，跟type数据合并返回即可。

```js
export function addTodo( text ){
    return {
        type: types.ADD_TODO,
        text
    }
}
```

#### Reducer
用于处理action发来的需求，分发给不同的操作和需求，reducer也是通过普通函数创建，接受两个必要的参数，state，action，state: 即此次操作默认的状态，action: 本次操作的需求，即action传过来的数据，可以通过type属性，处理不同的操作，也可获取个性需求，即action传过来的漂亮的，性感的，然后找到对应的女朋友，（不管什么样的女朋友都是不可重复的，哪怕是双胞胎可是两个不一样的『物体』哟），所以，你需要每次返回一个新的状态对象（如何返回不一样的，可以google，或者等待我下次再说）。对于知道java的来说，有木有mvc的contraller的赶脚。切记哟，如果没有找到需要的，一定要把原配（默认的state状态）给返回去哟，要不就光棍啦！

```js
function(state = {}, action){
    switch(action.type){
        case types.ADD_TODO:
            return Object.assign({},state, {
                text: action.text
            })
        case types.OPERATION_TODO:
            return Object.assign({},state, {
                text: action.text
            })
        default:
            return state;
    }
};
```

实际项目可能有多个reducer，可以使用combineReducers合并。

```js

import { combineReducers } from 'redux';
import todo from './todo';
import todoList from './todoList';

export default combineReducers({todo, todoList});

```

#### Store

到这里，可能会有人问？你说action告诉reducer要找女朋友，那他是怎么告诉他的？好吧，这就是store啦，他就是个中介，跟reducer一伙，但有可以骚扰action，因为只有他才能通过促使action去告诉reducer，但是这个中介又是怎么来的？他是通过强迫reducer出来的（你懂的），通过createStore，传入reducer，和默认state，创造出来的，使用store.dispatch({ type: 'ADD_TODO' })让action告诉reducer想要什么样的。

```js
import { createStore } from 'redux';
import todoReducers from '../reducers';

createStore(todoReducers, initialState = {});

```

到此，是不是有种action去要求reducer找女朋友然后又造出个store去驱动action找女朋友的。

![](/images/2016-07-11/reducer.png)

好吧，问题又来啦，现在假如性感的，漂亮的女朋友找到啦，store把找到的女朋友给放到state里边啦，我怎么去拿的女朋友爽爽啊，如果拿不到，那就然并卵啦。其实这个你可以去找store啊，因为是他放进去的，自然要找他喽，然后store就提供了一个监听的方法：

```js
store.subscribe(() =>
  console.log(store.getState())
);

```

这样的话，就完美啦，只要store把新的女朋友放到state里边的时候，就会告诉我通过store.getState()，去取啦。然后，下边就很污啦......
