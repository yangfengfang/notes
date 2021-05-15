# 

# 「修饰符」

修饰符是由点开头的指令后缀来表示的。

## 事件修饰符

1. `.stop `阻止单击事件继续传播

```html
<a v-on:click.stop="doThis"></a>
```

2.`.prevent` 提交事件不再重载页面

```html
<form v-on:submit.prevent="onSubmit"></form>
```

3.修饰符可以串联

```html
<a v-on:click.stop.prevent="doThat"></a>
```

4.



- `.capture`
- `.self`
- `.once` 点击事件将只会触发一次
- `.passive`

[`.lazy`](https://cn.vuejs.org/v2/guide/forms.html#lazy) - 取代 `input` 监听 `change` 事件

[`.number`](https://cn.vuejs.org/v2/guide/forms.html#number) - 输入字符串转为有效的数字

[`.trim`](https://cn.vuejs.org/v2/guide/forms.html#trim) - 输入首尾空格过滤

# 生命周期



- created: 当页面创建时，回调
- mounted: 当页面加载完成时回调
- updated: 当页面刷新时回调

# 组件

## 非父子组件间传值

1.使用一个中转站如单独的事件中心来管理组件间的通信

```js
var eventHub = new Vue();
```

2.监听事件与销毁事件

```js
eventHub.$on('add-todo', addTodo) // on用于监听事件
eventHub.$off('add-todo') // off用于销毁事件
```

3.触发事件

```js
eventHub.$emit('add-todo', id)
```

emit第一个参数是事件的名称，第二个参数是要传递的实参

实例：

<a href='./vue-html/组件开发/03非父子组件间传值.html'>运行</a>

```js
 <div id="app">
     <button v-on:click="handel">销毁事件</button>
    <test-tom></test-tom>
    <test-jerrt></test-jerrt>
</div>
<script src="../js/vue.js"></script>
<script>
    /**
         * 非父子组件数组交互
         */
    // 定义事件中心
    const hub = new Vue();

// 定义两个兄弟组件
Vue.component('test-tom', {
    data: function() {
        return {
            num: 0,
        }
    },
    template: `
    <div>
    <div>TOM:{{num}}</div>
    <div><button @click="handel">点击</button></div>
    </div>        
    `,
    methods: {
        handel: function() {
            // 触发事件,触发对方的事件
            hub.$emit('jerrt-event', 1)
        }
    },
    // 事件监听
    mounted: function() {
            hub.$on('tom-event', (val) => {
                this.num += val;
            }); // 添加监听事件，监听自己的事件
        }
    });
    Vue.component('test-jerrt', {
        data: function() {
            return {
                num: 0,
            }
        },
        template: `
    <div>
    <div>JERRT:{{num}}</div>
    <div><button @click="handel">点击</button></div>
    </div>        
    `,
    methods: {
        handel: function() {
            // 触发对test-tom的监听事件
            hub.$emit('tom-event', 1);
        },
    },
    // 监听事件
    mounted: function() {
        // 添加事件监听
        hub.$on('jerrt-event', (val) => {
            this.num += val;
        });
    }
});
const app = new Vue({
    el: "#app",
    data: {},
    methods: {
        handel: function() {
            // 销毁事件
            hub.$off('tom-event');
            hub.$off('jerrt-event');
        }
    }
});
</script>
```

# vue-router

## 路由跳转

- route-link 

- 事件跳转

  ```js
  methods: {
      onClick() {
      	this.$router.push('path')
      }
  }
  ```

## 参数传递

- 动态路由(params)

  ```
  const myRoutes = [
      {
          path: '/user_create/:id',
          component: UserCreate,
          name: '添加用户',
      }
  ];
  ```

  接受参数

  $route.params 接受path参数

  ```js
  $route.params.id
  ```

- query传参

  ```js
  一、router-link
  <router-link :to="{paht: 'user', query: {id: 1,parmas...}}">
  
  二、方法：
  methods: {
      onclick() {
          this.$router.push({
              path: 'path',
              query: {
                  params:...
              }
          });
      }
  }
  ```

  获取参数：

  ```
  $route.query // 获取全部
  $route.query.params
  ```

  ## 路由模式

  在实例中加入mode属性

  ```js
const router = new VueRouter({
      routes: myRoutes,
      mode: 'history', // 改变路由模式
  });
  ```
  
  ## 导航守卫

  可以在跳转路由时，监听

  router.beforeEach()

  实现跳转路由时，修改页面的标题

  ```js
  const routes = [
        {
          path: '/user_create',
          component: UserCreate,
          name: '添加用户',
          meta: {
              title: '添加用户', // 定义路由标题
          },
      }
    ];
  
  const router = new VueRouter({
      routes,
      mode: 'history', // 改变路由模式
  });
  
  // 导航守卫
  router.beforeEach((to, from, next)=>{
      // 修改页面title
      // to: 从from跳转到to
      document.title = to.matched[0].meta.title;
      next(); // 下一页
  });
  ```

# vuex状态管理

安装

```node
npm install vuex --save
```

作用：

​	可以用来定义全局变量，实现数据响应；管理多个组件共同属性。

## vuex实例

```js
// 引入插件
import Vue from 'vue'
import Vuex from 'Vuex'

// 安装插件
Vue.use(Vuex);

// 创建对象
const store = new Vuex.Store({
    // 定义方法
    // 状态（属性）
    state: {}, 
    // 修改状态
    mutations: {},
    // 异步操作
    actions: {},
    getters: {},
    modules: {},
});

export default store;
```

操作store状态

- 通过this.$store.state.属性（$store.state.属性） 的方式来访问状态
-  通过this.$store.commit('mutation中的方法')来修改状态

```js
// 通过this.$store.state.属性 的方式来访问状态
const store = new Vuex.store({
    // 定义方法
    // 状态（属性）
    state: { count: 0}, 
    // 修改状态
    mutations: { add (state) {this.count++}, decre (state) { this.count--}},
    // 异步操作
    actions: {},
    getters: {},
    modules: {},
});


// 通过this.$store.commit('mutation中的方法')来修改状态
const vm = new Vue({
    methods: {
        addStoreCount () {
            this.$store.commit('add');
        },
    }
});
```

### Vuex核心概念

- state

  ​	单一状态树，vue推荐使用一个store来管理全部的数据元。

- getters

  ​	getters和计算属性类似。每次操作属性时需要处理该属性，可以用getters来集中处理。

  getters方法第一个参数为state，第二个参数为getters

  实例

  ```js
  const store = new Vuex.store({
      // 定义方法
      // 状态（属性）
      state: { count: 0}, 
      // 修改状态
      mutations: {},
      // 异步操作
      actions: {},
      getters: { 
          // getters方法第一个参数为state，第二个参数为getters
          addCount(state) {
            	return count+10;
          },
          chengCount(state, getters) {
              return getters.addCount() * 10;
          }
      },
      modules: {},
  });
  
  // 使用
  $store.getters.chengCount
  ```

  getters传参：

  ​	用于getters方法第一个参数为state，第二个参数为getters都是固定的，可以在方法中返回一个函数来对属性进行修改

  ```js
  const store = new Vuex.store({
      // 定义方法
      // 状态（属性）
      state: { count: 0, age: 12}, 
      // 修改状态
      mutations: {},
      // 异步操作
      actions: {},
      getters: { 
          // getters方法第一个参数为state，第二个参数为getters
          addCount(state) {
            	return count+10;
          },
          chengCount(state, getters) {
              return getters.addCount() * 10;
          },
          addAge(state, getters) {
              return function(param) {}
                  return state.age + param;
              }
          }
      },
      modules: {},
  });
  
  
  //调用
  $store.getters.addAge(10);
  ```

  

- mutations

  风格一：

  this.$store.commit('mutation中的方法'，payload)

  payload传递参数

  ```js
  
  this.$store.commit('action', count)
  ...
  mutations: {
      action(state, count)
  }
  ...
  count == count
  ```

  

  风格二：

  this.$store.commit({

  ​	type: action,

  ​	payload,

  })

  payload={};

  ```js
  this.$store.commit({
      type: action,
      payload: 0,
  })
  ...
  mutations: {
      action(state, payload)
  }
  ...
  payload== {payload:0}
  ```

  

- action

  Action 类似于 mutation，不同在于：

  - Action 提交的是 mutation，而不是直接变更状态。
  - Action 可以包含任意异步操作。

​       Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。

```js
actions: {
    [AUPDATE_USER_DATA](context, payload) {
        return context.commit(UPDATE_USER_DATA, payload);
    }
},
```

分发 Action

Action 通过 `store.dispatch` 方法触发：

```js
this.$store.dispatch(AUPDATE_USER_DATA, user);
```



- module

# axios



### 配置信息



实例

```js
// 创建实例
const isntance = axios.create({
    baseURL: url,
    timeout: 5000,
});
```

使用

```js
instance({
    url: '/home/data',
    params: {},
}).then((response)=>{}, (error) =>{});
```

## 封装axios

```js
export function request(config) {
    const instance = axios.request({
        // 配置
    });
    return instance(config);
}
```

# 循环

```js
var array = [1, 2, 3, 4]
//方法一：
for( var i in array):
console.log(array[i], i)

//方法二：
array.forEach((v, i) => {
console.log(v, i)
})
```

