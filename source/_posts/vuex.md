---
title: VUEX
date: 2022-09-04 21:36:22
description: 状态管理模式 + 库 Vuex 与 Pinia
categories:
- VUE
tags:
- VUE
---

***

**<font color=ddoodd size=6>VUEX</font>**

***

`Vuex` 是一个为 `Vue.js` 应用程序开发的**状态管理模式 + 库**，采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化

可以这样回答：`Vuex` 是一种状态管理模式库，存在的目的是**共享可复用的组件状态**

通过 `Vue` 插件系统将 `store` 实例从根组件中注入到所有的子组件里，子组件可通过 `this.$store` 访问，

在 `Vue3` 中：

```javascript
import { useStore } from 'vuex'

const store = useStore()
```

**优点：**

在 `vuex` 中集中管理共享的数据，易于开发和后期的维护
在 `vuex` 中的数据是**响应式**的，能够实时的保持数据与页面的同步

**`State` :**  保存着共有数据，数据是响应式的
**`Getter` :**  从 `state` 中派生出一些状态
**`Mutations` :**  更改 `store` 中的状态的**唯一方法**是**提交 `mutation` ，必须同步**
**`Actions` :**  在 `action` 中提交的是 `mutation`，可以包含任意**异步**操作
**`Modules` :**  将 `store` 分割成模块	

## State
保存着共有数据，数据是响应式的

```javascript
import { createStore } from 'vuex'
export default createStore({
    state: {
        name: 'jashin',
        age: '24',
        hobby: 'go'
    }
})

// mapState
// Vue2:
import { mapState } from 'vuex'
computed: {
    ...mapState({
        myName: state => state.name,		// Or: myName: name,
        myAge: state => state.age,			// Or: myAge: age,
        myHobby: state => state.hobby		// Or: myHobby: hobby,
    })
}
// Vue3:
import { computed } from 'vue'
import { mapState, useStore } from 'vuex'
export default {
    setup() {
        const store = useStore()
        const statesFunc = mapState(['name','age','hobby'])
        const storeState = {}
        Object.keys(statesFunc).forEach(item => {
            // vuex mapState 中使用的是 this.$store, 要更改 this 绑定
            const func = statesFunc[item].bind({ $store: store })
            storeState[item] = computed(func)
        })
        return { ...storeState }
    }
}
```
## Getter
可以认为是 `state` 的计算属性，有时需要从 `state` 中派生出一些状态，例如对列表进行过滤并计数
```javascript
import { createStore } from 'vuex'
export default createStore({
    state: {
        todos: [{ id: 1, text: '...', done: true },
                { id: 2, text: '...', done: false }]
    },
    getters: {
        doneTodos(state) {
            return state.todos.filter(todo => todo.done)
        },
        doneTodosCount (state, getters) {
            return getters.doneTodos.length
        },
        getTodoById: (state) => (id) => {
            return state.todos.find(todo => todo.id === id)
        }
    }
})
// 使用
store.getters.doneTodos 				// [{ id: 1, text: '...', done: true }]
store.getters.doneTodosCount 		// 1
store.getters.getTodoById(1) 		// { id: 1, text: '...', done: true }

// mapGetter
// Vue2
import { mapGetters } from 'vuex'
export default {
    computed: {
        ...mapGetters(['doneTodos','doneTodosCount'])
        // OR
        ...mapGetters({ todos: 'doneTodos', count: 'doneTodosCount' })
    }
}
// Vue3
import { computed } from 'vue'
import { mapGetters, useStore } from 'vuex'
export default {
    setup() {
        const store = useStore()
        const gettersFunc = mapGetters(['doneTodos','doneTodosCount'])
        const storeGetters = {}
        Object.keys(gettersFunc).forEach(item => {
            const func = gettersFunc[item].bind({ $store: store })
            storeGetters[item] = computed(func)
        })
        return { ...storeGetters }
    }
}
```
## Mutations
更改 `store` 中状态唯一方法是 `mutation`，通过 `commit` 提交，方法必须**同步，**每个 `mutation` 都有一个字符串的事件类型 `type` 和一个回调函数 `handler`
```javascript
import { createStore } from 'vuex'
export default createStore({
    state: { count: 0 },
    mutations: {
        increment_one (state) {
            state.count++
        },
        increment_two (state, n) {
            state.count += n
        },
        increment_three (state, payload) {
            state.count += payload.amount
        }
    }
})
// 使用
store.commit('increment_one')
store.commit('increment_two', 10)
store.commit('increment_three', { amount: 10 })
// OR
store.commit({ type: 'increment_three', amount: 10 })

// mapMutations
methods: {
    ...mapMutations(['increment', 'increment_two', 'increment_three']),
    // OR
    ...mapMutations({ add: 'increment' })
}
this.add()
this.increment()
this.increment_two(10)
this.increment_three({ amount: 10 })
```
**常量常量替代 `Mutation` 事件类型：**

```javascript
// kokomi.js
export const MY_MUTATION = 'MY_MUTATION'

// store.js
import { createStore } from 'vuex'
import { MY_MUTATION } from './kokomi'
export default createStore({
    state: {},
    mutations: {
        // 我们可以使用 ES2015 风格的计算属性命名功能
        // 来使用一个常量作为函数名
        [MY_MUTATION] (state) { }
  }
})
```
## Actions
类似于 `mutation`，但 `action` 提交的是 `mutation`，而不是直接变更状态，可以包含任意**异步**操作
通过 `store.dispatch` 来分发 `actions`，异步的方法写在 `actions` 中

```javascript
import { createStore } from 'vuex'
export default createStore({
    state: { count: 0 },
    mutations: {
        increment (state) {
            state.count++
        },
        increment_two (state, payload) {
            state.count += payload.amount
        }
    },
    actions: {
        increment (context) {
            context.commit('increment')
        },
        incrementAsync ({ commit }, payload) {
            setTimeout(() => { commit('increment_two', payload) }, 1000)
        }
    }
})

// 使用
store.dispatch('increment')
store.dispatch('incrementAsync', { amount: 10 })
store.dispatch({ type: 'incrementAsync', amount: 10 })

// Promise Async Await
action_promise ({ commit }) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            commit('mutation')
            resolve()
        }, 1000)
    })
}
async action_async ({ commit }) {
    commit('mutation', await do_something())
}
// 使用
store.dispatch('action_promise').then(() => { ... })
```
## Modules
将 `store` 分割成模块，每个模块拥有自己的 `state` `mutation` `action` `getter`
```javascript
const moduleA = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... },
    getters: { ... }
}
const moduleB = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... }
}
const store = createStore({
    modules: {
        a: moduleA,
        b: moduleB
    }
})
// 使用
store.state.a 	// moduleA 的状态
store.state.b 	// moduleB 的状态
```
**注：**`getter` 中，根节点状态作为第三、四个参数**：`(state, getters, rootState, tootGetters)`**
**注：**`actions` 中局部状态为 `context.state`，根节点状态为 **`context.rootState`**
**注：**不要在不同的、无命名空间的模块中定义两个相同的 `getter`
**注：**可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块
**注：**全局分发 `action` 或 `mutation`，将 `{ root: true }` 作为第三参数传给 `dispatch` 或 `commit`
**注：**可将模块空间名称作为第一个参数传递给辅助函数 `mapState / Getter / Mutation / Action`
**注：**在 `store` 创建之后，你可以使用 `store.registerModule` 方法动态注册模块

```javascript
const store = createStore({ })
store.registerModule('newModule', { })
```
**注：**可使用 `createNamespacedHelpers` 创建基于某个命名空间的辅助函数

```javascript
import { createNamespacedHelpers } from 'vuex'
const { mapState, mapActions, ... } = createNamespacedHelpers('namespace')
```
**注：**模块重用，使用一个函数来声明模块状态

**流程：**

页面通过 `store.dispatch` 提交事件到 `action` 或 `mutation`，`action` 异步操作之后通过 `commit` 把对应参数同步提交到  `mutation`，`mutation` 修改 `state` 中对应的值，最后通过 `getter` 把对应值抛出

## PINIA
`Pinia` 也是一个 `Vue` 状态管理工具，与 `Vuex` 有很多相似之处

`Pinia` 与 `Vuex` 相比：

- `Pinia` 去除了 `Vuex` 对于 `Mutations` 与 `Actions` 的区分，在 **`Actions`** 中便能够使用同步和异步方法
- `Pinia` 对于 `Typescript` 的支持性更好，友好的 `devTools` 支持，简化了很多方法的写法
- `Vuex` 比较完善，比较适合大型项目，`Pinia` 更加适合小型项目
***
```javascript
import { defineStore } from 'pinia'

export const caomei_store = defineStore('caomei', {
    state: () => ({ name: 'jashin' }),
    getters: { },
    actions: {
        deleteName() {
            this.name = null
        }
    },
})

// 使用
import { userStore } from '...'
import { mapActions } from 'pinia'

const store = userStore()
store.name
store.deleteName()
// OR 注册到 methods 中
...mapActions(userStore, ['deleteName'])
...mapActions(useCounterStore, { deleteUser: 'deleteName' })	// 别名
```

