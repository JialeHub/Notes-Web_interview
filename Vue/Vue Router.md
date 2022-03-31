# Vue Router

## Vue-Router 的懒加载如何实现

非懒加载：

```js
import List from '@/components/list.vue'
const router = new VueRouter({
  routes: [
    { path: '/list', component: List }
  ]
})
```

方案一(常用)：使用箭头函数+**import**动态加载

```js
const List = () => import('@/components/list.vue')
const router = new VueRouter({
  routes: [
    { path: '/list', component: List }
  ]
})
```

方案二：使用箭头函数+**require**动态加载

```js
const router = new Router({
  routes: [
   {
     path: '/list',
     component: resolve => require(['@/components/list'], resolve)
   }
  ]
})
```

方案三：使用webpack的**require.ensure**技术，也可以实现按需加载。 这种情况下，多个路由指定相同的chunkName，会合并打包成一个js文件。

```js
// r就是resolve
const List = r => require.ensure([], () => r(require('@/components/list')), 'list');
// 路由也是正常的写法  这种是官方推荐的写的 按模块划分懒加载 
const router = new Router({
  routes: [
  {
    path: '/list',
    component: List,
    name: 'list'
  }
 ]
}))
```



## 路由的hash和history模式的区别

Vue-Router有两种模式：**hash模式**和**history模式**。默认的路由模式是hash模式。

- **hash模式**：hash模式是开发中默认的模式，它的URL带着一个 **#**

  hash值会出现在URL里面，但是不会出现在HTTP请求中，对后端完全没有影响。所以改变hash值，不会重新加载页面。这种模式的浏览器支持度很好，低版本的IE浏览器也支持这种模式。hash路由被称为是前端路由，已经成为SPA（单页面应用）的标配。

  **原理：** hash模式的主要原理就是**onhashchange()事件**：

  ```js
  window.onhashchange = function(event){
  	console.log(event.oldURL, event.newURL);
  	let hash = location.hash.slice(1);
  }
  ```

  使用onhashchange()事件的好处就是，在页面的hash值发生变化时，无需向后端发起请求，window就可以监听事件的改变，并按规则加载相应的代码。除此之外，hash值变化对应的URL都会被浏览器记录下来，这样浏览器就能实现页面的前进和后退。虽然是没有请求后端服务器，但是页面的hash值和对应的URL关联起来了。

- **history模式**： history模式的URL中没有#，它使用的是传统的路由分发模式，即用户在输入一个URL时，服务器会接收这个请求，并解析这个URL，然后做出相应的逻辑处理。 **特点：** 当使用history模式时，相比hash模式更加**好看**。但是，history模式需要**后台配置支持**。如果后台没有正确配置，访问时会返回404。 **API：** history api可以分为两大部分，切换历史状态和修改历史状态：

  **修改历史状态**：包括了 HTML5 History Interface 中新增的 **pushState()** 和 **replaceState()** 方法，这两个方法应用于浏览器的历史记录栈，提供了对历史记录进行修改的功能。只是当他们进行修改时，虽然修改了url，但浏览器不会立即向后端发送请求。如果要做到改变url但又不刷新页面的效果，就需要前端用上这两个API。

  **切换历史状态：** 包括`forward()`、`back()`、`go()`三个方法，对应浏览器的前进，后退，跳转操作。

  虽然history模式丢弃了丑陋的#。但是，它也有自己的缺点，就是在刷新页面的时候，如果没有相应的路由或资源，就会刷出404来。如果想要切换到history模式，就要进行以下配置（后端也要进行配置）：

  ```js
  const router = new VueRouter({
    mode: 'history',
    routes: [...]
  })
  ```

  两种模式对比：

  调用 history.pushState() 相比于直接修改 hash，存在以下优势：

  - pushState() 设置的新 URL 可以是与当前 URL **同源**的任意 URL；而 **hash** **只可修改 # 后面**的部分，因此只能设置**与当前 URL 同文档的 URL**；
  - pushState() 设置的新 URL **可以与当前 URL 一模一样**，这样也会把记录添加到栈中；而 **hash** 设置的新值必须与原来**不一样才会触发**动作将记录添加到栈中；
  - pushState() 通过 stateObject 参数**可以添加任意类型的数据到记录**中；而 **hash** 只可添加**短字符串**；
  - pushState() 可额外设置 **title** 属性供后续使用。
  - hash模式下，仅**hash**符号之前的url会被包含在请求中，后端如果没有做到对路由的全覆盖，也**不会返回404**错误；**history**模式下，前端的url必须和实际向后端发起请求的url一致，如果没有对用的路由处理，将返回**404错误**。

  hash模式和history模式都有各自的优势和缺陷，还是要根据实际情况选择性的使用。



## 如何获取页面的hash变化

- **监听$route的变化**

  ```
  // 监听,当路由发生变化的时候执行
  watch: {
    $route: {
      handler: function(val, oldVal){
        console.log(val);
      },
      // 深度观察监听
      deep: true
    }
  }
  ```

- **window.location.hash读取#值**

  window.location.hash 的值可读可写，读取来判断状态是否改变，写入时可以在不重载网页的前提下，添加一条历史访问记录。



## `$route 和 $router` 的区别

- $route 是“路由信息对象”，包括 path，params，hash，query，fullPath，matched，name 等路由信息参数
- $router 是“路由实例”对象包括了路由的跳转方法，钩子函数等。



## 如何定义动态路由？如何获取传过来的动态参数？

- **param方式**

  配置路由格式：`/router/:id`，传递的方式：在path后面跟上对应的值，传递后形成的路径：`/router/123`

  ```vue
  //路由定义
  //在APP.vue中
  <router-link :to="'/user/'+userId" replace>用户</router-link>    
  
  //在index.js
  {
     path: '/user/:userid',
     component: User,
  },
    
  //路由跳转
  // 方法1：
  <router-link :to="{ name: 'users', params: { uname: wade }}">按钮</router-link
  // 方法2：
  this.$router.push({name:'users',params:{uname:wade}})
  // 方法3：
  this.$router.push('/user/' + wade)
  ```

- **query方式**

  配置路由格式：`/router`，也就是普通配置，传递的方式：对象中使用query的key作为传递方式，传递后形成的路径：`/route?id=123`

  ```js
  //路由定义
  //方式1：直接在router-link 标签上以对象的形式
  <router-link :to="{path:'/profile',query:{name:'why',age:28,height:188}}">档案</router-link>
  
  // 方式2：写成按钮以点击事件形式
  <button @click='profileClick'>我的</button>    
  
  profileClick(){
    this.$router.push({
      path: "/profile",
      query: {
          name: "kobi",
          age: "28",
          height: 198
      }
    });
  }
  
  //跳转方法
  // 方法1：
  <router-link :to="{ name: 'users', query: { uname: james }}">按钮</router-link>
  // 方法2：
  this.$router.push({ name: 'users', query:{ uname:james }})
  // 方法3：
  <router-link :to="{ path: '/user', query: { uname:james }}">按钮</router-link>
  // 方法4：
  this.$router.push({ path: '/user', query:{ uname:james }})
  // 方法5：
  this.$router.push('/user?uname=' + jsmes)
  
  //获取参数：通过$route.query 获取传递的值
  ```




## Vue-router 导航守卫有哪些

- 全局前置/钩子：beforeEach、beforeResolve、afterEach
- 路由独享的守卫：beforeEnter
- 组件内的守卫：beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave



## 路由钩子在生命周期的体现

- **导航守卫**

  - **全局路由钩子**

    - router.beforeEach 全局前置守卫 进入路由之前

      ```js
      //判断是否登录了，没登录就跳转到登录页
      router.beforeEach((to, from, next) => {  
          let ifInfo = Vue.prototype.$common.getSession('userData');  // 判断是否登录的存储信息
          if (!ifInfo) { 
              // sessionStorage里没有储存user信息    
              if (to.path == '/') { 
                  //如果是登录页面路径，就直接next()      
                  next();    
              } else { 
                  //不然就跳转到登录      
                  Message.warning("请重新登录！");     
                  window.location.href = Vue.prototype.$loginUrl;    
              }  
          } else {    
              return next();  
          }
      })
      ```

    - router.beforeResolve 全局解析守卫（2.5.0+）在 beforeRouteEnter 调用之后调用

    - router.afterEach 全局后置钩子 进入路由之后

      ```js
      router.afterEach((to, from) => {  
          // 跳转之后滚动条回到顶部  
          window.scrollTo(0,0);
      });
      ```

  - **单个路由独享钩子**

    - beforeEnter ：如果不想全局配置守卫的话，可以为某些路由单独配置守卫

      ```js
      export default [    
          {        
              path: '/',        
              name: 'login',        
              component: login,        
              beforeEnter: (to, from, next) => {          
                  console.log('即将进入登录页面')          
                  next()        
              }    
          }
      ]
      ```

  - **组件内钩子**

    - beforeRouteEnter：进入组件前触发，beforeRouteEnter组件内还访问不到this，因为该守卫执行前组件实例还没有被创建，需要传一个回调给 next来访问

      ```js
      beforeRouteEnter(to, from, next) {      
          next(target => {        
              if (from.path == '/classProcess') {          
                  target.isFromProcess = true        
              }      
          })    
      }
      ```

    - beforeRouteUpdate：当前地址改变并且改组件被复用时触发

    - beforeRouteLeave：离开组件被调用

    

- **生命周期函数 完整路由导航解析流程**

  - **导航被触发**。
  - 在失活的组件里调用 `beforeRouteLeave` 守卫。
  - 调用全局的 `beforeEach` 守卫。
  - 在重用的组件里调用 `beforeRouteUpdate` 守卫。
  - 在路由配置里调用 `beforeEnter`。
  - **解析异步路由组件**。
  - 在被激活的组件里调用 `beforeRouteEnter`。
  - 调用全局的 `beforeResolve` 守卫(2.5+)。
  - **导航被确认**。
  - 调用全局的 `afterEach` 钩子。
  - **触发 DOM 更新**。
  - 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。



## Vue-router跳转和location.href有什么区别

- 使用 `location.href= /url `来跳转，简单方便，但是刷新了页面；
- 使用 `history.pushState( /url )` ，无刷新页面，静态跳转；
- 引进 router ，然后使用 `router.push( /url )` 来跳转，使用了 `diff` 算法，实现了按需加载，减少了 dom 的消耗。其实使用 router 跳转和使用 `history.pushState()` 没什么差别的，因为vue-router就是用了 `history.pushState()` ，尤其是在history模式下。



## params和query的区别

**用法**：query要用path来引入，params要用name来引入，接收参数都是类似的，分别是 `this.$route.query.name` 和 `this.$route.params.name` 。

**url地址显示**：query更加类似于ajax中get传参，params则类似于post，说的再简单一点，前者在浏览器地址栏中显示参数，后者则不显示

**注意**：**query刷新不会丢失**query里面的数据 **params刷新会丢失** params里面的数据。



## 对前端路由的理解

在前端技术**早期**，**一个 url 对应一个页面**，如果要从 A 页面切换到 B 页面，那么必然伴随着页面的刷新。这个体验并不好，不过在最初也是无奈之举——用户只有在刷新页面的情况下，才可以重新去请求数据。后来，改变发生了——**Ajax** 出现了，它允许人们在不刷新页面的情况下发起请求；与之共生的，还有“**不刷新页面即可更新页面内容**”这种需求。在这样的背景下，出现了 **SPA（单页面应用**）。SPA极大地提升了用户体验，它允许页面在不刷新的情况下更新页面内容，使内容的切换更加流畅。但是在 SPA 诞生之初，人们并没有考虑到“定位”这个问题——在内容切换前后，页面的 URL 都是一样的，这就带来了**两个问题**：

- SPA 其实并不知道当前的页面“**进展**到了哪一步”。可能在一个站点下经过了反复的“前进”才终于唤出了某一块内容，但是此时只要刷新一下页面，一切就会被清零，必须重复之前的操作、才可以重新对内容进行定位——SPA 并不会“记住”你的操作。
- 由于有且仅有一个 URL 给页面做映射，这对 **SEO 也不够友好**，搜索引擎无法收集全面的信息

为了解决这个问题，**前端路由**出现了。前端路由可以帮助我们在仅有一个页面的情况下，**“记住”用户当前走到了哪一步**——为 SPA 中的各个视图匹配一个唯一标识。这意味着用户前进、后退触发的新内容，都会映射到不同的 URL 上去。此时即便他刷新页面，因为当前的 URL 可以标识出他所处的位置，因此内容也不会丢失。那么如何实现这个目的呢？首先要解决两个问题：

- 当用户**刷新页面**时，浏览器会默认根据当前 URL 对资源进行**重新定位**（发送请求）。这个动作对 SPA 是不必要的，因为我们的 SPA 作为单页面，无论如何也只会有一个资源与之对应。此时若走正常的请求-刷新流程，反而会使用户的前进后退操作无法被记录。
- 单页面应用对服务端来说，就是一个URL、一套资源，那么如何做到**用“不同的URL”来映射不同的视图内容**呢？

从这两个问题来看，服务端已经完全救不了这个场景了。所以要靠咱们前端自力更生，不然怎么叫“前端路由”呢？作为前端，可以提供这样的解决思路：

- **拦截用户的刷新操作(跳转刷新)**，避免服务端盲目响应、返回不符合预期的资源内容。把刷新这个动作完全放到前端逻辑里消化掉。
- **感知 URL 的变化**。这里不是说要改造 URL、凭空制造出 N 个 URL 来。而是说 URL 还是那个 URL，只不过我们可以给它做一些微小的处理——这些处理并不会影响 URL 本身的性质，不会影响服务器对它的识别，只有我们前端感知的到。一旦我们感知到了，我们就根据这些变化、用 JS 去给它生成不同的内容。

