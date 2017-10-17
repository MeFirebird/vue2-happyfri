##  项目学习    forked  from   <a href="https://github.com/bailicangdu/vue2-happyfri">baillicangdu/vue2-happyfri </a>   


### 待处理问题：
1.App.vue居然没被main.js引入 render？或者tempalte   why呢？  








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




总结：
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
答： 单向数据流的问题：  a.多个view依赖同一个state      b.不同view的需要变更同一个状态，不同view的methods都有一个变更此状态的方法了，所以你也不知道state的改变是谁弄的。
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


































#############################################################################################
# 说明

>  非常简单的一个vue2 + vuex的项目，整个流程一目了然，麻雀虽小，五脏俱全，适合作为入门练习。

>  如果对您有帮助，您可以点右上角 "Star" 支持一下 谢谢！ ^_^

>  或者您可以 "follow" 一下，我会不断开源更多的有趣的项目

>  如有问题请直接在 Issues 中提，或者您发现问题并有非常好的解决方案，欢迎 PR 👍

>  开发环境 macOS 10.12.3  Chrome 56 nodejs 6.10.0

>  这个项目主要用于 vue2 + vuex 的入门练习，另外推荐一个 vue2 比较复杂的大型项目，覆盖了vuejs大部分的知识点。目前项目已经完成。[地址在这里](https://github.com/bailicangdu/vue2-elm)


## 项目运行（nodejs 6.0+）
``` bash
# 克隆到本地
git clone https://github.com/bailicangdu/vue2-happyfri.git

# 安装依赖
npm install

# 开启本地服务器localhost:8088
npm run dev

# 发布环境
npm run build
```



# 效果演示


[demo地址](http://cangdu.org/happyfri/)（请用chrome手机模式预览）
   
### 移动端扫描下方二维码
<img src='https://github.com/bailicangdu/vue2-happyfri/blob/master/src/images/ewm.png' width="300" height="300" />



## 路由配置
```js
import App from '../App'

export default [{
    path: '/',
    component: App,
    children: [{
        path: '',
        component: r => require.ensure([], () => r(require('../page/home')), 'home')
    }, {
        path: '/item',
        component: r => require.ensure([], () => r(require('../page/item')), 'item')
    }, {
        path: '/score',
        component: r => require.ensure([], () => r(require('../page/score')), 'score')
    }]
}]

```



## 配置actions
```js
import ajax from '../config/ajax'

export default {
	addNum({ commit, state }, id) {
		//点击下一题，记录答案id，判断是否是最后一题，如果不是则跳转下一题
		commit('REMBER_ANSWER', id);
		if (state.itemNum < state.itemDetail.length) {
			commit('ADD_ITEMNUM', 1);
		}
	},
	//初始化信息
	initializeData({ commit }) {
		commit('INITIALIZE_DATA');
	}
}

```


## mutations
```js
const ADD_ITEMNUM = 'ADD_ITEMNUM'
const REMBER_ANSWER = 'REMBER_ANSWER'
const REMBER_TIME = 'REMBER_TIME'
const INITIALIZE_DATA = 'INITIALIZE_DATA'
export default {
	//点击进入下一题
	[ADD_ITEMNUM](state, payload) {
		state.itemNum += payload.num;
	},
	//记录答案
	[REMBER_ANSWER](state, payload) {
		state.answerid[state.itemNum] = payload.id;
	},
	/*
	记录做题时间
	 */
	[REMBER_TIME](state) {
		state.timer = setInterval(() => {
			state.allTime++;
		}, 1000)
	},
	/*
	初始化信息，
	 */
	[INITIALIZE_DATA](state) {
		state.itemNum = 1;
		state.allTime = 0;
	},
}
```

## 创建store
```js
import Vue from 'vue'
import Vuex from 'vuex'
import mutations from './mutations'
import actions from './action'


Vue.use(Vuex)

const state = {
	level: '第一周',
	itemNum: 1,
	allTime: 0,
	timer: '',
	itemDetail: [],
	answerid: {}
}

export default new Vuex.Store({
	state,
	actions,
	mutations
})
```


## 创建vue实例
```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import routes from './router/router'
import store from './store/'

Vue.use(VueRouter)
const router = new VueRouter({
	routes
})

new Vue({
	router,
	store,
}).$mount('#app')
```

