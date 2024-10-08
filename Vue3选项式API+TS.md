# vue3核心语法

## Setup

setup是Vue3中的一个新的配置项，值是一个函数

setup函数返回的对象中的内容，都直接再模板中使用

setup中无法访问this

setup函数会在beforeCreate之前调用



## 响应式数据

### 基本类型的响应式数据

使用ref创建基本类型的响应式数据

语法：let 变量名=ref(初始值)

使用ref前，需要import ref from "vue"

```ts
import {ref} from "vue"
let name=ref("陈木")
```

### 对象类型的响应式数据

使用reactive创建对象类型的响应式数据

```vue
<template>
<h1>{{ person.name }}</h1>
<h1>{{ person.gender }}</h1>
<button @click="changeGender">点击改变性别</button>
</template>

<script lang="ts" setup>
import {ref,reactive} from "vue"
let person=reactive({name:"陈木华",gender:"女"})
let changeGender=()=>{
  person.gender="其他"
}
</script>
```

### toRefs和toRef

toRefs和toRef可以将对象类型的响应式数据解构为ref类型的变量,从而可以修改数据

```vue
<template>
<h1>{{ person.name }}</h1>
<h1>{{ person.gender }}</h1>
<button @click="changeInfo">点击修改信息</button>
</template>

<script lang="ts" setup>
import {ref,reactive,toRefs} from "vue"
let person=reactive({name:"陈木华",gender:"女"})
let personInfo=toRefs(person)
let changeInfo:()=>void=()=>{
  personInfo.gender.value="男"
  personInfo.name.value="陈木"
}

</script>
```

```vue
<template>
<h1>{{ person.name }}</h1>
<h1>{{ person.gender }}</h1>
<button @click="changeInfo">点击修改信息</button>
</template>

<script lang="ts" setup>
import {ref,reactive,toRef} from "vue"
let person=reactive({name:"陈木华",gender:"女"})
let gender=toRef(person,"gender")
let name=toRef(person,"name")
let changeInfo:()=>void=()=>{
  gender.value="男" 
  name.value="陈木"
}

</script>
```

### Computed计算属性

有些属性需要通过已有属性计算得出

```vue
<template>
<h1>{{ personInfo}}</h1>
<button @click="changeInfo">修改信息</button>
</template>

<script lang="ts" setup>
import {ref,reactive,toRef, computed} from "vue"
let person=reactive({name:"陈木华",gender:"女"})
let personInfo=computed({
  get(){
    return person.name+"-"+person.gender
  },
  set(value){
    let [name,gender]=value.split("-")
    person.name=name
    person.gender=gender
  },
})
function changeInfo(){
   personInfo.value="李昕泽-沃尔玛购物袋" 
  }

</script>
```

## watch

作用：监视数据的变化，只能监视以下四种数据

1. ref定义的数据
2. reactive定义的数据
3. 函数返回的一个值
4. 一个包含上面内容的数组

### 监听ref定义的基本数据

```vue
<template>
<h1>sum = {{ sum }}</h1>
<button @click="changeSum">sum+1</button>
<h2>oldSum:{{ oldValue }} newSum:{{ newValue }}</h2>
</template>

<script lang="ts" setup>
import { ref,watch} from 'vue';

let sum=ref(1)
let oldValue=ref(0)
let newValue=ref(1)
function changeSum(){
  sum.value+=1
}
const watchSum=watch(sum,(newVal,oldVal)=>{
  oldValue.value=oldVal
  newValue.value=newVal
  if(newVal>10){
    watchSum()
  }
  })
</script>
```

watch的参数

1. 监视的数据
2. 回调函数，有两个参数
   1. newVal 被监视的数据的新值
   2. oldVal 被监视的数据的旧值

通过调用watchSum()停止监视

### 监听ref定义的对象类型数据

监听ref定义的对象类型数据，监听的是对象的地址值，若想监听对象内部属性的变化，需要手动开启深度监听

```ts
<template>
<h1>{{ p.name }}</h1>
<h1>{{ p.age }}</h1>
<button @click="agePlus">年龄+1</button>
<button @click="nameChange">姓名+~</button>
</template>

<script lang="ts" setup>
import { ref,watch} from 'vue';
class Person{
  name:string
  age:number
  constructor(name:string,age:number)
  {
    this.name=name
    this.age=age
  }
}
let p = ref(new Person("陈木",18))
function agePlus(){p.value.age+=1}
function nameChange(){p.value.name+="~"}
const watchP=watch(p,
(newValue,oldValue)=>
{console.log("新值:",newValue,"\n","旧值:",oldValue)},
{deep:true,immediate:true})
</script>
```

### 监听reactive定义的对象类型

reactive定义的对象类型默认开启深度监视

```ts
<template>
<h1>{{ p.name }}</h1>
<h1>{{ p.age }}</h1>
<button @click="agePlus">年龄+1</button>
<button @click="nameChange">姓名+~</button>
</template>

<script lang="ts" setup>
import { reactive, ref,watch} from 'vue';
class Person{
  name:string
  age:number
  constructor(name:string,age:number)
  {
    this.name=name
    this.age=age
  }
}
let p = reactive(new Person("陈木",18))
function agePlus(){p.age+=1}
function nameChange(){p.name+="~"}
const watchP=watch(p,
(newValue,oldValue)=>
{console.log("新值:",newValue,"\n","旧值:",oldValue)})
</script>
```

### 监视ref或reactive定义的对象类型数据中的某个属性

1. 该属性不是对象类型，需要写成函数形式
2. 若该属性依然是对象类型，可以直接编，也可以写成函数，建议为函数

```ts
<template>
<h1>姓名:{{ person.name }}</h1>
<h2>汽车:{{ person.car.c1 }} {{ person.car.c2 }}</h2>
<button @click="changeName">修改姓名</button>
<button @click="changeCar1">修改第一辆车</button>
<button @click="changeCar2">修改第二辆车</button>
</template>

<script lang="ts" setup>
import {reactive,watch} from 'vue'
let person=reactive({name:"江洵",car:{c1:"宝马",c2:"路虎"}})

function changeCar1(){
  person.car.c1="路虎"
}

function changeCar2(){
  person.car.c2="保时捷"
}

function changeName(){
  person.name+="~"
}

//watch
//监视ref,reactive定义的对象类型数据中的基础属性 需要定义为一个函数
watch(()=>person.name,()=>{console.log("姓名改变了");
})
//监视ref，ractive定义的对象类型数据中的对象属性，可以定义函数，也可以直接编写
//直接编写
//watch(person.car,()=>{console.log("车改变了")})
//使用函数
watch(()=>person.car,()=>{console.log("车改变了")},{deep:true})
</script>
```

### 监视上述多个数据

使用数组

```ts
<template>
<h1>姓名:{{ person.name }}</h1>
<h2>汽车:{{ person.car.c1 }} {{ person.car.c2 }}</h2>
<button @click="changeName">修改姓名</button>
<button @click="changeCar1">修改第一辆车</button>
<button @click="changeCar2">修改第二辆车</button>
</template>

<script lang="ts" setup>
import {reactive,watch} from 'vue'
let person=reactive({name:"江洵",car:{c1:"宝马",c2:"路虎"}})

function changeCar1(){
  person.car.c1="路虎"
}

function changeCar2(){
  person.car.c2="保时捷"
}

function changeName(){
  person.name+="~"
}

//watch
watch([()=>person.name,()=>person.car.c1],()=>{console.log("姓名或c1车改变了")},{deep:true})
</script>
```



## watchEffect

立即运行一个函数，响应式地追踪器依赖，并在依赖更改时重新执行该函数

```ts
<template>
<h1>姓名:{{ person.name }}</h1>
<h2>汽车:{{ person.car.c1 }} {{ person.car.c2 }}</h2>
<button @click="changeName">修改姓名</button>
<button @click="changeCar1">修改第一辆车</button>
<button @click="changeCar2">修改第二辆车</button>
</template>

<script lang="ts" setup>
import {reactive,watchEffect} from 'vue'
let person=reactive({name:"江洵",car:{c1:"宝马",c2:"路虎"}})

function changeCar1(){
  person.car.c1="路虎"
}

function changeCar2(){
  person.car.c2="保时捷"
}

function changeName(){
  person.name+="~"
}
watchEffect(()=>{
  if(person.car.c1=="路虎"){
    console.log("宝马换路虎")
  }
  if(person.car.c2=="保时捷"){
    console.log("路虎换保时捷")
  }
})

</script>
```

##  标签的ref属性

```ts
<template>
<h1 ref="name">陈木华</h1>
<button @click="getName">获取姓名组件</button>
</template>

<script lang="ts" setup>
import {ref} from 'vue'
let name=ref()
function getName(){
  console.log(name)
}
</script>
```



## Props的使用

```ts
<template>
<Child :list="personList"/>
</template>

<script lang="ts" setup>
import Child from "@/components/Child.vue"
import  { type Person } from "./person";
import {reactive} from 'vue'
let personList=reactive<Person[]>([
  {id:1,name:"陈木华",age:18},
  {id:2,name:"陈火华",age:118},
  {id:3,name:"陈金华",age:138},
  {id:4,name:"陈土华",age:148},
  {id:4,name:"陈水华",age:158},
])
</script>
```



```ts
<template>
    <h1>陈X华</h1>
<ul>
    <li v-for="person in list" :key="person.id">{{ person.name }}--{{ person.age }}</li>
</ul>
</template>

<script lang="ts" setup>
import type { Person } from '@/person';
import {defineProps} from 'vue'
//接收list+限制类型+限制必要性+指定默认值
withDefaults(defineProps<{list?:Person[]}>(),{list:()=>[{id:1,name:"陈木华",age:18}]})
</script>
```



## Vue3生命周期

```ts
<template>
<h1>当前求和为 {{ sum }}</h1>
<button @click="sumPlus">sum+1</button>
</template>

<script lang="ts" setup>
import { ref,onBeforeMount,onMounted,onBeforeUpdate,onUpdated,onBeforeUnmount,onUnmounted} from 'vue';

let sum=ref(0)
function sumPlus(){
    sum.value+=1
}

//生命周期
//创建 
console.log("创建")

//挂载
onBeforeMount(()=>{console.log("挂载前")})
onMounted(()=>{console.log("挂载完毕")})
onBeforeUpdate(()=>{console.log("更新前")})
onUpdated(()=>{console.log("更新完毕")})
onBeforeUnmount(()=>{console.log("卸载前");
onUnmounted(()=>{console.log("卸载完毕")})})
</script> 
```

## 自定义HOOK

# 路由

##  路由器的工作方式

### history模式

URL更加美观，不带有#，更接近传统的网站URL

后期项目上线时，需要服务端配合处理路径问题

### hash模式

兼容性更好，不需要服务器端处理路径

URL带有#不太美观，且在SEO优化放慢相对较差

## to的两种写法

```vue
//字符串写法
<RouterLink to="/home"></RouterLink>
//对象写法
<RouterLink :to="{ path='/home'}"></RouterLink>
```

## 命名路由

```ts
import Child from "@/components/Child.vue";
import { createRouter, createWebHashHistory } from "vue-router";

const router= createRouter({
    history:createWebHashHistory(),
    routes:[
        {
            name:'haizi',
  .          path:"/child",
            component:Child
        }
    ]
})
```

命名后，可以通过name路由

```vue
<RouterLink :to="{name='haizi'}">
</RouterLink>
```

## Query传参

```vue
<RouterLink:to=:'/news/detail?id={id}&?title={title}'>
 </RouterLink>
```

使用参数

```vue
<template>
	<h1>
        title= {{route.query.title}}
    </h1>
	<h1>
        id= {{route.query.id}}
    </h1>
</template>
<script setup lang="ts">
    import {useRoute} from 'vue-router'
    let route= useRoute()
</script>
```



对象写法

```vue
<RouterLink :to="{
                 path:'/news/detail'
                 query:{
                 title=title,
                 id=id
                 }
                 }">
 </RouterLink>
```

## params参数



```ts
import Child from "@/components/Child.vue";
import Detail from "@/components/detail.vue";
import { createRouter, createWebHashHistory } from "vue-router";

const router= createRouter({
    history:createWebHashHistory(),
    routes:[
        {
            name:'zhuye',
            path:"/child",
            component:Child,
            children:[
                {
                    name:'detail'
                    //使用占位符占位
                    path:'/detail/:id/:title',
                    component:Detail
                }
            ]
        }
    ]
})
```



```vue
<RouterLink :to="'/news/router/${id}/${title}'"></RouterLink>

//对象写法
<RouterLink :to="{
                 //不能用path，只能用name
				 name:"detail",
                 params:{
                      id:id,
                      title:title
                      }
                 }"></RouterLink>
```



参数使用

```vue
<template>
	<h1>
        title= {{route.query.title}}
    </h1>
	<h1>
        id= {{route.query.id}}
    </h1>
</template>
<script setup lang="ts">
    import {useRoute} from 'vue-router'
    let route= useRoute()
</script>
```



params参数不能传递对象和数组

## props配置

让路由组件更方便的收到参数，可以将路由参数作为props传递给组件

 

```ts
import Child from "@/components/Child.vue";
import Detail from "@/components/Detail.vue";
import { createRouter, createWebHashHistory } from "vue-router";

const router= createRouter({
    history:createWebHashHistory(),
    routes:[
        {
            name:'zhuye',
            path:"/child",
            component:Child,
            children:[
                {
                    path:'/detail/:id/:title',
                    component:Detail,
                    //配置props 只能处理params参数
                    props: true
                }
            ]
        }
    ]
})
```



```vue
<script setup lang="ts">
    defineProps(['id','title'])
</script>
```

第二种写法 将props写成对象也可以，就是第三种写法

```ts
import Child from "@/components/Child.vue";
import Detail from "@/components/Detail.vue";
import { createRouter, createWebHashHistory } from "vue-router";

const router= createRouter({
    history:createWebHashHistory(),
    routes:[
        {
            name:'zhuye',
            path:"/child",
            component:Child,
            children:[
                {
                    path:'/detail/:id/:title',
                    component:Detail,
                    //可以自己决定用什么做参数
                    props(route){
                        return route.query
                    }
                }
            ]
        }
    ]
})
```



## 编程式路由导航

```vue
<script setup lang="ts">
import { useRouter } from 'vue-router';

    const router=useRouter()
    router.push("/home")
    router.replace("/home")
</script>
```



# Pinia

## Pinia安装后使用

```ts
import { createApp } from 'vue'
import App from './App.vue'
import { createPinia } from 'pinia'

const app=createApp(App)
const pinia=createPinia()
app.use(pinia)
app.mount("#app")
```

## 存储和读取数据

建立count.ts文件 建立仓库

```ts
import { defineStore } from "pinia";

export const useCountStore=defineStore('count',{
    //真正存储数据的地方
    state(){
        return {
            sum:0
        }
    }
})
```

在count.vue中获取数据

```vue
<template>
<h1>求和: {{ sum }}</h1>
<button @click="add()">加一</button>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import {useCountStore} from '@/store/count'
let countStore=useCountStore()
//通过这两个方式可以获取state中存的
// countStore.sum
// countStore.$state.sum
let sum=ref(countStore.sum)
function add(){
    sum.value+=1;
}
</script>
```

## 修改数据的三种方式

```ts
count.ts
import { defineStore } from "pinia";

export const useCountStore=defineStore('count',{
    //真正存储数据的地方
    state(){
        return {
            sum:0
        }
    },
    actions:{
        add(){
            this.sum+=1
        }
    }
})
```



```vue
Count.vue
<template>
<h1>求和: {{ countStore.sum }}</h1>
<button @click="add()">加一</button>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import {useCountStore} from '@/store/count'
let countStore=useCountStore()
function add(){
    //修改数据的第一种方式
    countStore.sum+=1
    //第二种方式，通过patch修改
    countStore.$patch({sum:123})
    //第三种方式，通过action方法
    countStore.add()
}
</script>
```

## store To Refs

```ts
<template>
    <!-- 上面介绍的获取数据的方式都需要通过countStore.这样的方式获取 -->
    通过解构我们可以省去countStore.这样的步骤
<h1>求和: {{ sum }}</h1>
<button @click="add()">加一</button>
</template>

<script lang="ts" setup>
import { ref,  toRefs } from 'vue';
import {useCountStore} from '@/store/count'
import { storeToRefs } from 'pinia';
let countStore=useCountStore()
//直接通过toRefs解构，将store中的所有数据都转换为ref类型,代价太大
// let {sum} =toRefs(countStore)
//通过storeToRefs解构 只将store中的数据变为ref类型
let {sum} =storeToRefs(countStore)
function add(){
    countStore.sum+=1
}
</script>

```



## getters

getter可以将state中的属性进行"加工"

```ts
import { defineStore } from "pinia";

export const useCountStore=defineStore('count',{
    //真正存储数据的地方
    state(){
        return {
            sum:1
        }
    },
    actions:{
        add(){
            this.sum+=1
        }
    },
    //getter可以将state中的属性进行"加工"
    getters:{
        tenSum:state=>state.sum*10,
        elevenSum(state){
            return state.sum*11
        }
    }
})
```

同样可以通过storeToRefs解构获取getter中的数据

```vue
<template>
    <!-- 上面介绍的获取数据的方式都需要通过countStore.这样的方式获取 -->
<h1>求和: {{ sum }}</h1>
<h1>十倍:{{ tenSum }}</h1>
<h1>十一倍:{{ elevenSum }}</h1>
<button @click="add()">加一</button>
</template>

<script lang="ts" setup>
import { ref,  toRefs } from 'vue';
import {useCountStore} from '@/store/count'
import { storeToRefs } from 'pinia';
let countStore=useCountStore()
//通过storeToRefs解构 只将store中的数据变为ref类型
let {sum,tenSum,elevenSum} =storeToRefs(countStore)
function add(){
    countStore.sum+=1
}
</script>
```

## $subscribe

通过subscribe可以监视store中的数据

```vue
<template>
<h1>求和: {{ sum }}</h1>
<h1>十倍:{{ tenSum }}</h1>
<h1>十一倍:{{ elevenSum }}</h1>
<button @click="add()">加一</button>
</template>

<script lang="ts" setup>
import { ref,  toRefs } from 'vue';
import {useCountStore} from '@/store/count'
import { storeToRefs } from 'pinia';
let countStore=useCountStore()
let {sum,tenSum,elevenSum} =storeToRefs(countStore)
function add(){
    countStore.sum+=1
}

//通过$subscribe可以监视store中数据的变化
//mutate 修改的值的信息
//state 修改后的state情况
countStore.$subscribe((mutate,state)=>{
    console.log("数据发生变化")
    console.log(mutate)
    console.log(state)
})
</script>
```



# VUE组件间通信



## props

通过props组件，父子组件可以相互传递数据

**子组件**

```vue
<template>
<h1>父传子:{{ fatherData }}</h1>
<button @click="sendData('子数据')">子传父</button>
</template>

<script lang="ts" setup>
defineProps(["fatherData","sendData"])
</script>

```

**父组件**

```vue
<template>
    <h1 v-if="childData">{{childData}}</h1>
<Count :fatherData="fatherData" :sendData="getChildData"/>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Count from './component/Count.vue';
let fatherData="父数据"
let childData=ref('')
function getChildData(value:any){
    childData.value=value
}
</script>

<style>
</style>
```

父传子直接传递数据，子传父传递函数(本质上是调用了父组件传递给子组件的函数，还是父传子)



## 自定义事件

```vue
<template>
    <h1 v-if="childData">{{childData}}</h1>
//@buttonClick是自定义事件，可以用于子组件向父组件传递信息
<Count @buttonClick="buttonClick"/>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Count from './component/Count.vue';
let childData=ref('')
function buttonClick(value:string){
    childData.value=value
}
</script>

<style>
</style>
```



在子组件中，可以通过emit触发自定义事件

```vue
<template>
<h1></h1>
<button @click="emit('buttonClick','66666666')">子传父</button>
</template>

<script lang="ts" setup>
let emit=defineEmits(["buttonClick"])

</script>

```



## mitt

一个用于任意组件间通信的第三方库

接收数据的：提前绑定好事件(订阅)

提供数据的：在合适的时候触发事件(发布)

### 安装mitt

```powershell
npm install mitt
```

### 引入mitt

在项目下建立util文件夹，用于引入mitt

```ts
//引入mitt
import mitt from "mitt"
//调用mitt获取emitter 可以用于绑定事件，触发事件
const emitter=mitt()

export default emitter
```

### mitt使用，以上面的例子为例

父组件

```vue
<template>
    <h1 v-if="childData">{{childData}}</h1>
<Count/>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Count from './component/Count.vue';
//接收数据的组件引入emitter
import emitter from './util/emitter';
let childData=ref('')
//绑定事件
emitter.on('buttonClick',(value:any)=>{childData.value=value})

</script>

<style>
</style>
```

子组件

```vue
<template>
<h1></h1>
<button @click="emitter.emit('buttonClick',5555)">子传父</button>
</template>

<script lang="ts" setup>
import emitter from '@/util/emitter';



</script>

```



## v-model

### v-model在标签上的使用

使用v-model可以实现数据和组件的双向绑定

```vue
<template>
<h1></h1>
<input type="text" v-model="username">
</template>

<script lang="ts" setup>
let username="陈木华"
</script>
```

v-model底层使用过 value绑定和@input事件来实现的

```vue
<template>
<h1></h1>
<input type="text" :value="username" @input="username=(<HTMLInputElement>$event.target).value">
</template>

<script lang="ts" setup>
let username="陈木华"
</script>
```

### v-model在组件上的使用

v-model双向绑定底层实现

```VUE
<template>
<h1>请输入用户名:</h1>
<Input :modelValue="username" @update:modelValue="username=$event"></Input>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Input from './Input.vue';

let username=ref("陈木华")
</script>

```



```VUE
<template>
<input type="text"
 :value="modelValue"
 @input="emit('update:modelValue',($event.target as HTMLInputElement).value)"
 >
</template>

<script setup lang="ts">
defineProps(["modelValue"])
let emit=defineEmits(["update:modelValue"])

</script>

<style>
input{
    border: 2px solid black;
    background-image: linear-gradient(45deg,red,yellow,green);
    height: 30px;
    font-size: 20px;
    color: white;
}
</style>
```



## $attrs

$attrs用于实现当前组件的父组件，向其子组件进行通信(祖->孙)

$attrs是一个对象，包括所有父组件传入的标签属性

> $attrs会自动排除props中声明的属性

父组件

```vue
<template>
<Child :a="a" :b="b"></Child>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Child from './child.vue';

let a =ref(1)
let b=ref(2)
</script>
```



子组件

```ts
<template>
    <GrandChild ></GrandChild>
</template>

<script setup lang="ts">
import GrandChild from './grandChild.vue';
</script>
```



孙组件

```vue
<template>
<h1>a: {{ a }}</h1>
<h1>b: {{ b }}</h1>
</template>

<script setup lang="ts">
defineProps(['a','b'])
</script>
```



## $refs和\$parent

$refs用于父穿子

$parent用于子传父

### ref

通过ref属性可以获取组件的DOM元素，进而可以修改其值

父组件

```vue
<template>
<!-- <h1>{{ child.value.a }}</h1> -->
<Child ref="child"></Child>
<button @click="change">修改a</button>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Child from './child.vue';
let child=ref()
function change(){
    child.value.a=200
}
</script>
```

子组件

```vue
<template>
<h1>
    子组件
</h1> 
<h2>a: {{ a }}</h2>
</template>

<script setup lang="ts">
import { ref } from 'vue';

let a=ref(100)
defineExpose({a})
</script>
```



### $refs

通过$refs可以获取所有的子组件



同理 通过$parent可以获取父组件



## provide和inject

使用provide和inject 可以实现祖孙之间传递数据

```vue
<template>
<Father></Father>
</template>

<script lang="ts" setup>
import { provide } from 'vue';
import Father from './component/Father.vue';
provide('a',123)
provide('b',453)
</script>

<style>
</style>
```



```vue
<template>
<h1>
    子组件
</h1> 
<h2>a: {{ a }}</h2>
<h2>b: {{ b }}</h2>
</template>

<script setup lang="ts">
import { inject, ref } from 'vue';

let a=inject('a')
let b= inject('b')
</script>
```



## 插槽slot

### 默认插槽

```vue
<template>
<Child>
    <!-- vue组件使用双标签，可以在双标签内加上一些标签等元素
    在Child.vue中，使用slot标签可以显示这些内容 -->
    <h1>插槽内容</h1>
</Child>
</template>

<script setup lang="ts">
import Child from './child.vue';

</script>

```

```vue
<template>
<h1>子组件</h1>
<slot>默认值</slot>
</template>
```

### 具名插槽

意如其名，有名字的插槽

在template或者component标签上，使用v-slot:名字的语法，可以给插槽起名

在子组件中，使用name属性可以给slot赋值

```vue
<template>
	<Child>
    <template v-slot:1>
        <h1>插槽1内容</h1> 
    </template>
    <template v-slot:2>
        <h2>插槽2内容</h2>
    </template>
	</Child>
</ template>

<script setup lang="ts">
import Child from './child.vue';

</script>



```

```vue
<template>
<h1>子组件</h1>
<slot name="1"></slot>
<slot name="2"></slot>
</template>
```



### 作用域插槽

有时会出现数据在子组件，而数据的呈现形式由父组件决定的情况，这时可以使用作用域插槽

```vue
<tempalte>
    <Child>
     <template v-slot="{games}">
      <ol>
        <li v-for="game in games">
            {{game}}
        </li>
      </ol>
		</template>
</Child>
</tempalte>
<script setup lang="ts">
import Child from './child.vue';
</script>

```



```vue
<template>
<h1>子组件</h1>
<slot :games="games"></slot>
</template>

<script setup lang="ts">
let games:string[]=["陈木华","陈火华","陈金华","陈水华","陈土华","陈光华","陈暗华"]
</script>
```



# 其他API

### shallowRef和shallowRactivce

只处理第一层的响应式

### readonly与shallowReadonly

readonly接收一个响应式数据，返回一个只读数据

shallowReadonly与readonly类似，但是只保证第一层次的数据不被修改

### toRaw和markRaw

toRaw接收一个响应式对象，返回一个原始对象

markRaw标记一个对象，使其永远不会变成响应式的



## customRef

使用Vue提供的默认ref定义的响应式数据，数据一变，页面立马更新

若需要在页面更新前做一些操作，则需要用到customRef 即自定义Ref



```vue
<template>
 <input type="text" v-model="msg">
</template>

<script setup lang="ts">
import { customRef } from 'vue';
let initValue="陈木华万岁"
//使用customRef定义响应式数据 
let msg=customRef((track,trigger)=>{
    return{
        //get在msg被读取时调用
        get(){
            //track 告诉Vue msg数据很重要，需要跟踪
            track()
            return initValue
        },
        //set在msg被修改时调用
        set(value) {
            initValue=value
            //trigger 通知vue 数据改变了
            trigger()
        },
    }
})
</script>

```



# Vue3新组件

## teleport

将组件传送到现有的容器中

```vue
<template>
//将input传送到 body容器下
    <teleport to="body">
        <input type="text" v-model="msg">
    </teleport>
</template>

<script setup lang="ts">
import { customRef } from 'vue';
let initValue="陈木华万岁"
//使用customRef定义响应式数据 
let msg=customRef((track,trigger)=>{
    return{
        //get在msg被读取时调用
        get(){
            //track 告诉Vue msg数据很重要，需要跟踪
            track()
            return initValue
        },
        //set在msg被修改时调用
        set(value) {
            initValue=value
            //trigger 通知vue 数据改变了
            trigger()
        },
    }
})
</script>

```

## Suspense

等待异步组件时，渲染一些额外的内容，以获得更好的用户体验

一般子组件渲染前需要做一些异步任务，则需要用suspense标签包裹

```vue
<template>
<Suspense>
    <template v-slot:default>
        <Father></Father>
    </template>
	< /Suspense>
< /template>

<script lang="ts" setup>
import {  Suspense } from 'vue';
import Father from './component/Father.vue';
</script>

<style>
</style>
```

## 全局API转移到应用对象 

1. app.component('组件名',组件) 注册全局组件
2. app.config.globalProperties.x= xxx 注册全局属性
3. app.directive("指令名",回调函数) 注册全局指令
4. app.mount 挂载应用
5. app.unmount 卸载应用
6. app.use 使用第三方库

