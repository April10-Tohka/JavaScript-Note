---
html:
    toc: true
    # number_sections: true
    toc_depth: 6
    toc_float: true
        collapsed: true
        smooth_scroll: true
--- 
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [搭建Vue的脚手架](#搭建vue的脚手架)
- [搭建Vuex开发环境](#搭建vuex开发环境)
- [模板语法](#模板语法)
  - [文本](#文本)
  - [属性](#属性)
  - [双向数据绑定](#双向数据绑定)
- [Vue的条件语句](#vue的条件语句)
  - [v-if指令](#v-if指令)
  - [v-else-if  v-else](#v-else-if--v-else)
- [Vue的循环语句](#vue的循环语句)
  - [对对象进行遍历](#对对象进行遍历)
  - [计算属性也有setter()函数](#计算属性也有setter函数)
- [侦听属性watch](#侦听属性watch)
- [过滤器](#过滤器)
  - [全局过滤器](#全局过滤器)
  - [局部过滤器](#局部过滤器)
- [样式绑定](#样式绑定)
  - [给v-bind:class 一个对象](#给v-bindclass-一个对象)
  - [给v-bind:class 一个数组](#给v-bindclass-一个数组)
- [深入响应式原理](#深入响应式原理)
    - [vue无法检测到数组和对象的变化](#vue无法检测到数组和对象的变化)
      - [对于对象](#对于对象)
  - [Vue.set(object,属性名，值) / vm.$set() 来添加响应式属性](#vuesetobject属性名值--vmset-来添加响应式属性)
      - [对于数组](#对于数组)
  - [Vue.set(数组,索引，值) / vm.$set()](#vueset数组索引值--vmset)
- [组件间通信方式](#组件间通信方式)
  - [父子间通信](#父子间通信)
    - [props方式](#props方式)
      - [子传父（需要父组件给子组件传一个函数用来接收回调](#子传父需要父组件给子组件传一个函数用来接收回调)
    - [$emit自定义事件](#emit自定义事件)
  - [事件总线(可以父子间，兄弟间通信)](#事件总线可以父子间兄弟间通信)
- [插槽slot](#插槽slot)
  - [作用域插槽](#作用域插槽)
- [vuex状态管理模式](#vuex状态管理模式)
  - [mapState辅助函数](#mapstate辅助函数)
- [路由](#路由)
  - [安装路由](#安装路由)
  - [开始](#开始)
- [多级路由](#多级路由)
- [路由传参](#路由传参)
- [命名路由](#命名路由)
- [编程式路由](#编程式路由)
- [缓存路由](#缓存路由)
- [路由两个专属生命钩子](#路由两个专属生命钩子)
  - [activated](#activated)
  - [deactivated](#deactivated)
- [全局前置路由守卫](#全局前置路由守卫)
- [全局后置路由守卫](#全局后置路由守卫)
- [路由独享守卫](#路由独享守卫)
- [组件内的路由守卫](#组件内的路由守卫)

<!-- /code_chunk_output -->

# 搭建Vue的脚手架
在命令行中输入
第一次使用 全局安装@vue/cli

> npm install -g @vue/cli

切换到要创建项目的目录
> vue create xxx

启动项目
> npm run serve

# 搭建Vuex开发环境
安装 vuex3（适用于vue2） 
>npm i vuex@3

```javascript
    //main.js
  
import Vue from 'vue'  //引入vue

import App from './App.vue'//引入APP组件
import Vuex from 'vuex'//引入vuex
Vue.use(Vuex);//使用vuex插件
//创建一个store对象
const store =new Vuex.Store({
  state:{},
  actions:{},
  mutations:{},
})
//vue实例化
const vm=new Vue({
  render: h => h(App),
  store:store,
}).$mount('#app')
```
**vm 和vc 都有$store**

后续为了更改单独更改store，可以通过引入
>src/store/index.js
```javascript
    index.js
//引入vue
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//使用vuex插件
Vue.use(Vuex)
const actions={};
const mutations={};
const state={}

//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state,
})
```
```javascript
    main.js
//引入store
import store from './store'
// vue实例化
const vm=new Vue({
  render: h => h(App),
  store:store,
}).$mount('#app')
```

# 模板语法

## 文本
```
    <div id='app'>
        <p>{{message}}</p>
    </div>
```

## 属性 
对HTML元素的属性的值使用 v-bind:属性=值 或者简写 :属性=值
```html
    <div id='app'> 
        <a :href='url'>菜鸟教程</a>
    </div>

    <script>
        new Vue({
        data:{
            url:'https://www.runoob.com'
        }
    })
    </script>
    
```

## 双向数据绑定 
使用 v-model来实现数据绑定
``` html

```
    <div id='app'>
        <p>{{message}}</p>
        <input v-model=message>
    </div>
```

# Vue的条件语句
## v-if指令
v-if 指令将根据表达式seen的值为true还是false来决定是否插入h1元素
​``` html
    <div id='app'> 
        <h1 v-if="seen">取决于seen来是否显示</h1>
    </div>

    <script>
        new Vue({
        data:{
            seen:true,

        }
    })
    </script>
```
## v-else-if  v-else
跟平常的`if- else if - else`用法一样
```html
    <input type="text" v-model=shu>
    <div v-if="Number(shu)==5">
        v-if语句
    </div>
    <div v-else-if="Number(shu) >5">
        v-else-if语句
    </div>
    <div v-else>
        v-else语句
    </div>
    
```
> 5     v-if语句
3   v-else语句
78  v-else-if语句

# Vue的循环语句
就像Python那样`for 变量 in 可迭代元素(数组，对象)`
语法 :
>v-for='item in items' 

##对数组进行遍历
> v-for='(value,index) in array'
value 对应数组的每个元素
index 对应数组的每个元素的索引
```
    <p v-for="(value,index) in array" :key="index">{{ value}}---{{index}}</p>


    data:function()
  {
    return {
      
      array:['C','C++','Python','Java'],
    }
  }
```
输出：
> C---0
  C++---0
  Python---0
  Java---0


## 对对象进行遍历
> v-for="(value,key,index) in obj" :key="index">{{ value }}--{{ key }}--{{ index }}
value 对应对象的值
key 对应对象的键
index 对象的索引
```
    <p v-for="(value,key,index) in obj" :key="index">{{ value }}--{{ key }}--{{ index }}</p>
    data:{
        obj:{
        name:'宝马',
        price:2000,
        address:'德国',

      }
    }
```
输出：
>   value--key--index
    宝马--name--0
    2000--price--1
    德国--address--2

#计算属性
例子
```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>

  <script>
        new Vue({
    el: '#example',
    data: {
    message: 'Hello'
    },
    computed: {
        //提供的函数将作为计算属性reversedMessage的getter()函数
        reversedMessage: function () {
        // `this` 指向 vm 实例
        return this.message.split('').reverse().join('')
        }
    }
    })
  </script>
</div>
```
输出：
> Original message: "Hello"
Computed reversed message: "olleH"

计算属性reversedMessage依赖于message，只有message发生改变，reversedMessage才会更新
>所以只要message没有发生改变，多次访问reversedMessage都是同一个结果

## 计算属性也有setter()函数
```html
    <p>名字：{{fullname}}</p>
    <script>
        data:
        {
                
            firstname:'tony',
            lastname:'jenny'
        },
        computed:{
            fullname:{
                //计算属性更完整的写法
                get()
                {
                    return this.firstname+" "+this.lastname
                },
                set(value)
                {
                    name=value.split(' ')
                    this.firstname=name[0];
                    this.lastname=name[1];
                }
            }
        }
    </script>
    

```
`vm.fullname='holly bot'`会调用计算属性fullname的setter()
但此时输出`vm.fullname` 仍然是`tony jenny`  
访问计算属性的值是调用getter()  只有更改计算属性依赖的数据`firstname lastname`

# 侦听属性watch
```html
    <p>千米:<input v-model="kilometer"></p>
    <p>米:<input v-model="meter"></p>

    <script>
        watch:{
            //提供的函数将作为侦听器的handler()
            //该函数有两个参数
            kilometer:function(newvalue,oldvalue)
            {
                this.meter=this.kilometer*1000;

            },
            //侦听器的完整写法
            meter:{
                deep:true,//若侦听的为对象、数组，可以深度侦听到内部元素[1,2,[3,[4]]]
                handler:function(newvalue,oldvalue)
                {
                    this.kilometer=this.meter/1000;
                }
            }
        }
    </script>

```

# 过滤器
过滤器可以用在两个地方：**双花括号插值和 v-bind 表达式** 
## 全局过滤器
```
Vue.filter('过滤器名',function(){

})
```

## 局部过滤器
```
 new Vue({
    filters:{
        过滤器名:function()
        {

        }
    }
 })
```
 `{{message | filterA}}` message作为filterA函数的参数
过滤器也可以串联
`{{ message | filterA | filterB }}` 
*message*作为filterA的参数，filterA函数返回值作为filterB的参数



# 样式绑定
**class  style**都是HTML元素的属性，可以使用v-bind来绑定
不是很能理顺，看看[官方文档](https://v2.cn.vuejs.org/v2/guide/class-and-style.html)
## 给v-bind:class 一个对象
`<div v-bind:class="{ active: isActive }"></div>`
active这个class是否存在取决于**data数据里的isActive**的true/false

```
    data:
    {
        isActive:true
    }
```
结果：
> `<div class='active'></div>`
* * *

`<div v-bind:class="classObject"></div>`
```
    data:
    {
        classObject:
        {
            active:true,
            'text-danger':true,
            yangshi1:false,
        }
    }
```
结果：
> `<div class="active text-danger"></div>`

## 给v-bind:class 一个数组
`<div v-bind:class="[activeClass, errorClass]"></div>`

>data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}

结果：
> `<div class="active text-danger"></div>`


# 深入响应式原理
### vue无法检测到数组和对象的变化
#### 对于对象
当你把一个普通的 JavaScript 对象传入 Vue 实例作为 `data `选项，Vue 将遍历此对象所有的 property，并使用` Object.defineProperty` 把这些 property 全部转为 `getter/setter`。

## Vue.set(object,属性名，值) / vm.$set() 来添加响应式属性
```html
Vue.set(vm,'sex','男')
 [Vue warn]: Avoid adding reactive properties to a Vue instance or its root $data at runtime - declare it upfront in the data option.

*无法对vm添加响应式属性，只能对vm里的data里的对象才能添加*

```

```html
data:{
    school:{},
}

Vue.set(vm.school,'sex','男')
```

#### 对于数组
Vue 不能检测以下数组的变动：

当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`
当你修改数组的长度时，例如：`vm.items.length = newLength`


## Vue.set(数组,索引，值) / vm.$set()

> shuzu:['jenny','tony','jack']
school:{{shuzu[0]}}
school:jenny

`vm.shuzu[0]='baby`
>school:jenny

`Vue.set(vm.shuzu,0,'baby')`
>school:baby

# 组件间通信方式
## 父子间通信
### props方式
props只能父组件传递数据给子组件

#### 子传父（需要父组件给子组件传一个函数用来接收回调
```
//父组件
<template>
  <div id="app">
      <Student :shuchuage="shuchuage"></Student>
    
  </div>
  
</template>

<script>
import Student from './components/Student.vue'
export default {
  name: 'App',
  components: {
    Student
  },
  
  methods:{
    shuchuage:function(data)
    {
      console.log("这里是父组件！收到数据为",data);
      
    }
  }
}

```

```html
//子组件
<template>
    <div>
        名字：{{ name }}
        年龄:{{ age }}
        性别：{{ sex }}
        <button @click="sendmes">点击将数据传回给父组件</button>
    </div>

</template>

<script>
export default {
    name:'Student',
    props:['shuchuage'],
    data:function()
    {
        return {
            name:'张三',
            age:18,
            sex:'男'
        }
    },
    methods:{
        sendmes:function()
        {
            console.log("调用父组件传来的方法shuchuage");
            this.shuchuage(this.age);
        }
    }
}

```

### $emit自定义事件
`this.$emit('事件名',传回的参数)`
``` 
//父组件
<template>
  <div id="app">
        <Student v-on:send="shuchuage"></Student>
        //给Student子组件绑定send事件，一旦触发send事件，调用shuchuage()
  </div>
  
</template>

<script>
import Student from './components/Student.vue'
export default {
  name: 'App',
  components: {
    Student
  },
  data:function()
  {
    return {
      

    }
  },
  methods:{
    shuchuage:function(data)
    {
      console.log("这里是父组件！收到数据为",data);
      
    }
  }
```

```html
//子组件
<template>
    <div>
        名字：{{ name }}
        年龄:{{ age }}
        性别：{{ sex }}
        <button @click="sendmes">点击触发send事件</button>
    </div>
</template>

<script>
export default {
    name:'Student',
    data:function()
    {
        return {
            name:'张三',
            age:18,
            sex:'男'
        }
    },
    methods:{
        sendmes:function()
        {
            console.log("触发send事件！");
            this.$emit('send');
            
        }
    }
}
```

## 事件总线(可以父子间，兄弟间通信)

**组件的原型对象===Vue的原型对象**
安装全局事件总线(在main.js)
``` javascript
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  beforeCreate:function()
  {
    Vue.prototype.$bus=this;//在Vue的原型对象上安装全局事件总线
  }
}).$mount('#app')
```

```
//父组件
<template>
  <div id="app">
      <Student></Student>
    
  </div>
  
</template>

<script>

import Student from './components/Student.vue'


export default {
  name: 'App',
  components: {
    Student
  },
  data:function()
  {
    return {
      

    }
  },
  methods:{
    shuchuage:function(data)
    {
      console.log("这里是父组件！收到数据为",data);
      
    }
  },
  //在app组件挂载完成后，给全局事件总线绑定一个sendage事件
  mounted:function()
  {
    this.$bus.$on('sendage',this.shuchuage);
  }
}
```

```html
//子组件 
<template>
    <div>
        名字：{{ name }}
        年龄:{{ age }}
        性别：{{ sex }}
        <button @click="sendmes">点击触发原型对象上$bus的sendage事件</button>
    </div>

</template>

<script>
export default {
    name:'Student',
    
    data:function()
    {
        return {
            name:'张三',
            age:18,
            sex:'男'
        }
    },
    methods:{
        sendmes:function()
        {
            console.log("触发sendage事件！");
            this.$bus.$emit('sendage',this.age);
        }
    }
}
```

# 插槽slot

```
//父组件
<template>
  <div id="app">
        <Categoty>

        </Categoty>
  </div>
</template>

<script>

import Category from './components/Category.vue';
export default {
	
  name: 'App',
  components: {
    Category,
  },
  data:function()
  {
	return {
		
	}
  }
}
</script>
```

```html
//子组件
<template>
    <div class="item">
        
    </div>
</template>

<script>
export default {
    name:'Category',
    
    data:function()
    {
        return {
            games:['cf','cs','qq飞车','QQ炫舞'],
        }
    }
}
</script>
```

`<slot name='header'></slot>` 该插槽的名字为header  不带`name`属性的slot元素默认为`default`

`<slot>默认内容</slot>`  若该组件标签间没有内容时，将会显示！

```html
<Categoty>
    <template v-slot:插槽名>
        <h1>你好！</h1>
    </template>
    //在一个 <template> 元素上使用 v-slot 指令,现在 <template> 元素中的所有内容都将会被传入相应的插槽。
    任何没有被包裹在带有 v-slot 的 <template> 中的内容都会被放入默认插槽。
</Categoty>
```

## 作用域插槽
有时让插槽内容能够访问子组件中才有的数据是很有用的。
```html
//子组件
<template>
    <div class="item">
        <slot v-bind:games='games' mes='hello'></slot>
        //为了games数据能在父组件app中可用,我们将games作为slot元素的一个属性绑定上去
        //绑定在slot元素上的属性称为插槽的props
    </div>
</template>

<script>
export default {
    name:'Category',
    
    data:function()
    {
        return {
            games:['cf','cs','qq飞车','QQ炫舞'],
        }
    }
}
</script>
```

```html
//父组件
<template>
  <div id="app">
    // slotProps包含所有插槽 prop 的对象
        <Categoty >
            <template v-slot:header='slotprops'>//放在name=header的插槽中
                {{slotprops}}  //{ "games": [ "cf", "cs", "qq飞车", "QQ炫舞" ], "mes": "hello" }
                <ul>
                    <li v-for="(value,index) in slotprops.games" :key="index">
                    {{ value }}
                    </li>
                </ul>
            </template>
            
        </Categoty>
  </div>
</template>

<script>

import Category from './components/Category.vue';
export default {
  name: 'App',
  components: {
    Category,
  },
  
}
</script>
```

# vuex状态管理模式
<img src='../vuex.png'>

>actions mutations state都是对象



```html
<button @click="jia">+</button>
methods:{
    jia:function()
    {
        this.$store.dispatch('jia',value)；
        //也可以直接调用$store.commit()，不经过actions，直接到mutations处理修改数据
            this.$store.commit('jia',value);
    }
}
```

>src/store/index.js

```javascript
    index.js
//引入vue
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//使用vuex插件
Vue.use(Vuex)
const actions={
    jia:function(context,value)
    {
        context.commit('jia',value)
    }
};

const mutations={
    jia:function(state,value)
    {
        state.sum+=value  
    }
};
const state={
    sum:0,
}

//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state,
})
```

## mapState辅助函数
**mapGetter辅助函数同理**
>//像这样组件需要获取多个$store.state里的数据，并且都作为计算属性的值会有些重复
//mapState()辅助函数来帮助我们生成计算属性

```javascript
computed:{
        // biga:function()
        // {
        //     return this.$store.state.a;
        // },
        // bigb:function()
        // {
        //     return this.$store.state.b;
        // },
        // bigc:function()
        // {
        //     return this.$store.state.c;
        // },
        //像这样组件需要获取多个$store.state里的数据，并且都作为计算属性的值会有些重复
        //mapState()辅助函数来帮助我们生成计算属性

        ...mapState({
            biga: state => state.a,
            bigb:'b',//等同于 state => state.b
            bigc:'c',
        })

        //若采用模块化编程的话,
        ...mapState('modelu',{
            biga:'a',//等同于 this.biga  映射到 this.$store.state.modelu.a
            bigb:'b',
        })
```

# 路由

## 安装路由
> npm i vue-router@3


## 开始
>src/router/index.js
该文件专门来创建该应用的整个路由器

```javascript
    //src/router/index.js
   
import VueRouter from "vue-router";
//引入组件
import Home from '../components/Home.vue'
import About from '../components/About.vue'
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About,
        },
        {
            path:'/home',
            component:Home,
        }
    ]
})
```



```javascript
    //main.js
    import VueRouter from 'vue-router'//vue-router是一个插件，在main.js中引入并使用该插件
    import router from './router'//引入路由器文件
    Vue.use(VueRouter)
    new Vue({
        render: h => h(App),
        router:router,
}).$mount('#app')
```

```html
    //App.vue
    <!-- 使用 <router-link>组件来导航，通过to属性来指定链接 -->
    <!-- 最终 <router-link>被渲染成一个a标签-->
    <router-link to="/Home" active-class="active">home</router-link>
    <br>
    <router-link to="/About" active-class="active">about</router-link>

    <!-- 路由路径匹配对应的组件渲染在这里 类似于插槽 -->
    <router-view></router-view>
```

# 多级路由

我们将经常跳转页面的vue组件统一放到同一个文件夹管理pages
>src/pages

一个被渲染组件同样可以包含自己的嵌套 `<router-view>`
message组件里的student组件
```javascript
{
    path:'message',
    component:Message,
    //children配置跟routes配置一样
    children:[
        {
            path:'student',
            component:Student,
            children:[
                //student组件也可以有自己的路由
                {
                    path:
                    component:
                }
            ]
        }
    ]
}
```
**只有一级路由的路径需要`/`,其他路径不需要`/`**


#   路由传参
```
<route-link 
:to="{
    path:'/student/detail',
    
    //query来传递参数
    query:{
        xuexiao:school,
        xuexiaodizhi:schooladress,
        xuexiaopaiming:schoolrank
    },

    // params来传递参数
    params:{
        xuexiao:school,
        xuexiaodizhi:schooladress,
        xuexiaopaiming:schoolrank,
    },
    }"> 

    </route-link>
```
>detail对应的组件detail有`$route`属性，该属性内有`query`属性，得到传来的参数
detail组件使用传来的参数，通过`$route.query`

**每次使用这些参数都需要this.$route.query/params来获得参数,形成高度耦合**
**_通过props来将路由和组件解耦_**
>src/router/index.js

```javascript
 routes:[
        {
            path:'/about',
            component:About,
            children:[
                {
                    
                    path:'doumenyizhong',
                    component:doumenyizhong,

                    //props第一种写法，将对象key-value全部作为组件的props属性
                    // props:{a:1,b:2,c:3},

                    // props第二种写法，若为true，将$route里的params参数全部为props参数
                    // props:true,

                    //props第三种写法，函数写法  函数接受该组件的$route属性，并返回一个对象
                    props:function($route)
                    {
                        return {
                            'xuexiao':$route.params.xuexiao,
                            'xuexiaodizhi':$route.params.xuexiaodizhi,
                            'xuexiaopaiming':$route.params.xuexiaopaiming,

                        }
                    }
                },
                
            ]
        },
```

```html
doumenyizhong组件
<div>
        <!-- $route.query -->
        <h1>query</h1>
        <p>学校：{{ $route.query.xuexiao }}</p>
        <p>学校地址：{{ $route.query.xuexiaodizhi }}</p>
        <p>学校排名：{{ $route.query.xuexiaopaiming }}</p>

        <!-- $route.params -->
        <h1>params</h1>
        <p>学校：{{ $route.params.xuexiao }}</p>
        <p>学校地址：{{ $route.params.xuexiaodizhi }}</p>
        <p>学校排名：{{ $route.params.xuexiaopaiming }}</p>

        <!-- props -->
        <h1>props</h1>
        <p>学校：{{ xuexiao }}</p>
        <p>学校地址：{{ xuexiaodizhi }}</p>
        <p>学校排名：{{ xuexiaopaiming }}</p>
</div>
props:['xuexiao','xuexiaodizhi','xuexiaopaiming'],
```
# 命名路由
给路由设置名字，更方便快捷跳转
```javascript
 routes:[
        {
            path:'/about',
            component:About,
            children:[
                {
                    name:'doumen1zhong',
                    path:'doumenyizhong',
                    component:doumenyizhong,
                }
            ]
        }
 ]

<router-link :to="{
            // path:'/about/doumenyizhong',
            name:'doumen1zhong',
}


```

# 编程式路由
**不使用`<route-link>`标签,通过调用vc上`$router`的`push()back()forward()go()`方法**
```html
<button @click="pushhome">home</button> 
<button @click="replacehome">replacehome</button>
        
<br>
<button @click="pushabout">about</button> 
<button @click="replaceabout">replaceabout</button>


method:{
    pushhome()
    {
      this.$router.push({
        path:'home',
        query:{a:1,b:2}
      })
    },
    pushabout()
    {
      this.$router.push({
        path:'about',
      })
    },
    replaceabout()
    {
      this.$router.replace({
        path:'about',
      })
    },
    replacehome()
    {
      this.$router.replace({
        path:'home',
      })
    },
}
```

# 缓存路由
**作用：让不展示的路由组件保持挂载，不被销毁**
```html
    <keep-alive include="About">
            <router-view ></router-view>
    </keep-alive>
    include属性对哪个组件进行缓存，

    若对多个组件进行缓存 :include=['About','Home']
    <keep-alive :include="['About','Home']">
            <router-view ></router-view>
    </keep-alive>
```

# 路由两个专属生命钩子
## activated
**about路由组件被渲染时，调用activated钩子**
```
activated()
  {
    console.log("about组件激活了");
    this.timer=setInterval(()=>{
      console.log("@@")
    },500)
  },
```
## deactivated
**about路由组件不被渲染时，调用deactivated钩子**
```
deactivated()
  {
    console.log("about组件失活了");
    clearInterval(this.timer);
  }
```

# 全局前置路由守卫
vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航
使用`router.beforeEach()`来注册一个全局前置路由守卫`
```javascript
src/router/index.js文件中
const router= new VueRouter({
    routes:[],
})


router.beforeEach((to,from,next)->{
    //当路由发生跳转前，会调用该函数,初始化也会调用该函数
    //to:要跳转的路由
    //from:当前的路由
    //next:为一个方法，一定要调用该方法才能放行

    // 实例：
    if(to.name==='xinwen'||to.name==='xiaoxi')
    {
        next()
    }
    else
    {
        next();
    }


})



export default router;
```

# 全局后置路由守卫
使用`router.afterEach()`来注册一个全局前置路由守卫
```javascript
router.afterEach((to,from)=>{
    //当路由发生跳转后，会调用该函数,初始化也会调用该函数
    //该钩子没有next函数
})
```

# 路由独享守卫
**在路由配置上直接定义beforeEnter守卫**
渲染该组件前调用该函数beforeEnter()
>在doumenyizhong该路由配置中

```javascript
name:'doumenyizhong',
path:'doumenyizhong',
beforeEnter:function(to,from,next)
{
  console.log("这里是斗门一中组件的独享守卫！");
  if(to.meta.isauth)
  {
  next()
  }
  else if(to.meta.title==='斗门一中')
  {
  next();
  }
  else
  {
  alert("抱歉！无权访问！！");
  }

},
```

# 组件内的路由守卫
**在路由组件内直接定义一下路由守卫
`beforeRouteEnter ` `beforeRouteUpdate` `beforeRouteLeave`**
>Home.vue
```javascript
//beforeRouteEnter  通过路由规则渲染该组件前调用该函数
  beforeRouteEnter(to,from,next)
  {
      console.log("beforeRouteEnter",to,from,next);
      next();
  },
  // beforeRouteLeave 通过路由规则离开该组件时，调用该函数
  beforeRouteLeave(to,from,next)
  {
      console.log("beforeRouteLeave",to,from,next);
      next();
  }
  //beforeRouteUpdate 暂时还不知道怎么用，看不会文档！
```
