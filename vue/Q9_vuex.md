# Vuex总结
## Q1：vuex是什么？什么时候使用vuex？

- 专门在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信

- 当多个组件依赖于同一状态，来自不同组件的行为需要变更同一状态需要使用vuex

  ![](https://img-blog.csdnimg.cn/img_convert/11a966479bad6de2bc521d30b90d391f.png)

## Q2：Vuex的基本使用

操作：放在 src/store/index.js

当需要判断或者处理其他业务逻辑的时候放在`actions(context,value)`里，当处理完毕以后通过`context.commit('mutations名称',value)`把数据放到mutations里

mutations里进行对state里数据的操作，使用数据的时候需要state.数据名称

组件中读取vuex中的数据：`$store.state.数据名称`

```js
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)
   
//准备actions对象——响应组件中用户的动作
const actions = {
    addOdd(context,value){
        console.log("actions中的addOdd被调用了")
        if(context.state.sum % 2){
            context.commit('ADD',value)
        }
    },
    addWait(context,value){
        console.log("actions中的addWait被调用了")
        setTimeout(()=>{
			context.commit('ADD',value)
		},500)
    },
}
//准备mutations对象——修改state中的数据
const mutations = {
    ADD(state,value){
        state.sum += value
    },
    SUBTRACT(state,value){
        state.sum -= value
    }
}
//准备state对象——保存具体的数据
const state = {
    sum:0 //当前的和
}
   
//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state
})

```

在组件中使用，绑定方法，在方法中`this.$store.commit('方法名称',传的值)`

```
<template>
	<div>
		<h1>当前求和为：{{$store.state.sum}}</h1>
		<h3>当前求和的10倍为：{{$store.getters.bigSum}}</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment">+</button>
		<button @click="decrement">-</button>
		<button @click="incrementOdd">当前求和为奇数再加</button>
		<button @click="incrementWait">等一等再加</button>
	</div>
</template>

<script>
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		methods: {
			increment(){
				this.$store.commit('ADD',this.n)
			},
			decrement(){
				this.$store.commit('SUBTRACT',this.n)
			},
			incrementOdd(){
				this.$store.dispatch('addOdd',this.n)
			},
			incrementWait(){
				this.$store.dispatch('addWait',this.n)
			},
		},
	}
</script>

<style>
	button{
		margin-left: 5px;
	}
</style>

```



## Q3：Vuex中的getter

概念：当`state`中的数据需要经过加工后再使用时，可以使用`getters`加工

在`store.js`中追加`getters`配置

组件中读取数据：`$store.getters.bigSum`

 ```
  ...
  const getters = {
  	bigSum(state){
  		return state.sum * 10
  	}
  }
  
  //创建并暴露store
  export default new Vuex.Store({
  	...
  	getters
  })
  
  
 ```



## Q3：Vuex中的map

mapState方法：用于帮助我们映射`state`中的数据，取代一个个写$store，在组件中直接当作组件内变量使用

```
computed: {
    //借助mapState生成计算属性：sum、school、subject（对象写法）
     ...mapState({sum:'sum',school:'school',subject:'subject'}),
         
    //借助mapState生成计算属性：sum、school、subject（数组写法）
    ...mapState(['sum','school','subject']),
},
```

mapGetters方法：用于帮助我们映射`getters`中的数据

```
computed: {
    //借助mapGetters生成计算属性：bigSum（对象写法）
    ...mapGetters({bigSum:'bigSum'}),

    //借助mapGetters生成计算属性：bigSum（数组写法）
    ...mapGetters(['bigSum'])
},
```

mapActions方法：用于帮助我们生成与`actions`对话的方法，即：包含`$store.dispatch(xxx)`的函数

```js
<button @click="组件内调用名称(n)">+</button>
methods:{
    //靠mapActions生成：incrementOdd、incrementWait（对象形式）
    ...mapActions({组件内调用名称:'vuex中的函数名称'})
}

```

mapMutations方法：用于帮助我们生成与`mutations`对话的方法，即：包含`$store.commit(xxx)`的函数

```js
<button @click="组件内调用名称(n)">+</button>
methods:{
    //靠mapActions生成：increment、decrement（对象形式）
    ...mapMutations({组件内调用名称:'vuex中的函数名称'}),
}
```



## Q4：Vuex中的命名空间

使用 const 名称来命名

读取的时候需要加上名称

```
const countAbout = {
	namespaced:true,//开启命名空间
	state:{x:1},
    mutations: { ... },
    actions: { ... },
  	getters: {
    	bigSum(state){
       		return state.sum * 10
    	}
  	}
}

const personAbout = {
  	namespaced:true,//开启命名空间
  	state:{ ... },
  	mutations: { ... },
  	actions: { ... }
}

const store = new Vuex.Store({
  	modules: {
    	countAbout,
    	personAbout
  	}
})

```

