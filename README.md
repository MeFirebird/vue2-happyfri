##  项目学习    forked  from   <a href="https://github.com/bailicangdu/vue2-happyfri">baillicangdu/vue2-happyfri </a>   


### 待处理问题：
1.App.vue居然没被main.js引入 render？或者tempalte   why呢？  
2.vuex的那几个辅助函数   比如action啊什么的    自带的参数是什么，你要搞清楚啊







### 问题二
vue-happyfri项目代码         总结处理完毕后：上传github吧！！！  ------> webpack官网的实践（代码分割 组件异步加载）

问题：  vuex   vue-router

1.history  hash模式？　
  hash模式：hash就是url的#后面的东东，这个改变时候，浏览器是不会重新加载的
  history模式： 没有#

  补充：history API

2.webpack代码分割       路由被访问的时候才加载对应组件  路由组件的懒加载:Vue异步组件  webapck代码分割
  (string array:回调函数执行要依赖的模块， 回调函数，  模块名 )
  require.ensure(dependencies: String[], callback: function(require), errorCallback: function(error), chunkName: String)


3.vue异步组件： 将组件定义为一个工厂函数，组件需要渲染时候触发工厂函数，然后结果缓存。

4.mapState  mapActions  辅助函数




### 总结：
一：vue-router tips



二：vuex  和  global event bus

1.state:  驱动应用的数据源
  view:   声明方式将state映射到视图
  action: 响应在 view 上的用户输入导致的状态变化。
  store：容器，存储state
         特点： store中的state(data)是响应式的    改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation
               store对象创建： new Vuex.store({ state mutations})
               mutation 必须是同步函数
  mapState 辅助函数帮助我们生成计算属性     mapState 函数返回的是一个对象
  getters（computed）：store中的state派生一些状态， 全局的，因为是在store对象中
  mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性，局部的，因为在某一个组件中


2.为什么提出vuex？
答： 单向数据流的问题： 
    a.多个view依赖同一个state      b.不同view的需要变更同一个状态，不同view的methods都有一个变更此状态的方法了，所以你也不知道     
      state的改变是谁弄的。
      view和state多对一的问题，action和state多对一的问题
      代码结构化  易维护

3.const store = new Vuex.Store({
    state:{
        count: 0
    },
    mutations:{
        increment(state){
            state.count++
        }
    },
    actions:{
        increment(context){
              context.commit('increment')
        }
    }
 })


































