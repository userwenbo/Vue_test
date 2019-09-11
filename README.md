# 1. Vue.js是什么?
	1). 一位华裔前Google工程师(尤雨溪)开发的前端js库
	2). 作用: 动态构建用户界面
	3). 特点:
		* 遵循MVVM模式
		* 编码简洁, 体积小, 运行效率高, 移动/PC端开发
		* 它本身只关注UI, 可以轻松引入vue插件和其它第三库开发项目
	4). 与其它框架的关联:
		* 借鉴angular的模板和数据绑定技术
		* 借鉴react的组件化和虚拟DOM技术
	5). vue包含一系列的扩展插件(库):
		* vue-cli: vue脚手架
		* vue-resource(axios): ajax请求
		* vue-router: 路由
		* vuex: 状态管理
		* vue-lazyload: 图片懒加载
		* vue-scroller: 页面滑动相关
		* mint-ui: 基于vue的组件库(移动端)
		* element-ui: 基于vue的组件库(PC端)

# 2. 基本使用
	1). 引入vue.js
	2). 创建Vue实例对象(vm), 指定选项(配置)对象
		el : 指定dom标签容器的选择器
		data : 指定初始化状态数据的对象/函数(返回一个对象)
	3). 在页面模板中使用{{}}或vue指令

# 3. Vue对象的选项
## 1). el
	指定dom标签容器的选择器
	Vue就会管理对应的标签及其子标签

## 2). data
	对象或函数类型 
	data(){
	  return {
	    
	  }
	}
	指定初始化状态属性数据的对象
	vm也会自动拥有data中所有属性
	页面中可以直接访问使用
	数据代理: 由vm对象来代理对data中所有属性的操作(读/写)

## 3). methods
	包含多个方法的对象   函数要有函数名
	供页面中的事件指令来绑定回调
	回调函数默认有event参数, 但也可以指定自己的参数
	所有的方法由vue对象来调用, 访问data中的属性直接使用this.xxx
	所有vue控制的this都是vm

## 4). computed
	包含多个方法的对象
	对状态属性进行计算返回一个新的数据, 供页面获取显示   减少运算的次数
	一般情况下是相当于是一个只读的属性     初始显示 ，变化显示
	利用set/get方法来实现属性数据的计算读取, 同时监视属性数据的变化
	如何给对象定义get/set属性
		在创建对象时指定: get name () {return xxx} / set name (value) {}
	  	对象创建之后指定: Object.defineProperty(obj, age, {get(){}, set(value){}})

## 5). watch
	包含多个属性监视的对象      根据状态数据显示
	分为一般监视和深度监视
		'xxx' : {
			deep : true,
			handler : fun(value)
		}
	另一种添加监视方式: vm.$watch('xxx', funn)

## Style/class

```
\1. 理解

  在应用界面中, 某个(些)元素的样式是变化的

  class/style绑定就是专门用来实现动态样式效果的技术

\2. class绑定: :class='xxx'

  xxx是字符串

  xxx是对象

  xxx是数组
          div class="classC" :class="myClass">xxxxx</div>  <!-- 类名不确定的 -->
          <div :class="{classA: hasA, classB: hasB}">yyyy</div> <!-- 什么时候用对象语法:  类           名确定, 但    不确定有没有-->
          <div :class="['classA', 'classB']">zzzzzz</div>
\3. style绑定

  :style="{ color: activeColor, fontSize: fontSize + 'px' }"  
  :号已经是动态的
  react中 {{}} 大括号里面写js代码 里面是js对象
  其中activeColor/fontSize是data属性
  下标是数组的属性名
  vue监视的是data中所有层次的数据
  对象：中所有属性是通过setter监视  只能用set
  数组：重写方法
  调用数组原有的对应方法
  更新
  
  data里面的属性都是set监视的
  属性里分数组和对象
```

##  条件渲染

###  条件渲染指令

v-if与v-else   直接删除了   当条件不成立时，v-if的所有子节点不会解析

v-show   只是隐藏起来  如果需要频繁切换v-show较好

### 列表渲染

​    vue监视的是data中所有层次的数据

​    对象: 中所有属性是通过setter监视

​    数组: 重写数组改变数组元素的一系列方法, 来实现监视

​        1). 调用数组原有的对应方法, 对元素进行操作

​        2). 更新界面

```
methods: {
      deleteP (index) {
        this.persons.splice(index, 1)// splice不是数组原生方法
      },

      updateP (index, newP) {
       //  this.persons[index] = newP
        this.persons.splice(index, 1, newP)

        // this.persons[index].name = newP.name
        // this.persons = []
      }
```

### 绑定监听

```
1. 绑定监听:
  v-on:xxx="fun"
  @xxx="fun"
  @xxx="fun(参数)"
  默认事件形参: event
  隐含属性对象: $event
2. 事件修饰符:
  .prevent : 阻止事件的默认行为 event.preventDefault()
  .stop : 停止事件冒泡 event.stopPropagation()
3. 按键修饰符
  .keycode : 操作的是某个keycode值的健
  .enter : 操作的是enter键
```

### 表单输入绑定

```
1. 使用v-model(双向数据绑定)自动收集数据
  text/textarea
  checkbox
  radio
  select
  <form action="/xxx" @submit.prevent='register'>
  <input type="checkbox" id="basket" v-model="user.likes" value="basket">
  <span>城市: </span>
    <select v-model="user.cityId">
      <option value="">未选择</option>
      <option value="city.id" v-for="city in allCitys" :key="city.id">{{city.name}}</option>
    </select><br>
```

# 4. 过渡动画

	利用vue去操控css的transition/animation动画
	模板: 使用<transition name='xxx'>包含带动画的标签
	css样式
		.fade-enter-active: 进入过程, 指定进入的transition
		.fade-leave-active: 离开过程, 指定离开的transition
		.xxx-enter, .xxx-leave-to: 指定隐藏的样式
	编码例子
	    .xxx-enter-active, .xxx-leave-active {
	      transition: opacity .5s
	    }
	    .xxx-enter, .xxx-leave-to {
	      opacity: 0
	    }
	    
	    <transition name="xxx">
	      <p v-if="show">hello</p>
	    </transition>
	 1. vue动画的理解
	  操作css的trasition或animation
	  vue会给目标元素添加/移除特定的class
	2. 基本过渡动画的编码
	  1). 在目标元素外包裹<transition name="xxx">
	  2). 定义class样式
	    1>. 指定过渡样式: transition
	    2>. 指定隐藏时的样式: opacity/其它
	3. 过渡的类名
	  xxx-enter-active: 指定显示的transition
	  xxx-leave-active: 指定隐藏的transition
	  xxx-enter: 指定隐藏时的样式   

# 5. 生命周期
	vm/组件对象
	生命周期图
	beforeCreate()  init 之前执行
	内部是模版元素
	主要的生命周期函数(钩子)
		created() / mounted(): 启动异步任务(启动定时器,发送ajax请求, 绑定监听)
		beforeDestroy(): 做一些收尾的工作
	 vue对象的生命周期
	  1). 初始化显示
	    * beforeCreate()
	    * created()
	    * beforeMount()
	    * mounted()
	  2). 更新状态
	    * beforeUpdate()
	    * updated()
	  3). 销毁vue实例: vm.$destory()
	    * beforeDestory()
	    * destoryed()
	2. 常用的生命周期方法
	  created()/mounted(): 发送ajax请求, 启动定时器等异步任务
	  beforeDestory(): 做收尾工作, 如: 清除定时器	
	     /*
	   第一次显示之后立即执行一次
	   执行异步操作: 启动定时器, 发送ajax请求, 订阅消息
	   */
	   mounted() {
	     this.timeId =setInterval(() => {
	    console.log("---")
	    this.isShow=!this.isShow
	     }, 1000)
	   },


# 6. 自定义过滤器
## 1). 理解
	对需要显示的数据进行格式化后再显示

```
1. 理解过滤器
  功能: 对要显示的数据进行特定格式化后再显示
  注意: 并没有改变原本的数据, 可是产生新的对应的数据
2. 编码
  1). 定义过滤器         左侧表达式的value
    Vue.filter(filterName, function(value[,arg1,arg2,...]){
      // 进行一定的数据处理
      return newValue
    })
  2). 使用过滤器
    <div>{{myData | filterName}}</div>
    <div>{{myData | filterName(arg)}}</div> 通过filterName(arg)参数来指定特定的数据
```

## 2). 编码

	1). 定义过滤器
		Vue.filter(filterName, function(value[,arg1,arg2,...]){
		  // 进行一定的数据处理
		  return newValue
		})
	2). 使用过滤器
		<div>{{myData | filterName}}</div>
		<div>{{myData | filterName(arg)}}</div>

# 7. vue内置指令
  消息订阅与发布 两个操作
	
	v-text/v-html: 指定标签体
		* v-text : 当作纯文本   innerText
		* v-html : 将value作为html标签来解析   innerHTML 
	v-if v-else v-show: 显示/隐藏元素     以空间换时间
		* v-if : 如果vlaue为true, 当前标签会输出在页面中
		* v-else : 与v-if一起使用, 如果value为false, 将当前标签输出到页面中
		* v-show: 就会在标签中添加display样式, 如果vlaue为true, display=block, 否则是none
	v-for : 遍历     
		* 遍历数组 : v-for="(person, index) in persons"   :key="id"  一定情况下可用index
		* 遍历对象 : v-for="(value,key) in person"   $key   
	v-on : 绑定事件监听
		* v-on:事件名, 可以缩写为: @事件名
		* 监视具体的按键: @keyup.keyCode   @keyup.enter
		* 停止事件的冒泡和阻止事件默认行为: @click.stop   @click.prevent
		* 隐含对象: $event
	v-bind : 强制绑定解析表达式  
		* html标签属性是不支持表达式的, 就可以使用v-bind
		* 可以缩写为:  :id='name'       动态获取属性值
		* :class
		  * :class="a"
			* :class="{classA : isA, classB : isB}"
			* :class="[classA, classB]"
		* :style
			:style="{color : color}"
	v-model
		* 双向数据绑定        model-->view 比较重要   单向数据绑定
		* 自动收集用户输入数据
	ref : 标识某个标签
		* ref='xxx'       xxx标签对象放到属性值
		* 读取得到标签对象: this.$refs.xxx
		所有指令都是操作标签的
# 8. 自定义指令
## 1). 注册全局指令
    Vue.directive('my-directive', function(el, binding){
      el.innerHTML = binding.value.toUpperCase()
    })
    el  指定属性所在的标签
    binding 包含指令相关信息数据的对象(value: 表达式对应的值)
    一般名字有特殊字符 用""
    注册全局指令： 对全部vm都有效

```
1. 注册全局指令
  Vue.directive('my-directive', function(el, binding){
    el.innerHTML = binding.value.toupperCase()
  })
2. 注册局部指令
  directives : {
    'my-directive' (el, binding) {
      el.innerHTML = binding.value.toupperCase()
    }
  }
3. 使用指令
  v-my-directive='xxx'
-->
<!--
需求: 自定义2个指令
  1. 功能类型于v-text, 但转换为全大写
  2. 功能类型于v-text, 但转换为全小写
```

## 2). 注册局部指令

    directives : {
      'my-directive' : function(el, binding) {
          el.innerHTML = binding.value.toUpperCase()
      }
    } 
    局部指令：只对当前当前vm有效        指令名不包括v-

## 3). 使用指令
    <div v-my-directive='xxx'>

### 插件

声明使用插件：内部调用插件的install方法来安装插件