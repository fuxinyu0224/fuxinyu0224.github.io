---
title: Vue
date: 2022-07-10 10:59:42
tags: vue
categories: vue
top: true
---
##### vue基本概念

   ```vue
   1. mvvm设计模式
      m->model 数据 v-> views视图 vm->viewModle 数据视图连接层
   2. 服务器端渲染
      vue.js服务端渲染是在浏览器中输出Vue组件，进行生成DOM和操作DOM,
      然而，也可以将同一个组件渲染为服务器端的HTML字符串，将它们直接发送到浏览器,
      最后将这些静态标记"激活"为客户端上完全可交互的应用程序。
   3. 挂载
      虚拟dom与真实dom建立关系，让Vue实例控制页面中的某个区域的过程，称之为挂载。
      挂载的方式有：
      	1、通过“el:'css选择器'”语句进行配置；
      	2、通过“Vue实例.$mount("css选择器")”语句进行配置。
   4.render函数和template
   	template:
   		一个字符串模板，用作 component 实例的标记。模板将会替换所挂载元素的 innerHTML。
   		挂载元素的任何现有标记都将被忽略，除非模板中存在通过插槽分发的内容。
   		如果字符串以 # 开始，则它将被用作 querySelector，并使用匹配元素的 innerHTML 作为模板字符串。
   		这允许使用常见的 <script type="x-template"> 技巧来包含模板。
   	render:
   		字符串模板之外的另一种选择，允许你充分利用 JavaScript 的编程功能。
       	render 函数的优先级高于根据 template 选项或挂载元素的 DOM 内 HTML 模板编译的渲染函数。
   ```
##### vue生命周期图示

![](https://fxytb.oss-cn-nanjing.aliyuncs.com/vue/images/lifecycle.svg)

<!--more-->

##### 生命周期函数

```
beforeCreate
	在实例初始化之后、进行数据侦听和事件/侦听器的配置之前同步调用。
created
	在实例创建完成后被立即同步调用。
	在这一步中，实例已完成对选项的处理，意味着以下内容已被配置完毕：数据侦听、计算属性、方法、事件/侦听器的回调函数。
	然而，挂载阶段还没开始，且 $el property 目前尚不可用。
beforeMount
	在挂载开始之前被调用：相关的 render 函数首次被调用。该钩子在服务器端渲染期间不被调用。
mounted
	在实例挂载完成后被调用，这时候传递给 app.mount 的元素已经被新创建的 vm.$el 替换了。
	如果根实例被挂载到了一个文档内的元素上，当 mounted 被调用时， vm.$el 也会在文档内。
	注意 mounted 不会保证所有的子组件也都被挂载完成。如果你希望等待整个视图都渲染完毕，可以在 mounted 内部使用 vm.$nextTick 。
beforeUpdate
	在数据发生改变后，DOM /被更新之前被调用。
	这里适合在现有 DOM 将要被更新之前访问它，比如移除手动添加的事件监听器。
	该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务器端进行。
updated
	在数据更改导致的虚拟 DOM 重新渲染和更新完毕之后被调用。当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。
	然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或侦听器取而代之。
	注意，updated 不会保证所有的子组件也都被重新渲染完毕。如果你希望等待整个视图都渲染完毕，可以在 updated 内部使用 vm.$nextTick：
beforeUnmount
	在卸载组件实例之前调用。在这个阶段，实例仍然是完全正常的。
	该钩子在服务器端渲染期间不被调用。
unmounted
	卸载组件实例后调用。调用此钩子时，组件实例的所有指令都被解除绑定，所有事件侦听器都被移除，所有子组件实例被卸载。
	该钩子在服务器端渲染期间不被调用。
```

#####  常用模板指令

```
1.  指令
        v-on：用于绑定函数
        v-model: 将元素和数据进行双向绑定
        v-bind: 用于绑定元素属性
        v-for: 用于循环
        v-html:把指令对应的元素转为html
        v-once:指令对应的元素只能被渲染一次
        v-if: 通过控制元素在DOM里面是否存在来控制展示
        v-show:  通过disabled属性来控制展示
2.  差值表达式
            {{}}内元素指向实例内部的变量
            {{}}内部可以使用js方法 比如 Math.max(1,2)
3.  指令简写
            v-on:   -------> @
            v-bind: -------> :
4.  动态属性
            []内元素指向实例内部的变量,作为动态属性 比如 
			data(){
				return{
					name:'xxx',
					title:'xxx',
					event:click
				}
			}
		   :[name]="xxx"  相当于动态的给属性绑定值
		   @[event]='handleXXX' 此时相当于绑定了点击事件
5.  阻止默认行为
            e.preventDefault() ---------> @click.prevent="handleClick"
6.  组件
	调用组件的称为父组件或根组件
	被调用的组件称之为子组件
	例如:
	<demo :class="classA"></demo>
	app.component('demo',{
        template:`
        <div :class="$attrs.class">one</div>
        <div :class="$attrs.class">two</div>
        `
    })
	编译后
	        <div :class="classA">one</div>
        	<div :class="classA">two</div>
    如果想使用父组件的class属性可以使用 $attrs.class
```

##### data & methods & computed & watch

```vue
1. data
	example:
		data(){
			return {
				num: 1
			}
		}
	结论:
		如果vm是根组件获取data元素可以使用 vm.$data.属性 也可以直接使用 vm.属性的方式来获取根组件内部data的属性值
2. methods
	example:
        methods:{
            handleClick(){
                alert(1)
            }
        }
	结论:
        在根组件的methods:{}内部定义方法然后通过指令来绑定事件调用方法 比如 @click="handleClick" 绑定点击事件
        在插值表达式{{}}内部使用方法。比如 {{formatString(message)}},只有页面重新渲染，才会重新计算,
        调用多个方法，可以在事件内部使用逗号或分号隔开 *方法的括号要包含*
3. computed 
    example :	
        data(){
            return {
                a:1,
                b:2
            }
        },
        computed{
            total(){
                return a * b
            }
        }
    结论：
        当计算属性依赖的内容发送变更时。才会重新执行计算
        computed和method都能实现的功能建议使用computed因为有缓存
        computed和watch都能实现的功能建议使用computed因为更加简洁
4. watch
    example :	
        data(){
            return{
                num:1,
                num2:3
            }
        },
        watch:{
            total(c,p){
                // c 变更后的值
                // p 变更前的值
            }
        }
    结论:
        对属性进行监听 ，比如 price(current,prve) 第一个参数代表变更后 ，第二个参数代表变更前
        当监听数据发生变化时函数会执行
```

##### 样式绑定语法

```vue
绑定方式
1. :class 绑定 字符
	classString: 'red'
	styleString: 'color:red'
2. :class 绑定 对象
	classObject: { red: true, green: false }
	styleObject: {color: "orange",background:"yellow"}
3. :class 绑定 数组
	calssArray: ['red', 'green',{brown:true}]
	styleArray:['color:orange','background:yellow']
```

##### 条件渲染

```vue
v-if
v-else-if
v-else
```

##### 列表循环渲染

```vue
v-for 
(item,index) in list
(item,key,index) in listObject
list 变更函数
push 添加
pop 从尾移除
shift 从头开始移除
unshift 从头开始添加一个元素
splice 分割数组
reverse 反转数组
sort 数组排序
替换
concat 新增一个元素到目标数组内并返回新增元素后的数组
filter 过滤出符合条件的数据
直接更新数组内容
list[1]='hello'
循环对象
(item,key,index) in listObject
动态增加属性
this.listObject.age=12
this.listObject.sex=male
v-for 优先级 高于 v-if 
可以使用 template 进行控制
```

##### 条件绑定

```vue
如果需要在方法内使用event可以使用$event
事件修饰符
    stop 例如:@click.stop可以阻止事件冒泡
    self 例如 @click.self只要在自己的DOM事件被触发时才会调用方法，内部子组件触发则不会调用
    prevent 例如 @click.prevent 阻止默认行为 可以组件表单提交
    capture 把事件运营模式改为从外到内 默认时冒泡形式从内到外
    once 事件只会触发一次
    passive 提升性能 例如 scroll.passive 
按键修饰符
	enter、table、delete、esc、up、down、left、right
鼠标修饰符
	click.	left, right ,middle
精确修饰符
	click.exact 按住ctrl后 点击div触发 可以ctrl + 其他的
	click.ctrl.exact 此时安装ctrl 触发 不可以加其他的键
```

##### 表单中双向绑定的指令使用

```vue
控件:
    input
        type:
            checkbox
                true-value: 勾选时value值
                false-value: 取消勾选时value值
            radio
    textarea
    select
v-model 修饰符
    lazy: 不让数据实时发生变化，在input触发blur事件，数据变化
    number:将值转换为number
    trim: 去除前后的空格
```

##### 组件

```vue
1.全局组件
	app.component创建
	组件具有复用性 -- 组件属性是组件独有的
	app直接创建的组件具有全局性,性能不高但是使用方便 -- 不使用也会挂载在app上
	名称建议小写
2. 局部组件
	通过变量的形式定义组件
	使用components:{} 内部注册
	组件具有复用性
	局部组件通过变量形式定义，注册后才能使用，性能比较高，使用有些麻烦
	组件名建议大写
	局部组件使用时，要做一个名字和组件间的映射对象，不写映射vue底层也会自动进行映射
3. 数据传递
	props:
		type: 类型 String,Number,
		required: ture/false 设置属性是否是必传
		validater: function(){} 传递函数 定义 校验规则
		default: funcation 传递函数定义默认值
		可以直接传字符串 比如 <hello-world content="123"/> 使用props接收 props:["content"]
		属性传的时候使用 content-abc 接收的时候使用contentAbc
	单向数据流:
		子组件可以使用父组件传递过来的数据，但是子组件不能修改父组件的数据，可以定义变量接收，对子组件定义的变量进行修改
	Non-props
		当父组件向子组件传递属性，子组件没有用props进行接收时，会将属性作为子组件最外层的属性
		如果不想继承这些属性可以使用inheritAttrs:false
		当只有一个组件使用这个属性时可以直接写，当有多个组件使用这个属性是，需要使用$attrs
4.组件之间通过事件通信
	通过 $emit 向父组件传递函数 
	emits 属性可以标识有哪些函数被传递
        可以使用数组存储被标识的方法
        也可以使用{}定义函数
	modelValue和v-model:xxx
		当父子组件间数据具有双向绑定关系时，可以使用modelValue来简化代码
		子组件$emit 必须使用update:xxx的形式
		v-model:xxx 配合$emit 将数据从父组件传递导子组件
	自定义修饰符
		modelModifiers 可以接收v-model的修饰符从而修改数据

```

##### 插槽

```vue
概念
	父模板里调用的数据属性，使用的都是父模板里的数据
	子模版里调用的数据属性，使用的都是子模版里的数据
插槽
	<slot></slot>
具名插槽
	<template v-slot:xxx>
	<slot name="xxx">
	简写
	v-slot ---> #
作用域插槽
	<list v-slot="{item}"/>
		{{item}}
	</list>
	<div>
		<slot v-for="item in list" :item="item"/>
	</div>
```

##### 动态组件和异步组件

```vue
动态组件
	动态组件：根据数据的变化，结合 component 这个标签 来随时动态切换组件的显示
    <component :is=xxx > 通过is来判断展示那个组件
    <keep-alive></keep-alive> 可以保存数据状态
异步组件
	app.component('async-common-item',Vue.defineAsyncComponent(()=>{
		return new Promise((resolve,reject)=>{
			setTimeout(()=>{
				resolve({
					template:`<div>1234567</div>`
				})
			},4000)
		})
	}))
```

##### 基础知识查缺补漏

```vue
v-once使元素只渲染一次
ref 实际上是获取dom节点/组件的语法
provide  inject
```

##### vue实现基础的css过度与动画效果

```vue
过渡和动画
从一个状态缓慢到另一个状态叫做过渡
一个元素运动的情况叫做动画
动画
        <style>
            @keyframes leftToRight {
                0%{
                    transform: translateX(-100px)
                }
                50%{
                    transform: translateX(-50px);
                }
                0%{
                    transform: translateX(0px);
                }
            }
            .animation{
                animation: leftToRight 3s;
            }
        </style>
        <script>
                const app = Vue.createApp({
                    data(){
                        return{
                            count:"hello world",
                            animate:{
                                animation:true
                            }
                        }
                    },
                    methods:{
                        handleClick(){
                            this.animate.animation=!this.animate.animation
                        }
                    },
                    template:`<div :class="animate">{{count}}</div>
                    <button @click="handleClick">切换class</button>
                    `
            })
            const vm = app.mount('#root')
        </script>

过渡
	<style>
		.transition{
            transition: 3s background-color ease;
        }
        .blue{
            background-color: blue;
        }

        .green{
            background-color: green;
        }

        .red{
            background-color: red;
        }
    </style>
    
    <script>
    const app = Vue.createApp({
        data(){
            return{
                count:"hello world",
                animate:{
                    transition:true,
                    animation:true,
                    blue:true,
                    green:false
                }
            }
        },
        methods:{
            handleClick(){
                this.animate.animation=!this.animate.animation
                this.animate.blue=!this.animate.blue
                this.animate.green=!this.animate.green

            }
        },
        template:`<div :class="animate">{{count}}</div>
        <button @click="handleClick">切换class</button>
        `
    })
    const vm = app.mount('#root')
</script>
单组件和单元素的入场和出场
概念
    单元素的展示是入场
    单元素的隐藏是出场
过渡
	<transition></transition>
	入场
	  .v-enter-from {
            opacity: 0;
        }

        .v-enter-to{
            opacity: 1;
        }

        .v-enter-active{
            transition: opacity 3s ease-out;
        }
   
    离场
    	
    	.v-leave-form{
            opacity: 1;
        }

        .v-leave-to{
            opacity: 0;
        }

        .v-leave-active{
            transition: opacity 3s ease-in;
        }
 动画
 	 <style>
       
       //入场动画
        .v-enter-active{
            animation: leftToRight 3s;
        }

        .v-leave-active{
            animation: leftToRight 3s;
        }
        
       //出场动画
        @keyframes leftToRight {
            0% {
                transform: translateX(-100px)
            }

            50% {
                transform: translateX(-50px);
            }

            100% {
                transform: translateX(50px);
            }

        }

    </style>
入场和出场 class
	    enter-active-class="hello"
        leave-active-class="bye"
       	enter-from-class="hello"
        leave-from-class="bye"
        enter-to-class="hello"
        leave-to-class="bye"
```

##### transition设置

```vue
<transition></transition>
name="xxx" 当指定了name时 默认的v-enter-from 等 需要换成 xxx-enter-from
enter-from-class="xxx" 当指定了入场出场的class后，对应的属性可以修改为xxx
duration="100"/duaration="{enter:10000,leave:1000}" 指定过渡和动画执行时间
type="transition/animation" 指定类型后 可以设置以哪个为主
model="out-in/in-out" 可以设置过渡和动画先后顺序
appear 使第一次渲染也具有过渡或者动画效果	
```

##### 列表动画-状态动画

```vue
<transition-group></transition-group>
使用setInterval改变数据状态
```

##### 第五章

```vue
混入 mixin 
    1.组件data优先级高于mixin的data优先级
    2.mixin的生命周期函数优先于组件的生命周期函数执行
    3.组件methods函数优先级高于mixin的methods函数优先级
    4.使用mixins引用局部mixin数据
    5.app.mixin可以定义全局mixin,不需要使用mixins:[]引入
    6.组件的自定义属性优先级高于mixin
    7.修改自定义属性优先级的方法 
    app.config.optionMergeStragies.number=(mixValue,componentValue)=>{
        return mixValue || componentValue
    }

自定义属性
    1.直接定义在组件/mixin里面的属性
    2.使用this.$options.xxx进行调用

自定义指令 directive 
    1. 指令内生命周期函数 两个参数 el 和 binding 
    2. 可以使用updated对绑定的属性进行渲染
    3. el可以获取dom属性,binding.value 获取绑定值 binding.arg 获取绑定属性
    4. 局部属性使用directives进行指定,在dom上使用 v-xxx 进行调用
    5. 局部变量
    const directive = {
        mounted(el,binding){

        },
        updated(el,binding){

        }
    }
    6. 简写
    app.directive('xxx',(el,binding)=>{
        //相当于 局部变量里面定义 mounted和updated
    })
 传送门 teleport 
  	1. 使用 <teleport to="xxx"></teleport>标签包裹dom元素，并在to中指定所要挂载的dom的id可以将标签内的dom元素挂载到指定的元素上
 render讲解
    1.虚拟dom dom对象的js表述 对真实dom的映射
    2.使vue性能变快
    3.使vue具有跨平台性
    4.{
          tagName:xxx
          attributes:xxx
          text:xxx 可使用[] 数组内可以继续使用h函数嵌套
      }
    5.template->render->h->虚拟DOM(JS对象)->真实DOM->展示到的页面
 插件 plugin
	1.把通用性的功能封装起来
	2.定义插件
	  const myPlugin ={
        install(app,options){
            //传递属性
            app.provide('name','Dell Lee')
            //传递自定义指令
            app.directive('focus',{
                mounted(el){
                    el.focus();
                }
            })
            //传递mixin
            app.mixin({
                mounted(){
                    console.log("mounted")
                }
            })
            app.config.globalProperties.$sayHello = "hello world"
            console.log(app,options)
        }
    }
自定义校验器
	
    const validatePlugin = (app,options)=>{
        app.mixin({
        created(){
            for(let key in this.$options.rules){
                const item = this.$options.rules[key];
                this.$watch(key,(c)=>{
                    const result = item.validate(c);
                    if(!result)console.log(item.message);
                })
            }
        }
    })
    }
```

##### 第六章

```vue
compositionApi
	setup(props,context)
		1. 此函数在实例被初始化完成之前别调用类似 created
		2. 此函数无法使用this
		3. 无法调用根组件methods里面的
		4. 根组件可以调用setup里面的方法和变量 使用 this.调用
		5. 定义 setup(props,context){return {}}
	ref,reactive,readonly,toRefs
	    1.ref reactive 响应式引用
    	2.原理 通过proxy 对数据进行封装 ，当数据变化时，触发模板等内容的更新
    	3.proxy ，'dell' 变成 proxy({value:'dell'}) 这样的一个响应式引用
    	4.ref 处理基础类型的数据 字符串 数字
    	5.reactive 适用于引用数据类型 例如 对象 数组
    	6.readonly可以阻止响应数据被修改
	    7.toRefs可以将响应式对象的属性转换为响应式属性在模板中使用,普通结构无法使用
	toRef,context
		1.toRef 如果响应式对象中没有xxx属性,可以使用const xxx=toRef(xxx,'xxx') 设置属性默认值给模板使用
		2.context 提供了 attrs slots emit 
	computed 计算属性
		1. computed(()=>{return xxx+=1})
		2. computed({
			get:()=>{return xxx+=1},
			set:(param)=>{}
		})
	watch 和 watchEffect
		1. watch(()=>'xxx',(c,o)=>{})
		2. watch([()=>'xxx'...],([c1,c2...],[o1,o2...])=>{})
		3. watch具有惰性,当监听的值发生改变后才会进行调用
		4. watchEffect(()=>{})
		5. watchEffect会立即执行,并且对内部代码进行检查，如果不依赖外部代码就执行一次，如果依赖外部代码则每次都会进行监听
		6. watchEffect 只需要传递一个回调函数
		7. watchEffect 会返回一个停止监听的函数调用则会停止监听操作
		8. watchEfffect无法获取变更前的值,watch可以获得变更前的值
	生命周期函数
        1.  beforeCreate -> 使用 setup()
        2.  created -> 使用 setup()
        3.  beforeMount -> onBeforeMount
        4.  mounted -> onMounted
        5.  beforeUpdate -> onBeforeUpdate
        6.  updated -> onUpdated
        7.  beforeUnmount -> onBeforeUnmount
        8.  unmounted -> onUnmounted
        9.  errorCaptured -> onErrorCaptured
        10. renderTracked -> onRenderTracked
        11. renderTriggered -> onRenderTriggered
        12. activated -> onActivated
        13. deactivated -> onDeactivated
	project inject ref 
		1. project(xxx,xxx) project(xxx,()=>{})
		2. inject('xxx','xxx')
		3. const xxx = ref(null) return {xxx}  <div ref='xxx'/> dom元素就是 xxx.value
```

#####  第七章

```
	vue cli 
	vue 单文件组件
	vue router
	vue X
		1. 为啥使用vuex 
		vuex 数据管理框架
		vuex 创建了一个全局唯一的仓库,用来存储全局唯一的数据
```

