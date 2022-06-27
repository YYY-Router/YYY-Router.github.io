##### 1.vue官网的使用方法

[vue.js 官网](https://cn.vuejs.org/)

> + 教程和API：

​				教程帮助你入门，API相当于一本字典，风格指南帮助你规范代码，cookbook是帮助你优化代码的技巧。

> + 工具和核心

​				vue-cli 脚手架 （最好用最新版）

​				Awesome Vue.js 	vue中的现成的库（elementUI等等）

​				vue.js devtools	浏览器插件

​				vscode安装Vue 3 Snippets插件	可以有vue语法自动补全的作用

##### 2.vue起步

> + 引入vue.js
>   + 安装浏览器vue插件	Vue-Devtools 
> + 在脚本中写 Vue.config.productionTip = false  //阻止提示语言（因为使用开发版的vue.js会出相应的提示）

##### 3.vue初识

> + 想要vue工作，必须新建一个vue实例对象
>
> ```vue
> <script>
> new Vue({
> 
> })
> </script>
> ```
>
> + 在这个vue实例中需指定一个容器，让vue为这个容器来服务
>
> ```vue
> <html>
>     <div class="root">
> 
>     </div>
> </html>
> <script>
> new Vue({
>     el:".root"  //el用来指定当前vue为那个容器来服务（这个值通常为css选择器）。也可以用document来选择（但是不常用）
> })
> </script>
> ```
>
> 这个容器由el参数来承接，并且通过css选择器来绑定。

> + root容器里的代码被称为vue模板
> + vue实例中还有个data参数，常设置为对象的形式
>
> ```vue
> <script>
> new Vue({
>     el:".root",
>     data:{
>     name:"tom",
>         age:18,
> }
> })
> </script>
> ```
>
> 在data对象中定义好了数据以后，可以直接给html中的内容使用。采用"{{}}"这种方式来进行数据绑定。
>
> ```html
> <html>
>     <div class="root">
>         <p>
>             我是{{name}}
>         </p>
>     </div>
> </html>
> ```
>
> ###### 细节问题

> +  一对多多对一问题（vue实例和root容器一一对应）

​		由于el参数是根据css选择器来确定服务对象的，当界面中有多个相同的css选择器时，所创建的vue实例只会接管第一个容器，并且不会报错。当一个界面只有一个root容器，但是却有两个vue对象。那么这个容器只会被第一个vue实例所接管。

`总结：vue实例和root容器总是一一对应的，当一个容器被一个对象接管后，其他的对象将无法再接管。`

> + {{  }}中可以放置的东西的类型问题
>
> {{  }}可以写：js表达式，不可以写js代码。
>
> 注意js表达式和js代码（语句）的区别
>
> js表达式：一个表达式会产生一个值，可以放在任何一个需要调用的地方：
>
> (1) a
>
> (2) a+b
>
> (3) demo(1)
>
> (4)x === y ？z
>
> js代码：（语句,只是一个操作过程，并不会产生结果）
>
> (1) if(){}
>
> (2) for(){}

##### 4.模板语法

> + 插值语法（使用在标签体中）
>
>   ​	{{xx}}

> + 指令语法（常使用在标签属性中，但是标签体，标签事件绑定都可以用指令语法）
>
> vue中有许多的指令，在这里用v-bind：来举例。
>
> ```html
> <html>
>     <a v-bind:href="url">1111 </a>
> </html>
> <script>
> new Vue({
>     data:{
>         url:"www.baidu.com"
>     }
> })
> </script>
> ```
>
> 使用 v-bind： 配合“  ”来绑定动态标签属性。
>
> ​    `<a v-bind:href="url">1111 </a>`
>
> v-bind：可以简写为“ ： ”，
>
> ​    `<a :href="url">1111 </a>`

小技巧：可以在对象data中再写对象，这样避免了key发生冲突。

```html
<script>
new Vue({
    data:{
	name:tom,
        school:{
            name:jack
        }
    }
})
</script>

//在调用school对象那个中的name时，在前面写school.name

```

##### 5.数据的绑定

> + 单向数据绑定
>
>   v-bind:
>
>   数据只能从data流向页面
>
>   ```html
>   <html>
>       <a v-bind:href="url">111</a>
>   </html>
>   <script>
>   new Vue({
>       data:{
>           url:"www.baidu.com"
>       }
>   })
>   </script>
>   ```
>
>   简写方式
>
>   ```html
>   <html>
>       <a :href="url">111</a>
>   </html>
>   ```
>
> + 双向数据绑定
>
> v-model: (默认对value属性操作)
>
> 这个绑定方式只能应用在表单类元素（输入性元素即具有value值的元素）,数据不仅能从data流向页面，还可以从页面流向data

```html
<html>
    <input type="text" v-model:value="name">
</html>
<script>
new Vue({
    data:{
        name:"tom"
    }
})
</script>
```

简写方式

```html
<html>
    <input type="text" v-model="name">
</html>
```

##### 6.el和data的两种写法

> + 容器绑定的不同写法

使用`$mount("css选择器名字")`来代替el绑定容器

```html
<script>
const v = new Vue({
    data:{
        
    }
})
v.$mount(".root")
</script>
```

`挂载`

> + data的不同写法
>
> > + 对象式
> >
> > ```html
> > new Vue({
> > data:{
> > 	name:"tom"
> > 		}
> > })
> > ```
>
> > + 函数式
> >
> > ```
> > data:function(){
> > 	return{		//在这个函数中的this指的是这个实例对象
> > 	name:"tom"
> > 		}
> > }
> > ```
> >
> > 简写方式
> >
> > ```
> > data(){
> > 	return{
> > 	name:"tom"
> > 		}
> > }
> > ```
> >
> > ###### 注意由Vue管理的函数，一定不要写成箭头函数。

##### 7.MVVM模型

> + M 模型（Model）：对应data中的数据
> + V 视图（VIew）：模板
> + VM 视图模型（ViewModel）：Vue实例对象

因此经常用vm来代替实例对象

```
const vm = new Vue({

})
```

差值语法中的{{  }}内所有vm身上的属性都可以填在里面，不用写vm.xxx，直接就可以把这些个属性放进去。

<img src="https://yulearn.top/webSource/img/vue%E5%B1%9E%E6%80%A7.png" style="zoom: 50%;" />

```html
<html>
    
    <h1>
        {{$root}}
    </h1>
    <h1>
        {{_isVue}}
    </h1>
</html>
```

##### 8.Object.defineProperty（）方法

为对象添加属性的方式（ES6）

Object.defineProperty(为哪个对象添加属性，添加的属性名，配置项)	

> + 这样新添加的属性是不参与遍历的。如果想让新添加的属性参与遍历，可以为配置项新添属性：enumerable：true

> + 这样添加的属性还不可以在控制台被修改，如果想在控制台可以修改，可以为配置项新添属性：writable：true

> + 这样添加的属性还不可以被删除（delete Person.age 方法），如果想被删除，可以为配置项新添属性：configurable：true

```html
<script>
let Preson ={
    name:"tom"
	};
    Object.defineProperty(Person,age,{
		value:18,
        enumerable:true, //控制属性是否可以枚举（遍历），默认false
        writable:true, //控制属性是否可以被修改，默认false
        configurable:true //控制属性是否可以被删除
	})
</script>

```

对象的遍历

```html
//方法一
<script>
    let Preson ={
    name:"tom"
	};
console.log(Object.keys(Person))
</script>
```

上述方法可以把传入的对象转换为数组，并遍历出来。

```html
//方法二
<script>
    let Preson ={ 
    name:"tom"
	};
    for(let key in Person){
        console.log(Person[key]);
    }
</script>
//遍历对象是key数组时index
```

> + 还可以在Object.defineProperty(为哪个对象添加属性，添加的属性名，配置项)，的配置项中传入一个get函数，这样就可以动态的修改新添的对象的属性的value值。

```html
<script>
    let number = 18;
    let Person = {
        name:"tom",
    };
Object.defineProperty(Person,age,{
    get:function(){
        return nmuber
    }
})
    //每次读取age的属性，都会调用一下这个get函数，这样就可以通过修改number就可以达到动态修改数据的目的
</script>
```

> + 还可以在配置项中传入一个set函数，这个函数会在所增添的新的对象属性被修改时触发，并且会收到修改的具体值。

````html
<script>
   let number = 18;
    let Person = {
        name:"tom",
    };
Object.defineProperty(Person,age,{
    get:function(){
        return nmuber
    }
    set:function(value){
    number = value;
}
})
</script>
````

##### 9.数据代理

通过一个对象，来代理另一个对象中的属性的操作（读/写）。

```html
<script>
let obj = {
    x:100
	}
let obj2 = {
    y:20
	}
Object.defineProperty(obj2,x,{
    get(){
        return obj.x;
    }
    set(value){
    obj.x = value;
	}
})
//这样就通过一个对象来操作另一个对象
</script>
```

##### 10.Vue中的数据代理

```html
<html>
    <h1>
        我是{{name}}
    </h1>
</html>
<script>
const vm = new Vue({
    data:{
        name:"tom"
    }
})
</script>
```

当开始调用（页面渲染）name的值时，Vue对象就开始调用name属性的getter方法，把data.name属性传出去。

`vm._data 就是传入的data对象的值`

```
console.log(vm._data.name)
```

<img src="https://yulearn.top/webSource/img/vue%E6%95%B0%E6%8D%AE%E4%BB%A3%E7%90%86.png" style="zoom:50%;" />

###### 总结：

```html
1.Vue中的数据代理：
通过vm对象来代理data对象中属性的操作（读/写）
2.Vue中数据代理的好处：
更加方便的操作data中的数据
3.基本原理：
通过Object .defineProperty()把data对象中所有属性添加到vm上。为每一个添加到vm上的属性，都指定一个getter/setter。
在getter/setter内部去操作（读/写）data中对应的属性。
```

##### 10.Vue事件处理

> + 绑定单击事件
>
> ```html
> <html>
>     <button v-on:click = "show">点击</button>
> </html>
> <script>
> new Vue({
>     el:".root",
>     data:{
> 
>     },
>     methods:{
>         show(){
>             alert("你好")
>         }
>     }
> })
> </script>
> ```
>
> 绑定单机击事件的函数应该写在methods:{ }方法里面，
>
> 函数也可以传入event（和原生一样）
>
> ```html
> <csript>
> methods:{
> 	show(event){
> 		console.log(event.target) //查看触发单击事件的标签
> 		console.log(this) //此处的this是vm,即vue的实例对象
> 	}
> }
> </csript>
> ```
>
> 所有的vue函数最好写成普通函数，不要用箭头函数。（this的指向会改变）
>
> > + 简写的单击事件绑定
>
> ```html
> <button @click = "show">点击</button>
> ```
>
> > + 绑定单击函数并且传参,在函数名后加上“（）”，在其中写上需要传的参数
>
> ```html
>   <button @click = "show(111)">点击</button>
> <script>
> new Vue({
>     methods:{
>         show(number){
>             console.log(number)
>         }
>     }
> })
> </script>
> ```
>
> > + 如果在传入参数的同时还想再传入event事件属性,则需要用“$event”关键字来进行占位
>
> ```html
>   <button @click = "show(111，$event)">点击</button>
> <script>
> new Vue({
>     methods:{
>         show(number,event){
>             console.log(number)
>             console.log(event) //这个参数就是传入的事件类型
>         }
>     }
> })
> </script>
> ```
>
> 只有配置在data中的东西才会做数据代理，写在methods或者其他属性中的东西，不会做数据代理。

##### 11.事件的修饰符

> + 阻止事件的默认行为（例如：a标签的跳转）只需要在绑定事件后面写上“.prevent”属性即可（这个prevent就是事件修饰符）
>
> ```html
> <a href="baidu.com" @click.prevevt="show">点击</a>
> ```

> + 阻止事件冒泡 只需要在绑定事件后面写上“.stop”属性即可
>
> ```html
> <div @click= "show">
>     <button @click.stop="show">
>         点击
>     </button>
> </div>
> //此时这个按钮的点击事件就不会冒泡到盒子上面
> ```
>
> + 事件只触发一次 只需要在绑定事件后面写上“.once”属性即可

```html
  <button @click.once="show">
        点击
    </button>
//这样这个按钮点击一次后就会变成不可点击的状态
```

> + 事件的捕获（正常的事件委派流程是先捕获再冒泡）加上此属性后会在捕获的同时就开始冒泡。在绑定事件的时候加上“.capture”

```html
<div @click.capture="show(2)">
    <div @click="show(1)">
    </div>
</div>
<script>
new Vue({
    methods:{
        show(num){
            console.log(num)
        }
    }
})
</script>
//如果不加capture属性时，点击子盒子会先输出1，再输出2
//加上以后则会变成先输出2再输出1（因为最外层的盒子在捕获的阶段就会执行）
```

> + 只有event.target是当前操作的元素时，才会触发事件.在绑定事件的时候加上“.self”

```html
<div @click.self="show">
    <button @click="show">
        点击
    </button>
</div>
<script>
new Vue({
    methods:{
        show(evevt){
            console.log(event.target)
        }
    }
})
</script>
```

正常如果不加“.self”属性，点击按钮后，会触发按钮单击事件，然后冒泡到盒子的单击事件。但是两次触发的标签（event.target）都是button元素。

当加上了“.self”属性以后，点击按钮后，会正常从按钮冒泡到盒子，但是由于盒子有了self属性，而点击事件由button标签触发的，不是盒子元素触发的，所以盒子上的单击事件不会执行（在一定程度上解决了事件的冒泡）。

> + 事件的默认行为立即执行，无需等待回调函数执行完再执行.在绑定事件的时候加上“.passive”

```html
<ul @wheel.passive="show">
    
</ul>
```

###### 绑定滚动事件

> + @scroll	此事件可以被鼠标滚轮触发，也可以被键盘上下键的滚动触发（即绑定给了滚动条）

> + @wheel	此事件只可以被鼠标滚轮滚动触发（即使滚动条到头了，再滚动滚动轮也会触发该事件）

##### 12.键盘事件

> + 为输入框绑定键盘事件，并且在按下enter键后才执行,只需在事件后面加上“.enter”

```html
<input type="text" @keyup.enter="show">
<script>
new Vue({
    methods:{
        show(e){
            console.log(e.target.value)
        }
    }
})
</script>
```

###### 上述的方法是Vue给按键配置的别名

Vue中常用的按键别名：

```
回车 => enter
删除 => delete(捕获“删除delete”和“退格backspace”键）
退出 => esc
空格 => space
换行 => tab
上 => up
下 => down
左 => left
右 => right
```

但是可能会用到Vue没有起别名的按键，这事就需要我们自己转化一下，根据按键的名字

```html
<input type="text" @keyup.caps-lock="show">
```

上述例子其实是绑定的CapsLock键。但是想在绑定方法以后使用别名则需要把按键原有的key转化一下

转化规则为：

大写字母变小写字母。如果是多个单词的按键名，则需要把单词和单词之间用“-”来连接

注意：不是所有的按键都可以绑定事件。

> + 不是所有的按键都可以用keyon来绑定

```
例如：

“Tab”键，他原本的功能就是在按下还未抬起的时候就切换键盘焦点，如果此时绑定keyon（按键抬起事件），则不会触发相应的回调函数。此时就需要绑定成keydown事件了

```

> + 系统修饰键（用法特殊）：ctrl、alt、shift、meta(win键)

(1).配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他健，事件才被触发。

例如：按下Ctrl键后，再按s以后才会触发Ctrl绑定的事件。

(2).配合keydown使用：正常触发事件。

> + 也可以配合键码来绑定（keycode）但是极不推荐，因为不同的键盘的键码可能不相同（在Vue3中已经取消了这种用法 ）

> + 我们也可以自己去为某一个按键配置一个别名(但是也不推荐)

```html
<script>
Vue.config.keyCodes.huiche = 13;
</script>
```

##### 13.键盘事件小技巧总结

> + 同时使用两个修饰符（例如：阻止默认事件和冒泡事件）

```html
<a href="baidu.com" @click.prevent.stop="show">点击</a>
```

修饰符的顺序也会影响事件的发生，比如上述案例就是先阻止的默认事件，再阻止的事件冒泡。

> + 输入框绑定按键的别名也可以绑定多个

```html
<input type="text" @keyup.ctrl.y="show">
```

上述案例就是在满足按下Ctrl和y按键后才会触发回调函数。

##### 14.计算属性

###### Vue对象新增计算属性：computed:{ }

```html
<span>{{fullname}}</span>
<script>
const vm = new Vue({
    el:".root",
    data:{
        firstname:"张",
        lastname:"三"
    },
    methods:{
        
    },
    computed:{
        fullname:{
            get(){  //get的作用是当有人读取fullname时，get就会被调用，并且返回值就是fullname的值
                return this.firstname+'-'+this.lastname; //get中的this指向就是vm（vue底层做的工作）
            }
        }
    }
})
//get的调用时机：
	//1.初次读取fullname时
	//2.计算时所依赖的数据被改变时
//这样做是为了时时的更新数据。
</script>
```

###### computed还具有缓存属性

（相较于用methods方法实现的优势）

```html
<span>{{fullname}}</span>
<span>{{fullname}}</span>
<span>{{fullname}}</span>
<span>{{fullname}}</span>
<script>
const vm = new Vue({
    el:".root",
    data:{
        firstname:"张",
        lastname:"三"
    },
    methods:{
        
    },
    computed:{
        fullname:{
            get(){  //get的作用是当有人读取fullname时，get就会被调用，并且返回值就是fullname的值
                return this.firstname+'-'+this.lastname; //get中的this指向就是vm（vue底层做的工作）
            }
        }
    }
})
```

即使页面中有多处用到了fullname数据，computed中的fullname也只会调用一次get方法，剩下的fullname数据渲染都使用的缓存。

> + 当fullname会被修改时，则还需要调用set方法

```html
<span>{{fullname}}</span>
<script>
const vm = new Vue({
    el:".root",
    data:{
        firstname:"张",
        lastname:"三"
    },
    methods:{
        
    },
    computed:{
        fullname:{
            get(){  //get的作用是当有人读取fullname时，get就会被调用，并且返回值就是fullname的值
                return this.firstname+'-'+this.lastname; //get中的this指向就是vm（vue底层做的工作）
            },
            set(value){
                const arr = value.split('-')//数组的方法，根据字符来拆分
                this.firstname = arr[0];
                this.lastname = arr[1]
            }
        }
    }
})
```

###### 总结：

```
计算属性：
1.定义：要用的属性不存在，要通过已有属性（在vue中的数据）计算得来。
2.原理：底层借助了Objcet.defineproperty方法提供的getter和setter。
3.get函数什么时候执行？
(1).初次读取时会执行一次。
(2).当依赖的数据发生改变时会被再次调用。
4.优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便。
5.备注：
    a.计算属性最终会出现在vm上，直接读取使用即可。//使用插值语法时直接读取，不用写vm.
    b.如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时所依赖数据发生变化
```



##### 15.计算属性的简写（用函数形式）

不用把fullname写成对象，直接把他写成函数

```html
<span>{{fullname}}</span>  //调用的时候还是这样，而不是调函数
<script>
const vm = new Vue({
    el:".root",
    data:{
        firstname:"张",
        lastname:"三"
    },
    methods:{
        
    },
    computed:{
        fullname(){
              return this.firstname + '-' + this.lastname;
        }
    }
})
```

##### 16.监视属性

> + 小技巧
>
> 当帮绑定的函数实现的功能很简单，可以直接把代码语句写在@click的后面。但是当实现的功能多的时候，还是要写在methods中。
>
> ```html
> <button @click="ishot = !ishot">点我</button>
> ```

但是模板里并不是所有的方法都存在，比如：alert（）在vm上就没有。所以不能把他写在@click后面。

###### Vue实现监视的配置对象	

> + 监视方法一	watch:{ }

```html
<script>
const vm = new Vue({
    el:".root",
    data:{
        isHot:true
    },
    computed:{},
    methods:{},
    watch:{	//Vue的监视属性
        isHot:{
            immediate:true, //该配置项默认是false，配置成true后会在初始化的时候调用一下handler函数
            handler(newValue,oldValue){	//当isHot的值发生改变时handler函数会被调用
                console.log("isHot被修改了",newValue,oldValue)
            }
        }
    }
})
</script>
```

watch不光能监视data属性中的数据变化，还可以监视computed属性中数据的变化。

```html
<script>
const vm = new Vue({
    el:".root",
    data:{ isHot:true},
    computed:{
        info(){
            return this.isHot ? "炎热" : "凉爽" 
        }
    },
    methods:{},
    watch:{	//Vue的监视属性
        info:{
            immediate:true, //该配置项默认是false，配置成true后会在初始化的时候调用一下handler函数
            handler(newValue,oldValue){	//当isHot的值发生改变时handler函数会被调用
                console.log("info被修改了",newValue,oldValue)
            }
        }
    }
})
</script>
```

> + 监视方法二	vm.$watch('被监视的数据名',{配置对象})

```html
<script>
const vm = new Vue({
    el:".root",
    data:{ isHot:true},
    computed:{
        info(){
            return this.isHot ? "炎热" : "凉爽" 
        }
    },
    methods:{},
})
vm.$watch('isHot',{
     immediate:true, //该配置项默认是false，配置成true后会在初始化的时候调用一下handler函数
            handler(newValue,oldValue){	//当isHot的值发生改变时handler函数会被调用
                console.log("isHot被修改了",newValue,oldValue)
})
</script>
```

###### 总结：

```
监视属性watch:
1.当被监视的属性变化时，回调函数自动调用，进行相关操作（data，computed）
2.监视的属性必须存在，才能进行监视！！（如果属性不存在也不会报错）
3.监视的两种写法：
(1).new Vue时传入watch配置
(2).通过vm.$watch监视
```

##### 17.深度监视

> + 监视多级结构中某个属性的变化
>
> ```html
> <script>
> const vm = new Vue({
>     el:".root",
>     data:{
>         isHot:true,
>         numbers:{
>             a:1,
>             b:2
>         }
>     },
>     computed:{},
>     methods:{},
>     watch:{	//Vue的监视属性
>         isHot:{	//这种是简写方式，补全是 'isHot'（带有引号）
>             handler(newValue,oldValue){	//当isHot的值发生改变时handler函数会被调用
>                 console.log("isHot被修改了",newValue,oldValue)
>             },
>             'numbers.a':{	//多级结构中的深度监视
>                      handler(newValue,oldValue){	//当isHot的值发生改变时handler函数会被调用
>                 console.log("a被修改了",newValue,oldValue)
>             }
>             }
> 
>         }
>     }
> })
> </script>
> ```
>
> 上述方法只是监听了numbers中的a属性，但是当numbers中的值变多的时候如果还是这样'numbers.a'这样来监视会很麻烦。这时候就引入了一个watch属性中的新的配置项	deep
>
> ```html
> <script>
> const vm = new Vue({
>     el:".root",
>     data:{
>         isHot:true,
>         numbers:{
>             a:1,
>             b:2
>         }
>     },
>     computed:{},
>     methods:{},
>     watch:{	//Vue的监视属性
>         isHot:{	//这种是简写方式，补全是 'isHot'
>             handler(newValue,oldValue){	//当isHot的值发生改变时handler函数会被调用
>                 console.log("isHot被修改了",newValue,oldValue)
>             },
>            numbers:{
>                deep:true,	//默认是false，但是设置为true以后就会对numbers中的所有属性进行监测，而并不是单纯监测numbers是否改变
>                    handler(newValue,oldValue){	//当isHot的值发生改变时handler函数会被调用
>                 console.log("numbers中的内容被修改了",newValue,oldValue)
>            }
>         }
>     }
> })
> </script>
> ```

###### 总结：

```
深度监视：
(1).Vue中的watch默认不监测对象内部值的改变（一层）。
(2).配置deep:true可以监测对象内部值改变（多层）。
备注：
(1).Vue自身可以监测对象内部值的改变，但Vue提供的watchs属性默认不可以!
(2).使用watch时根据数据的具体结构，决定是否采用深度监视。
```



##### 18.监视属性的简写形式

###### 简写形式使用的前提是只使用handler函数，而不需要其他的配置项。

满足上述条件时就可以简写，如果有其他的配置项切记不要进行简写。

> + new Vue时传入watch配置的简写

```html
<script>
const vm = new Vue({
    el:".root",
    data:{ isHot:true},
    computed:{
        info(){
            return this.isHot ? "炎热" : "凉爽" 
        }
    },
    methods:{},
    watch:{	
        //完整写法
        info:{
            immediate:true, //该配置项默认是false，配置成true后会在初始化的时候调用一下handler函数
            handler(newValue,oldValue){	//当isHot的值发生改变时handler函数会被调用
                console.log("info被修改了",newValue,oldValue)
            }
        },
        
        //简写形式
        info(newValue,oldValue){
    console.log("info被修改了",newValue,oldValue)
    		}
        //上面这个函数就当handler来用
})
</script>
```

> + .通过vm.$watch监视的简写

```html
<script>
const vm = new Vue({
    el:".root",
    data:{ isHot:true},
    computed:{
        info(){
            return this.isHot ? "炎热" : "凉爽" 
        }
    },
    methods:{},
})
vm.$watch('isHot',function(newValue,oldValue){	//注意这的函数不可以是箭头函数（this指向问题）
      console.log("isHot被修改了",newValue,oldValue)
})
</script>
```

##### 19.computed属性与watch属性的对比

```
computed和watch之间的区别：
1.computed能完成的功能，watch都可以完成。
2.watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作(比如计时器)。
两个重要的小原则：
1.所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象。
2.所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数,Promise的回调函数等），最好写成箭头函数，
这样this的指向才是vm 或 组件实例对象。
//目标只有一个，让函数的this指向vm
```

##### 20.Vue绑定class样式

> + 绑定class样式--字符串写法，适用于样式的类名不确定，需要动态指定的场景
>
> ```html
> <html>
> <style>
>     .basic{
>         width:200px;
>         height:200px;
>     }
>     .happy{
>         background-color:red;
>     }
>     .sad{
>          background-color:blue;
>     }
>     </style>   
>     <div class="root">
>             <div class="basic" :class="mood" @click="changemood"></div>	//这里便用到了动态样式绑定
>     </div>
> </html>
> <script>
> const vm = new Vue({
>     el:".root",
>     data:{
>         mood:''
>     },
>     methods:{
>         changemood(){
>           const arr = ['happy','sad'];
>             const index = Math.floor(Math.random()*2)	//random会生成一个0到1但是不包括1的随机数，floor会向下取整
>             this.mood = arr[index]
>         }
>     }
> })
> </script>
> ```

> + 绑定class样式--数组写法，适用于要绑定的样式个数不确定，名字也不确定的场景
>
> ```html
> <html>
> <style>
>     .basic{
>         width:200px;
>         height:200px;
>     }
>     .happy{
>         background-color:red;
>     }
>     .sad{
>          background-color:blue;
>     }
>     </style>   
>     <div class="root">
>             <div class="basic" :class="classArr"></div>	//这里便用到了动态样式绑定
>     </div>
> </html>
> <script>
> const vm = new Vue({
>     el:".root",
>     data:{
>      classArr:['happy','sad']
>     }
> })
> </script>
> ```

> + 绑定class--对象写法，适用于要绑定的样式个数和名字都确定，但是要动态的决定用不用的场景
>
> ```html
> <html>
> <style>
>     .basic{
>         width:200px;
>         height:200px;
>     }
>     .happy{
>         background-color:red;
>     }
>     .sad{
>          background-color:blue;
>     }
>     </style>   
>     <div class="root">
>             <div class="basic" :class="classObject"></div>	//这里便用到了动态样式绑定
>     </div>
> </html>
> <script>
> const vm = new Vue({
>     el:".root",
>     data:{
>      classObject:{
>          happy:false,	//当值为false时将不会执行该样式
>          sad:false,
>      }
>     }
> })
> </script>
> ```

##### 21.Vue绑定style样式

原始css中的style的属性名会有 “ - ”，这在vue中是不合法的，需要把 “ - ”去掉，并把属性名改成驼峰写法

```html
<div class="root">
    <div :style="styleObject"></div>
</div>
<script>
const vm = new Vue({
    el:".root",
    data:{
        styleObject:{
            fontSize:"20px"，	//这里的css样式的属性名需要改成驼峰命名法
            color:"red"
        }
    }
})
</script>
```

###### 总结：

```
绑定样式
1．class样式
写法：class="xxx" xxx可以是字符串、对象、数组。
字符串写法适用于：类名不确定，要动态获取。
对象写法适用于：要绑定多个样式，个数不确定，名字也不确定。
数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。
2．style样式
:style="{fontSize：xxx}"其中xxx是动态值。
:style="[a,b]"其中a、b是样式对象。
```



##### 22.条件渲染（v-if）

> + v-show=" " 指令做条件渲染（虽然元素不显示但是节点还是存在的）

```html
<div class="root">
    <h1 v-show="true">
        hello world
    </h1>
</div>
```

当v-show后的值为true时，元素显示，反之则不显示。（底层的实现原理就是display:none；）

v-show="  "中还可以是一个表达式，只要能计算出真假即可

```html
<h1 v-show="1===1">
    hello world
</h1>
```

v-show="  "中还可以是一个变量，可以通过data中的数据来实现动态的操作

> + v-if=" " 指令做条件渲染（元素不显示，并且节点也不存在了）

```html
<div class="root">
    <h1 v-if="true">
        hello world
    </h1>
</div>
```

v-if后的值为true时显示，反之不显示。其他的属性和v-show一样。

上述两种方法的使用场景：

当切换频率高的时候用v-show,切换频率低的时候用v-if。

```html
<div v-if="true"></div>
<div v-else-if="true"></div>
<div v-else-if="true"></div>
<div v-else></div>
//上面的是一组的不可以被打断，即在他们之间再加别的元素
//上面是一组判断，用好了可以提高性能
```

###### 新增标签<template> </template>

可以和v-if连用（不可以和v-show连用），来达到控制元素隐藏还是显示。

他的好处是不会增加新的元素（如果用div套起来，会增添了新的元素）

```html
<template v-if="true">
<h1>我是一段话</h1>
    <h1>我是一段话</h1>
    <h1>我是一段话</h1>
    <h1>我是一段话</h1>
</template> 
```

###### 总结：

```
条件渲染：
1.v-if
写法：
(1).v-if="表达式"
(2).v-else-if="表达式"
(3).v-else="表达式"
适用于：切换频率较低的场景。
特点：不展示的DOM元素直接被移除。
注意：v-if可以和:v-else-if、v-else
起使用，但要求结构不能被“打断”
2.v-show
写法：v-show="表达式"
适用于：切换频率较高的场景。
特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉
3.备注：使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到。
```

##### 23.列表渲染

> + v-for 渲染（遍历数组）
>
> v-for加载在谁身上谁就遍历
>
> ```html
> <html>
>     <div class="root">
>         <ul>
>          <li v-for="p in persons" :key="p.id">	//这里的key是每个元素的标识（身份信息）
>             {{p.name}}-{{p.age}}	//这里的p是定义的形参，直接就是数组元素
>             </li>
>         </ul>
>     </div>
> </html>
> <script>
> const vm = new Vue({
>     el:".root",
>     data:{
>         persons:[
>             {id:001,name:"tom",age:18},
>             {id:002,name:"jack",age:20},
>             {id:003,name:"jerry",age:21}
>         ]
>     }
> })
> </script>
> ```
>
> v-for还可以传两个参数，v-for="(a,b) in 数组名"。参数a是数组元素，参数b是是数组元素的索引。
>
> ```html
> <html>
>     <div class="root">
>         <ul>
>          <li v-for="(p,index) in persons" :key="index">	//注意这里的in也可以换成of
>                 <li v-for="(p,index) of persons" :key="index">	
>             {{p.name}}-{{p.age}}	
>             </li>
>         </ul>
>     </div>
> </html>
> <script>
> const vm = new Vue({
>     el:".root",
>     data:{
>         persons:[
>             {id:001,name:"tom",age:18},
>             {id:002,name:"jack",age:20},
>             {id:003,name:"jerry",age:21}
>         ]
>     }
> })
> </script>
> ```

> + v-for渲染（遍历对象）
>
> ```html
> <html>
>     <div class="root">
>         <ul>
>          <li v-for="(value,key) in cars" :key="key">	//这里的key是每个元素的标识（身份信息）
>           {{key}}--{{value}}
>             </li>
>         </ul>
>     </div>
> </html>
> <script>
> const vm = new Vue({
>     el:".root",
>     data:{
>      cars:{
>          name:"奥迪A8",
>          price:"70w",
>          color:"black"
>      }
>     }
> })
> </script>
> ```
>
> 在遍历对象的时候，也会有两个参数。第一个参数收到对象的value值，第二个参数收到key值。

> + v-for渲染（渲染字符串）用得少

> + v-for渲染（遍历指定次数）用得少
>
> ```html
> <ul>
>     <li v-for="(number,index) in 5" :key="index">
>     {{number}}--{{index}}
>     </li>
>     //这个的的遍历效果是number从1开始到五，index的效果是从0到4
> </ul>
> ```

######  总结：

```
v-for指令：
1.用于展示列表数据
2.语法：v-for="(item, index) in xxx"  :key="yyy
3.可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）
```

##### 24.v-for 中的key

key的作用是给每一个循环的节点添加唯一的标识。

```
面试题：react、vue中的key有什么作用？（key的内部原理）
1．虚拟DOM中key的作用：
key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】，
随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：
2.对比规则：
(1).旧虚拟DOM中找到了与新虚拟DOM相同的key：
a.若虚拟DOM中内容没变，直接使用之前的真实DOM!
b.若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
(2).旧虚拟DOM中未找到与新虚拟DOM相同的key
创建新的真实DOM，随后渲染到到页面。
3．用index作为key可能会引发的问题：
1．若对数据进行：逆序添加、逆序删除等破坏顺序操作：
会产生没有必要的真实DOM更新 ==> 界面效果没问题，但效率低。
2．如果结构中还包含输入类的DOM：
会产生错误DOM更新 ==> 界面有问题。
4．开发中如何选择key？；
1.最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值。
2.如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的。
```

##### 25.列表的过滤

###### 用watch属性实现一个模糊搜索的功能

```html
<html>
    <div class="root">
        <input type="text">
    <ul>
        <li v-for="p in filpersons" :key="p.id">
        {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
    </ul>
    </div>
</html>
<script>
const vm = new Vue({
    el:".root",
    data:{
        keyword:"",
        persons:[
            {id:001,name:"周杰伦",age:19,sex:"男"}，
            {id:002,name:"马冬梅",age:20,sex:"女"}，
   		    {id:003,name:"周冬雨",age:18,sex:"女"}，
            {id:004,name:"温兆伦",age:30,sex:"男"}，
        ],
            filpersons:[]
    },
        watch:{
            keyword:{
                immediate:true,//在一开始就调用一次监视
                handler(val){
               this.filpersons = this.persons.filter((p)=>{	//filter方法可以实现对数组的过滤，不修改原数组,返回一个只包含指定元素的新的数组
                    return p.name.indexOf(val) !== -1 	//indexOf方法是判断一个字符串是否包含某个指定字符，不含有返回-1，含有返回位置索引
                })	
                }			
            }
        }
})
</script>
```

###### 用computed实现一个模糊搜索的功能

```html
<html>
    <div class="root">
        <input type="text">
    <ul>
        <li v-for="p in filpersons" :key="p.id">
        {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
    </ul>
    </div>
</html>
<script>
const vm = new Vue({
    el:".root",
    data:{
        keyword:"",
        persons:[
            {id:001,name:"周杰伦",age:19,sex:"男"}，
            {id:002,name:"马冬梅",age:20,sex:"女"}，
   		    {id:003,name:"周冬雨",age:18,sex:"女"}，
            {id:004,name:"温兆伦",age:30,sex:"男"}，
        ]
        
    },
        computed:{
            filperson(){
                return this.persons.filter((p)=>{
                    return p.name.indexOf(this.keyword) !== -1
                })
            }
        }
        )}
```

##### 26.列表的排序

使用computed来实现

```html
<html>
    <div class="root">
        <input type="text" V-model="keyword">
         <button @click="sortType = 2">年龄升序</button>	//这里不需要写this
         <button @click="sortType = 1">年龄降序</button>
         <button @click="sortType = 0">原顺序</button>
    <ul>
        <li v-for="p in filpersons" :key="p.id">
        {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
    </ul>
    </div>
</html>
<script>
const vm = new Vue({
    el:".root",
    data:{
        sortType:"0",//0代表原顺序，1降序，2升序
        keyword:"",
        persons:[
            {id:001,name:"周杰伦",age:19,sex:"男"}，
            {id:002,name:"马冬梅",age:20,sex:"女"}，
   		    {id:003,name:"周冬雨",age:18,sex:"女"}，
            {id:004,name:"温兆伦",age:30,sex:"男"}，
        ]
        
    },
        computed:{
            filpersons(){
                const arr = this.persons.filter((p)=>{
                    return p.name.indexOf(this.keyword) !== -1
                })
                if(this.sortType){
                    arr.sort((a,b)=>{	//sort是对数组排序，return a-b就是升序，return b-a就是降序
                        return this.sortType === 1 ? a.age-b.age : b.age-a.age
                    })
                }
                return arr
            }
        }
        )}
```

##### 27.Vue监测数据更新的原理

###### Vue.set( )的使用

后添加的数据也可以实现数据的响应式（实时更新页面数据）

```
参数：
Vue.set(想要增添新属性的对象,属性的key,属性的value)
Vue.set(vm._data.student,'sex','女)
简写：
Vue.set(vm.student,'sex','女)	//数据代理
```

###### vm.$set( )的使用

```
参数：
vm.$set(想要增添新属性的对象,属性的key,属性的value)
vm.$set(vm._data.student,'sex','女)
简写：
vm.$set(vm.student,'sex','女')	//数据代理
```

但是上述两种方法具有局限性，他们只能给data里面的对象添加属性，不能给data对象直接添加属性。

###### 数组实现数据响应

(注意直接对数组元素赋值是不会达到数组的响应式的)

- `push()`//将一个或多个元素添加到数组的末尾，并返回该数组的新长度
- `pop()`//从数组中删除最后一个元素，并返回该元素的值
- `shift()`//在数组末尾添加元素
- `unshift()`//在数组首位添加元素
- `splice()`//删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容
- `sort()`//对数组的元素进行排序，并返回数组(a和b的故事)
- `reverse()`//将数组中元素的位置颠倒，并返回该数组

只有用上述的几种变更方法操作数组，才可以达到数据的响应式。（因为他们都引起原数组的变化了）

相比之下，也有非变更方法，例如 `filter()`、`concat()` 和 `slice()`。它们不会变更原始数组，而**总是返回一个新数组**。当使用非变更方法时，可以用新数组替换旧数组

```
example1.items = example1.items.filter(function (item) {	//这里把返回的数组重新赋给原数组，这样也可以达到数组数据的响应式
  return item.message.match(/Foo/)
})
```



注意：使用Vue.set( )方法也可以达到数组的数据响应式

```
Vue.set(vm._data.student.hobby,1,'打台球')
vm.$set(vm._data.student.hobby,1,'打台球')
简写：
//数据代理
vm.student.hobby,1,'打台球'
```

##### 28.vue监视数据原理总结

```
Vue监视数据原理
1．vue会监视data中所有层次的数据。
2．如何监测对象中的数据？
通过setter实现监视，且要在new Vue时就传入要监测的数据。
(1).对象中后追加的属性，Vue默认不做响应式处理
(2).如需给后添加的属性做响应式，请使用如下API：
Vue.set(target，propertyName/index，value）或
vm.$set(target, propertyName/index, value)
3．如何监测数组中的数据？
通过包裹数组更新元素的方法实现，本质就是做了两件事：
(1).调用原生对应的方法对数组进行更新。
(2).重新解析模板，进而更新页面。
4.在Vue修改数组中的某个元素一定要用如下方法：
1.使用这些API:push()、pop()、shift()、unshift()、splice(）、sort()、reverse()
2.Vue.set(）或vm.$set()
特别注意：Vue.set(）和 vm.$set(）不能给vm 或 vm的根数据对象 添加属性！！！
```

###### 数据劫持

##### 29.表单数据收集

###### 表单小技巧：

输入框失去焦点事件：@blur="  "

表单中的单选框（radio类），如果想达到一个勾选一个取消的效果，可以不用js来控制。直接为二者起一个相同的name即可。

```html
男：<input type="radio" name="sex">
女：<input type="radio" name="sex">
```

表单下拉框实现

```html
<select>
      <option value="">选择地区</option>
      <option value="beijing">北京</option>
      <option value="beijing">北京</option>
      <option value="beijing">北京</option>
      <option value="beijing">北京</option>
</select>
```

多行信息输入

```html
<textarea></textarea>
```

输入框输入信息获取

```html
<input type="text" v-model="name">
<script>
const vm = new Vue({
    data:{
        name:""
    }
})
</script>
```

单选框获取输入信息(亲自配置value)

```html
男：<input type="radio" name="sex"  v-model="sex" value="male">
女：<input type="radio" name="sex" v-model="sex" value="female">
<script>
const vm = new Vue({
    data:{
       sex:""
    }
})
</script>
```

多勾选框获取输入信息（checkbox)	多组的勾选框要把model绑定的数据用数组来接，并且要手动添加value

```html
学习<input type="checkbox"  v-model="hobby" value="learn">
吃饭<input type="checkbox"  v-model="hobby" value="eat">
<script>
const vm = new Vue({
    data:{
     hobby:[]
    }
})
</script>
```

下拉框获取输入信息

```html
<select v-model="address">
      <option value="">选择地区</option>
      <option value="beijing">北京</option>
      <option value="shanghai">上海</option>
      <option value="wuhan">武汉</option>
</select>
<script>
const vm = new Vue({
    data:{
     address:""
    }
})
</script>
```

多文本框获取信息和普通输入框一样

一个多勾选框获取输入信息（可以不用创建数组来承接）

```html
<input type="checkbox"  v-model="agree">
<script>
const vm = new Vue({
    data:{
    agree:""
    }
})
</script>
//这个agree会接到true或者false。与其checked属性有关
```

表单中的按钮元素即使不绑定事件也会执行提交动作

可以给form标签绑定一个@submit事件（但是表单的默认行为是提交后刷新，此时可以用事件修饰来阻止默认行为）

```html
<form @submit.prevent="demo">
    <button>提交</button>
</form>
<script>
    new Vue({
        data:{
            user:{
                account:"",
                agree:"",
                hobby:[],
                sex:""
            }
        }
        methods:{
            demo(){
                console.log(JSON.stringify(this.user))//直接把对象转为JSON来输出
            }
        }
    })
</script>
```

输入数字的表单标签

```html
<input type="number" v-model="age">
const vm = new Vue({
    data:{
    age:""
    }
})
```

###### v-model的修饰符

> + .number
>
> 解决问题类型：number类的输入框存在一个问题。当输入数据后，age接收到的类型为字符串，并非是数字类型。这时候需要.number修饰符来转化

```html
<input type="number" v-model.number="age">
const vm = new Vue({
    data:{
    age:""
    }
})
```

这样就保证输入的值为纯数字。

> + .lazy
>
> 解决问题：多文本框没必要实时的收集输入的数据，只需要在文本框失去焦点的时候再进行收集即可。这就可以使用.lazy修饰符

```html
<textarea v-model.lazy="text"></textarea>
```

> + .trim
>
> 解决问题：移除输入的内容两边的空格

```html
<input type="text" v-model.trim="name">
```

###### 总结：

```
收集表单数据：
若：<input type="text"/>，则v-model收集的是value值，用户输入的就是value值。
若：<input type="radio"/>，则v-model收集的是value值，且要给标签配置value值。
若：<input type="checkbox"/>
1.没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
2.配置input的value属性:
(1)v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
(2)v-model的初始值是数组，那么收集的的就是value组成的数组
主：v-model的三个修饰符：
lazy: 失去焦点再收集数据
number：输入字符串转为有效的数字
trim：输入首尾空格过滤
```

##### 30.Vue过滤器

###### 好用的第三方库网站：[BootCDN - Bootstrap 中文网开源项目免费 CDN 加速服务](https://www.bootcdn.cn/#about)

时间戳处理库：day.js

###### 局部过滤器（仅在同一个vm中起作用）

Vue对象新添过滤器属性 filters:{ }

```html
<h3>
    {{time | timeFormater}} //用管道符定义一个过滤器名字
</h3>
<script>
new Vue = ({
    data:{
        time:""
    }
    fliters:{
        timeFormater(value){//过滤器会把返回的值代替time显现在界面上
            return dayjs(value).format("YYY/MM/DD HH:mm:ss")
        }
    }
})
</script>
```

过滤器收到的第一个参数永远都是管道符前面的数据，第二个参数是指定转化的格式。

###### 局部过滤器之间可以串联

```html
<h3>
    {{time | timeFormater | myslice}} //用管道符定义一个过滤器名字
</h3>
<script>
new Vue = ({
    data:{
        time:""
    }
    fliters:{
        timeFormater(value){//过滤器会把返回的值代替time显现在界面上
            return dayjs(value).format("YYY/MM/DD HH:mm:ss")
        },
        myslice(value){
            return value.slice(0,4)
        }
    }
})
//先执行timeFormater，把过滤出来的结果再传给myslice，再过滤一下，再把最终的数据渲染到界面上
</script>
```

###### 全局过滤器(必须在创建vue对象之前写过滤器)

```html
<h1>
    {{name | myslice}}
</h1>
<script>
Vue.filter('myslice',function(value){
 	return value.slice(0,4)
})
new Vue({
	data:{
    	name:"tom123"
	}
})
</script>
```

###### 总结：

```html
过滤器：
定义：
对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）
语法：
1.注册过滤器：Vue.filter(name,callback) 或 new Vue{filters:{}}
2.使用过滤器：{{ xxx | 过滤器名}} 或 v-bind:属性 = "xxx | 过滤器名"
备注：
1.过滤器也可以接收额外参数、多个过滤器也可以串联
2.并没有改变原本的数据，是产生新的对应的数据
```

##### 31.Vue内置指令

> + v-text 指令(不如{{ }}常用)
>
> 向其所在的节点中渲染文本内容（不支持结构的解析）
>
> ```html
> <div v-text="name"></div>
> <script>
> new Vue({
>     data:{
>         name:''
>     }
> })
> </script>
> ```

注意：v-text不能解析标签，就当成正常结构解析

> + v-html
>
> 向其所在的节点中渲染内容（支持结构的解析）
>
> ```html
> <div v-html="str"></div>
> <script>new
>     Vue({    
>         data:{  
>         str:'<h3>你好</h3>'    
>     		}
>         })
> </script>
> ```

###### cookie的工作原理

通过js拿到网站的cookie

```
document.cookie
```

###### v-html总结：

```
v-html指令：
1.作用：向指定节点中渲染包含html结构的内容。
2.与插值语法的区别：
(1).v-html会替换掉节点中所有的内容，{{xx}}则不会。
(2).v-html可以识别html结构。
3.严重注意：v-html有安全性问题！！！！
(1).在网站上动态渲染任意HTML是非常危险的，容易导致xss攻击。
(2).一定要在可信的内容上使用v-html，永不要用在用户提交的内容！
```

> + v-cloak指令

css属性选择器

```html
<style>
    [v-cloak]{//选中标签中含有v-cloak属性的元素
        
    }
</style>
```

该属性可以解决当网速慢vuejs未加载的时候页面展示问题

###### v-cloak总结：

```
V-cloak指令（没有值）：
1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
2.使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题。
```

```html
//示例
<style>
    [V-cloak]{
        display:none;
    }
</style>
<h1 V-cloak>{{name}}</h1>
<script src=".././Vue.js"></script>//现在认为Vue.js加载缓慢
<script>
	new Vue({
   	 data:"name"
	})
</script>
```

上述效果就是在vue实例未接管前，界面不显示。在接管后V-cloak属性自动去除。

> + v-once指令
>
> ```html
> <h1 v-once>初始化值:{{n}}</h1>//即使n再次改变，有v-once的标签也不会重新渲染
> <h1>当前的值:{{n}}</h1>
> <button @click="n++">点击加1</button>
> <script>
> new Vue({
>     data:{
>         n:1
>     }
> })
> </script>
> ```

###### v-once总结：

```
v-once指令：
1.v-once所在节点在初次动态渲染后，就视为静态内容了。
2.以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。
```

> + v-pre
>
> ```html
> <h1 v-pre>vue很简单</h1>
> <h1 v-pre>当前的值:{{n}}</h1> //如果在有vue语法的标签上加上这个属性，那么该处的vue语法将不会被视为vue语法，直接会在界面上渲染出{{}}等内容
> <button @click="n++">点击加1</button>
> <script>
> new Vue({
>     data:{
>         n:1
>     }
> })
> </script>
> ```

###### v-pre总结：

```
v-pre指令：
1.跳过其所在节点的编译过程，vue不去解析他了。
2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译。
```

##### 32.Vue自定义指令（js原生操作）

###### 使用Vue对象的directives:{ }属性

在上述的这个指令中进行自定义指令。现在以v-big为例，要求他可以给绑定的数值放大十倍

```html
<h2>当前值为：<span v-text="n"></span></h2>
<h2>放大十倍的值为：<span v-big="n"></span></h2>
<script>
new Vue({
    data:{
        n:1
    }
    directives:{
    //big函数何时被调用？
  	//1.当指令与元素绑定成功时会被调用（一上来）
    //2.指令所在的模板被解析时big方法就会被调用（并不是绑定数据发生改变才会被调用）
        big(element,binding){//两个参数，第一个是自定义指令所绑定的真实DOM，第二个参数是指令的信息（含有value）
            element.innerText = binding.value*10
        }
    }
})
</script>
```

注意：应用标签的时候加上前缀v-，定义指令的时候直接写函数名。

###### vue提供的三个钩子函数

适用情况：函数方法自定义指令，不能满足需求，这时候就需要用到。（细化执行时间）

```html
//现在需要实现让输入框上来就获取焦点的需求，定义一个fbind指令来实现
<h2>vue</h2>
<input v-fbind="n">
<script>
new Vue({
    data:{
        n:1
    }
    directives:{
        fbind:{
		//指令与元素成功绑定时（一上来）执行
		bind(element,binding){
			element.value = binding.value
			},
		//指令所在元素被插入页面时执行
		inserted(element,binding){
			element.focus()	//这是输入框获取焦点的办法
			},
		//指令所在的模板被重新解析时执行
		update(element,binding){
			element.value = binding.value
			}
    }
})
</script>
```

###### 自定义指令注意事项：

> + 当自定义指令的名称含有多个单词时，就需要用 “-” 来分割单词。并且在directives中定义时，需要用“ ”括起来
>
> ```html
> <h2>
>     <span v-big-number='n'></span>
> </h2>
> <script>
> new Vue({
>     directives:{
>         "big-number"(element,binding){
> 
>         }
>     }
> })
> </script>
> ```

> + 在自定义指令的函数中，this的指向是window

> + 全局自定义指令
>
> ```html
> //全局对象式
> <script>
> Vue.directive('fbind',{
>         fbind:{
> 		//指令与元素成功绑定时（一上来）执行
> 		bind(element,binding){
> 			element.value = binding.value
> 			},
> 		//指令所在元素被插入页面时执行
> 		inserted(element,binding){
> 			element.focus()	//这是输入框获取焦点的办法
> 			},
> 		//指令所在的模板被重新解析时执行
> 		update(element,binding){
> 			element.value = binding.value
> 			}
>     })
>     new Vue({
> 
> })
> </script>
> ```
>
> ```html
> //全局函数式
> <script>
> Vue.directive('big',function(element,binding){
>             element.innerText = binding.value*10
>         }
>              )
> </script>
> ```

###### 总结：

```
一、定义语法：
(1).局部指令：
new Vue({
		directives:{指令名:配置对象}
		}）
或
	new Vue({
		directives{指令名：回调函数}
			})
(2).全局指令：
Vue.directive(指令名,配置对象）或
Vue.directive(指令名,回调函数)
二、
配置对象中常用的3个回调：
(1).bind：指令与元素成功绑定时调用。
(2).inserted：指令所在元素被插入页面时调用。
(3).update：指令所在模板结构被重新解析时调用。
三、备注：
1.指令定义时不加v-，但使用时要加v-；
2.指令名如果是多个单词，要使用kebab-case命名方式，不
用camelCase命名。
```

##### 33.生命周期

给Vue对象新添个函数 mounted( ){ }

当vue完成模板的解析，并把初始的(第一次)真实的DOM放入界面后(即挂载完毕)开始调用mounted函数

```html
<script>
new Vue({
    data:{
        
    },
    methods:{
        
    },
    mounted(){
        
    }
})
</script>
```

（像mounted这样的函数就是生命周期函数）

<img src="https://yulearn.top/webSource/img/vue%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png" style="zoom:50%;" />

###### 总结：

```
生命周期：
1.又名：生命周期回调函数、生命周期函数、生命周期钩子。
2.是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数。
3.生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。
4.生命周期函数中的this指向是vm 或 组件实例对象。
```

##### 34.生命周期分析

小技巧：debugger；可以打断点让程序停在那里 

###### 生命周期总结：

<img src="https://yulearn.top/webSource/img/vue%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%AE%8C%E6%95%B4%E7%89%88.png" style="zoom:50%;" />

```
常用的生命周期钩子：
1.mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】。
2.beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。
关于销毁Vue实例
1.销毁后借助Vue开发者工具看不到任何信息。
2.销毁后自定义事件会失效，但原生DOM事件依然有效。(绑定的按钮依然可以操作)
3.一般不会再beforeDestroy操作数据，因为即便操作数据也不会触发更新流程了。
```

##### 35.Vue组件化

组件定义：实现应用中局部功能代码和资源的集合。

组件分类：

1.非单文件组件：一个文件中包含n个组件

2.单文件组件：一个文件中只包含一个组件(.vue文件)

###### 组件化的基本流程

> + 创建组件
>
> ```html
> <script>//创建组件用extend
> const school = Vue.extend({
>     // el:'.root' 一定不要写el配置项，组件不能指定服务对象
>     // data:{} 不可以写对象式，只可以写成函数式
>     data(){
>         return {
>             a:1,
>             b:2
>         }
>     }
> })
> new Vue({
>     ei:'.root',
> 
> })
> </script>
> ```

创建组件不光要有js，还应该有html。这时候就引入了组件的一个新属性 template

```html
<script>//创建组件用extend
const school = Vue.extend({
 //这里使用模板字符引号（键盘波浪线），这样可以随意换行
    template:`	<div>	//必须有一个根元素包起来
  			<h1>{{a}}</h1>
  			<h2>{{b}}</h2>
				</div>
  			`
    data(){
        return {
            a:1,
            b:2
        }
    }
})
new Vue({
    ei:'.root',
    
})
</script>
```



> + 注册组件（局部注册）
>
> （要把组件创建放在注册之前）
>
> ```html
> <script>
> const school = Vue.extend({
>     data(){
>         return {
>             a:1,
>             b:2
>         }
>     }
> })
> new Vue({
>     ei:'.root',
>     components:{//引入vue对象该属性，来注册组件
>         school:school //定义组件名（前面的才是真的组件名字）
>         school	//可以简写（属性名和属性值一样时）
>     }
> })
> </script>
> ```

> + 使用组件
>
> ```html
> <div class="root">
>     <school></school>//在这里使用组件（以标签的形式）
> </div>
> <script>
> const school = Vue.extend({
>     template:`	<div>	
>   			<h1>{{a}}</h1>
>   			<h2>{{b}}</h2>
> 				</div>
>   			`,
>     data(){
>         return {
>             a:1,
>             b:2
>         }
>     }
> })
> new Vue({
>     el:'.root',
>    components:{
>        school:school
>    }
> })
> </script>
> ```

###### 全局注册组件

```html
<script>
    const school = Vue.extend({
    template:`	<div>	
  			<h1>{{a}}</h1>
  			<h2>{{b}}</h2>
				</div>
  			`,
    data(){
        return {
            a:1,
            b:2
        }
    }
})
Vue.component('school','school')
</script>
```

###### 初识组件总结：

```
Vue中使用组件的三大步骤：
一、定义组件(创建组件)
二、注册组件
三、使用组件(写组件标签)
一、如何定义一个组件？
使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options几乎一样，但
区别如下：
1.el不要写，为什么？
最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器
2.data必须写成函数，为什么？
避免组件被复用时，数据存在引用关系。
备注：使用template可以配置组件结构。
二、如何注册组件？
1.局部注册：靠new Vue的时候传入components选项
2.全局注册：靠Vue.component('组件名',组件)
三、编写组件标签：
<school></school>
```

##### 36.组件的注意问题

> + 组件推荐命名
>
> 一个单词组件名，可以把首字母大写（与vue解析呼应）
>
> 多个单词组件名：1.把单词之间用“-”连接（vue解析后会自动去-，自动首字母大写）
>
> ​								2.把每个单词的首字母都大写（前提是在脚手架下使用）

> + 使用name配置项指定组件在开发者工具中呈现的名字
>
> ```html
> <script>
> const school = Vue.extend({
>     name:"s1",//在这里定义的名字就会显示在开发者工具中
>     data(){
>         return {
>             a:1,
>             b:2
>         }
>     }
> })
> new Vue({
>     ei:'.root',
> 
> })
> </script>
> ```

> + 组件定义简写
>
> ```html
> <script>
>     const s = {
>     name:"s1",//在这里定义的名字就会显示在开发者工具中
>     data(){
>         return {
>             a:1,
>             b:2
>         }
>     }
>     }
> </script>
> ```



###### 总结：

```
组件名：
个单词组成：
第一种写法(首字母小写)：school
第二种写法(首字母大写)：School
多个单词组成：
第一种写法(kebab-case命名)：my-school
第二种写法(CamelCase命名)：MySchool（需要Vue脚手架支持）
备注：
(1).组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行。
(2).可以使用name配置项指定组件在开发者工具中呈现的名字。
2.关于组件标签：
第一种写法：<school></school>
第二种写法：<school/>
备注：不用使用脚手架时，<school/>会导致后续组件不能渲染。
3.组件定义简写方式：
const school = Vue.extend(options) 可以简写为 const school= options
```

##### 37.组件的嵌套

##### 38.VueComponent

vue组件的本质就是构造函数VueComponent。

可以在vm的$children中查看vm所管理的vc（组件），也可以在vc中的$children中查看他所管理的嵌套组件

###### 总结：

```
关于VueComponent：
1.school组件本质是一个名为VueComponent的构造函数，并不是程序员定义的，是Vue.extend生成的。
2.我们只需要写<school/>或<school></school>，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：new VueComponent(options)。
3.特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent！！！！
4.关于this指向：
(1).组件配置中：
data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】
(2).new Vue()配置中：
data函数、methods中的函数、watch中的函数、compued的函数中，他们的this均是Vue的实例对象【vm】。
5.VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）。Vue的实例对象，以后简称vm。
```

##### 39.vm和vc的内置关系

二者大部分相同，但是也有一部分不相同，el，data等不相同。

###### 一个重要的内置关系

显式原型属性（prototype只有函数才有）和隐式原型属性__ _proto_ _ _

<img src="https://yulearn.top/webSource/img/vue%E5%8E%9F%E5%9E%8B%E5%AF%B9%E8%B1%A1.png" style="zoom: 33%;" />

###### 总结：

```
1．一个重要的内置关系
VueComponent.prototype.__proto__=== Vue.prototype
2.为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。
```

##### 40.Vue单文件组件

以.vue结尾的文件（命名规则和上述非单文件组件命名规则相同）推荐用：大驼峰命名法（首字母全大写）

> + 组件的结构
>
> 1.template标签
>
> ```vue
> <template>
> //组件的结构（html的内容）
> </template>
> ```
>
> 2.script标签
>
> ```vue
> <script>
> //组件的交互
> </script>
> ```
>
> 3.style标签
>
> ```vue
> <style>
> //组件的样式
> </style>
> ```

###### 安装vue插件来实现.vue文件代码的高亮：vetur

###### 快捷键快速构造vue模板  “ < + v ”

```vue
<template>
	<div class="demo">
    <h1>{{name}}</h1>
    </div>
</template>
<script>
    const school = Vue.extend({
        data(){
            return {
                name:"tom"
            }
        }
    })
    export default school //暴露组件(三种方式)
</script>
<style>//没有样式可以不写该标签
    .demo{
        width:100%;
        heignt:20px;
    }
</style>
```

默认暴露简写方式：

```vue
<script>
    export default {
        name:"School",//跟文件名最好一样
        data(){
            return {
                name:"tom"
            }
        }
    }
</script>
```

###### App.vue 汇总所有组件文件

```vue
<template>
  //在该标签下引入组件标签，应该在外部套一个标签，不可以直接用template标签
	<div>
    <demo></demo>
    </div>
</template>
<script>
import demo from './demo.vue' //引入组件
export default {
	name:'App',
    components:{
        demo
    }
}
</script>

<style>

</style>
```

###### main.js文件(入口文件)创建vue实例

```js
import App from './App.vue'
new Vue({
    el:'#root',
    components:{
        App
    }
})
```

###### index.html文件准备Vue服务的容器root

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>单文件容器准备</title>
</head>
<body>
    <div id="root">
        <App></App> //这里也可以不写，在main.js中Vue实例下的template属性下写（template:`<App></App>`）
    </div>
    <script src="./js/vue.js"></script>
    <script src="main.js"></script>
</body>
</html>
```

##### 41.Vue脚手架

##### 42.render配置项

脚手架引入了残缺的vue文件，阉割了template解析功能。此时就需要引用render函数

```html
render(createElement){//这个参数是一个函数
	return createElement('h1','hello')
}
```

简写为

```
render: h => h('h1','hello')
```

所以脚手架里面用的是

```
render: h => h(App)
```

###### 脚手架引入不完整vue的原因

> + template解析器占用空间过大，在用webpack打包时占用空间过大。

###### 总结：

```
关于不同版本的Vue：
1.vue.js与vue.runtime.xxx.js的区别：
(1).vue.js是完整版的Vue，包含：核心功能+模板解析器。
(2).vue.runtime.xxx.js是运行版的Vue，只包含：核心功能；没有模板解析器。
2.因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容。
```

##### 43.脚手架的默认配置

如果想查看vue工程的配置项，可以在控制台输入下面命令行，但是不可以修改

```
vue inspect > output.js
```

如果想定制vue配置项，可以新建一个名为vue.config.js文件（与package.json同级）

在里面进行修改，但是并不是所有的东西都能够改，详情见官网。

###### 关闭语法检查配置项

```
在vue.config.js文件中写上：
module.exports = {
pages:{

},
	lintOnSave:false	//注意是与pages配置项同级的
}
```

##### 44.ref属性

他的作用就是给元素打标识（类似于原生的id），可用来获取DOM元素

```html
<template>
<h1 ref="title">
    {{name}}
    </h1>
    <button @click="show">
        点击
    </button>
</template>
<script>
    import School from './components/School'
export default{
    name:'App',
    components:{
        school
    },
    data(){
        return {
            name:"hello"
        }
    },
    methods:{
        show(){
            console.log(this.$refs.title)//在这里用这个就可以获取到h1元素
        }
    }
}
</script>
```

###### ref属性也可以获取到子组件的实例对象。

```html
<School ref="school"></School>
<script>
//在这里可以获取组件的实例对象
console.log(this.$refs.school)
    //如果用id来获取的话，只会输出dom元素
</script>
```

###### 总结：

```
1.被用来给元素或子组件注册引用信息（id的替代者）
2.应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
3.使用方式:
打标识：<h1 ref="xxx"></h1>
获取：this.$refs.XXX
```

##### 45.props配置项

###### 组件调用时传输数据

```html
<school name="tom" sex="男" :age="18"></school> //对于number属性的数据传输应该用v-bind来绑定这样才能使传输过去的是数字类型
```

###### 定义组件时接收数据

```html
<template>
	<h1>{{name}}</h1>
    <h1>{{sex}}</h1>
    <h1>{{age}}</h1>
</template>
<script>
export default {
    name:"Student",
    data(){
        
    },
    //法1.简单的声明接收
    props:['name','sex','age']
    //法2.接收的同时限制接收的类型
    props:{
        name:String,
        sex:String,
        age:Number	//在这里限制了接收的数据类型，如果传输过来的数据不是该类型，也会编译，但是控制台会有报错提醒
    }
    //法3.接收的同时对数据的限制和对默认值的限制和对必须性的限制
    props:{
        name:{
            type:String,//name的类型是字符串
                required:true//name的值是必须传的
        }，
         sex:{
            type:String,//sex的类型是字符串
                required:true//sex的值是必须传的
        }，
         age:{
            type:Number,//age的类型是字符串
                default:99 //如果不声明必须传值，可以通过该属性来设置默认值
        }，
    }
}
</script>
```

###### 收到的数据是无法被修改的

虽然更改后会在页面上可以显示更新后的内容，但是控制台会警告

> + props中传过来的数据会比data的数据优先级高（当两个配置项中有重复的key时，会使用props中传过来的数据）
> + 如果有重名的key在控制台会报错
> + 但是可以利用这一点来实现对props中传过来的数据的修改业务（赋值）
>
> ```html
> <template>
> 	<h1>{{name}}</h1>
>     <h1>{{sex}}</h1>
>     <h1>{{age}}</h1>
> </template>
> <script>
> export default {
>     name:"Student",
>     data(){
>         myname:this.name//此时就可以对data中的值进行操作
>     },
>     //简单的声明接收
>     props:['name','sex','age']
> 
> }
> </script>
> ```

###### 并不是所有的数据都可以传

被vue征用的数据名称不可以传，例如：key，ref等

###### 总结：

```
配置项props
功能：让组件接收外部传过来的数据
(1).传递数据：
<Demo name="xxx"/>
(2).接收数据：
第一种方式（只接收）：
props:["name"]
第二种方式（限制类型）：
props:{
	name:Number
}
第三种方式（限制类型、限制必要性、指定默认值）：
props:{
	name:{
		type:String，//类型
		required:true，//必要性
		default：老王’//默认值
		}
	}
备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改则会发出警告。如果业务需求确实要更改，那么请赋值props中的内容到data中，
然后再对data中的数据进行修改
```

##### 46.mixin（两个组件共用一个配置项）

使用的场景是，多个组件公用一套vc配置项，此时就可以把这些配置项从组件文件中剥离出来，放在一个新建的js文件中，再模块化导入导出。

###### 局部配置项

```javascript
//新建一个mixin文件，在里面写入组件的公共配项
const mixin = {
    methods:{
        show(){
            console.log("hello")
        }
    },
    mounted(){
        alert("hello")
    }
}
export default mixin
```

```html
//在组件用引入并使用mixin文件
<script>
    import mixin from '../mixin.js'
export default {
    name:'School',
        data(){
        return{
            
        }
    },
        mixins:[mixin]//这里需要用新的配置项来承接
}
</script>
```

###### vc的mixins:[ ]配置项

用来承接公共的配置项文件。

一个mixin文件可以存放多个公用配置项

```js
//新建一个mixin文件，在里面写入组件的公共配项
export const mixin = {
    methods:{
        show(){
            console.log("hello")
        }
    },
    mounted(){
        alert("hello")
    }
}
export const mixin2 = {
    data(){
        return{
            name:"tom"
        }
    }
}
//在这里用非默认方法导出，则在vue组件中应该换种方法导入
import {mixin,mixin2} from '../mixin.js'
mixins:[mixin,mixin2]
```

###### 公用配置项和组件内部配置项优先性问题

当内外配置项发生冲突时，组件内部的配置项优先于引用的配置项，但是仅仅是对普通配置项优先。

当生命周期钩子发生冲突时，不会有优先级之分，内部外部配置的钩子都会执行，并且外部的钩子先在内部的钩子之前执行。

###### 全局配置项

在main文件中引用，并且用Vue.mixin( )来声明，这样在全局中都可以使用该外部配置项

```js
//这是在main.js中的代码
import {mixin,mixin2} from '../mixin'
Vue.mixin(mixin)
Vue.mixin(mixin2)
```

###### 总结：

```
mixin(混入)[
功能：可以把说个组件共用的配置提取成一个混入对象
使用方式：
第一步定义混合，例如：
{
data(){....},
methods:{....}
}
第二步使用混入，例如：
(1).全局混入：Vue.mixin(xxx)
(2).局部混入：mixins:['xxx"]
```

##### 47.插件

（可以增强一下vue）

插件的本质是对象，但是对象里面必须有install方法。在src文件夹中新建plugins.js文件

```js
//定义插件
const obj = {
    install(Vue){ //这里的形参是vue的原型对象
       console.log(Vue) 
        //可以写全局过滤器
        //可以定义全局指令
        //可以定义混入
        //可以给Vue原型上添加一个方法(vm和vc都可以使用了)
        Vue.prototype.hello = ()=>{alert("hello")}
    }
}
export default obj
```

```js
//引用插件（在main.js中引用）
import plugins from './plugins'
	Vue.use(plugins)
```

###### 总结：

```
插件功能：用于增强Vue
本质：包含install方法的一个对象，install的第一个参数是Vue，
第二个以后的参数是插件使用者传递的数据。
定义插件：
对象.install = function (Vue, options) {
// 1. 添加全局过滤器
Vue.filter(....)
// 2．添加全局指令
Vue.directive(....)
//3.配置全局混入
Vue.mixin(....)
//4.添加实例方法
Vue.prototype.$myMethod = function ( ) {....}
Vue.prototype.$myProperty = xxxx
}
使用插件：Vue.use( 插件名 )
```

##### 48.scoped样式

当组件过多时，在写每个组件样式的时候可能会发生类名重复的现象（不同vue文件中样式重名问题），这样就导致了样式错乱问题。

改进方法：

```html
<style scoped>//在此处写上该属性，就可以保证该组件文件的样式只作用于该文件

</style>
```

###### scoped的使用范围

可以在子组件中使用，但是不适合在APP组件（父组件）中引用。

###### 总结：

```
scoped样式的作用就是让样式只在局部中生效，防止冲突
写法：<style scoped>
```

##### 49.组件化的编码流程

> + 实现静态组件：抽取组件，把整个界面拆分为不同功能的组件。
> + 展示动态的数据
>
> 1.数据的类型名称是什么？
>
> 2.数据保存在哪个组件？

> + 实现交互功能

###### 推荐唯一id生成库：uuid（包大），nanoid（精简版的uuid）

```
//安装命令
npm i nanoid
使用方法：
1.安装完成引入 import {nanoid} from 'nanoid'
2.调用函数 nanoid()
```

`绑定@chang事件`

###### 总结：

```
1．组件化编码流程：
(1).拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突。
(2).实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：
	1).一个组件在用：放在组件自身即可。
	2)．一些组件在用：放在他们共同的父组件上（<span style="color:red">状态提升</span>)
(3).实现交互：从绑定事件开始。
2. props适用于：
(1).父组件 ==> 子组件 通信
(2).子组件 ==> 父组件 通信（要求父先给子一个函数）
3. 使用v-model时要切记：
V-model绑定的值不能是props传过来的值，因为props是不可以修改的！
4. props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做。
```

##### 50.浏览器的本地存储

###### localStorage

浏览器关闭后也不会清空数据

```html
//保存到本地的代码
<button onclick="saveData()">点我保存到本地</button>
<script>
window.localStorage.setTtem('msg','hello')//两个参数都应该是字符串类型,如果不是str类则会自动转化
    //保存对象
    let p = {}
    localStorage.setItem('mag1',JSON.stringify(p))
    //读取本地存储
    localStorage.getItem('msg')
    //删除本地存储
    localStorage.removeItem('msg')
    //清空本地存储
    localStorage.clear()
    //当读取的内容没有时，则会返回null
</script>
```

###### sessionStorage(api同localStorage)

（会话）浏览器关闭后数据就清空

```html
<button onclick="saveData()">点我保存到本地</button>
<script>
window.sessionStorage.setTtem('msg','hello')//两个参数都应该是字符串类型,如果不是str类则会自动转化
    //保存对象
    let p = {}
    sessionStorage.setItem('mag1',JSON.stringify(p))
    //读取本地存储
    sessionStorage.getItem('msg')
    //删除本地存储
    sessionStorage.removeItem('msg')
    //清空本地存储
    sessionStorage.clear()
    //当读取的内容没有时，则会返回null
</script>
```

###### 总结：

```
webStorage
1. 存储内容大小一般支持5MB左右（不同浏览器可能还不一样）
2.浏览器端通过Window.sessionStorage和Window.localStorage属性来实现本地存储机制。
3.相关API:
1.xxxxxStorage.setItem('key', 'value');
该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值。
2.xxxxxStorage getItem('person');
该方法接受一个键名作为参数，返回键名对应的值。
3.xxxxxStorage.removeItem('key');
该方法接受一个键名作为参数，并把该键名从存储中删除。
4.xxxxxStorage.clear()
该方法会清空存储中的所有数据。
4.备注：
1. SessionStorage存储的内容会随着浏览器窗口关闭而消失。
2. LocalStorage存储的内容，需要手动清除才会消失。
3.xxxxxStorage.getItem(xxx)如果xxx对应的value获取不到，那么getltem的返回值是null。
4. JSON.parse(null）的结果依然是null。
```

##### 51.组件的自定义事件

###### 通过给子组件绑定一个自定义事件（方法一）

实现子传给父数据

```html
//在APP组件中定义了一个student组件，并为其绑定一个自定义事件
<Student v-on:demo="show"/>//这里的demo事件是自定义的,绑定在student组件的实例对象身上
    <script>
        import Student from './Student'
  export default {
      methods:{
          show(name){
              console.log("我收到了",name)
          }
      },
      components:{School}
  }
    </script>
```

```html
//这里是在student组件中的内容
<template>
<button @click="sendName">
    点击
    </button>
</template>
<script>
export default {
    data(){
        return {
            name:"tom"
        }
    },
    methonds:{
        sendName(){
            //在该处触发自定义事件,用$emit触发,触发以后APP组件中就会调用demo事件绑定的show方法，可以向其传递参数
            this.$emit('demo',this.name)
        }
    }
}
</script>
```

###### 通过ref属性来为组件绑定自定义事件（方法二）

比第一种方法更灵活

```html
//App文件中的内容
<Student ref='student'/>
<script>
import Student from './Student'
    export default {
        components:{ student},
        metnods:{
            show(name){
                console,log("我收到了值",name)
            }
        },
        mounted(){
            //在此处用ref属性来绑定自定义事件
            //获取student组件实例对象,并绑定自定义demo事件
            this.$refs.student.$on('demo',this.show)
            //在这里可以使用定时器，来延后绑定事件
            //下面绑定方法只能执行一次（修饰符）
             this.$refs.student.$once('demo',this.show)
        }
    }
</script>
```

```html
//在student组件中
//这里是在student组件中的内容
<template>
<button @click="sendName">
    点击
    </button>
</template>
<script>
export default {
    data(){
        return {
            name:"tom"
        }
    },
    methonds:{
        sendName(){
            //在该处触发自定义事件,用$emit触发,触发以后APP组件中就会调用demo事件绑定的show方法，可以向其传递参数
            this.$emit('demo',this.name)
        }
    }
}
</script>
```

###### 解绑自定义事件

```html
//在student组件中
//这里是在student组件中的内容
<template>
<button @click="sendName">点击绑定事件</button>
    <button @click="unbind">点击解绑事件</button>
</template>
<script>
export default {
    data(){
        return {
            name:"tom"
        }
    },
    methonds:{
        sendName(){
            this.$emit('demo',this.name)
        },
        unbind(){
            //调用$off来解绑事件，但是只能一次解绑一个
            this.$off('demo')
            //想解绑多个事件应该把数组作为参数
            //假设现在有demo,demo1,demo2事件
            this.$off(['demo','demo1','demo2'])
            //不指定名字，就把所有的自定义事件都解绑
            this.$off()
        }
    }
}
</script>
```

###### 组件销毁后所有的自定义事件都不奏效了

```html
//在student组件中
//这里是在student组件中的内容
<template>
<button @click="sendName">点击绑定事件</button>
    <button @click="unbind">点击解绑事件</button>
    <button @click="death">销毁该vc</button>
</template>
<script>
export default {
    data(){
        return {
            name:"tom"
        }
    },
    methonds:{
        sendName(){
            this.$emit('demo',this.name)
        },
        unbind(){
            //调用$off来解绑事件，但是只能一次解绑一个
            this.$off('demo')
            //想解绑多个事件应该把数组作为参数
            //假设现在有demo,demo1,demo2事件
            this.$off(['demo','demo1','demo2'])
            //不指定名字，就把所有的自定义事件都解绑
            this.$off()
        },
        death(){
            //在此销毁后，自定义事件都不可以使用了
            this.$destroy()
        }
    }
}
</script>
```

##### 52.自定义事件注意事项

> + 谁触发的自定义事件，this就指向谁。
> + 组件标签如果直接用@绑定原生dom事件，vue会认为他是自定义事件。
>
> 如果想直接给组件标签绑定原生事件需要使用修饰符“.native”
>
> ```html
> <School @click.native="show"></School>
> ```

###### 总结：

```
组件的自定义事件
1. 一种组件间通信的方式，适用于：子组件===>父组件
2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（事件的回调在A中）。
3. 绑定自定义事件：
1.第一种方式，在父组件中:
<Demo @atguigu="test"/>
或
<Demo v-on:atguigu="test"/>
2. 第二种方式，在父组件中：
<Demo ref="demo"/>
mounted(){
this.$refs.xxx.$on('atguigu',this.test)//test提前准备好
}
3.若想让自定义事件只能触发一次，可以使用
once修饰符,或$once方法。
4. 触发自定义事件：
this.$emit('atguigu',数据)
5.解绑自定义事件 this.$off('atguigu')
6. 组件上也可以绑定原生DOM事件，需要使用 native 修饰符。
7.注意：通过 this.$refs.xxx.$on('atguigu',回调）绑定自定义事件时，回调要么配置在methods中，要么用箭头函数，否则this指向会出问题!
```

##### 53.全局事件总线

实现组件间的任意通信

```html
//在main.js中
//全局事件总线就是一个数据的中转站，在各个组件间起数据传递的中转作用
<script>
    new Vue({
        el:'.root',
        render: h=> h({App}),
        beforeCreate(){
            //安装全局事件总线$bus(名字不是强制的)
		Vue.prototype.$bus = this
		}
    })
</script>

```

可以在beforeDestroy钩子中进行事件的销毁

```html
<script>
new Vue({
    beforeDestroy(){
        //销毁hello事件
        this.$bus.$off('hello')
    }
})
</script>
```

###### 总结：

```
全局事件总线(GlcbalEventBus)
1. 一种组件间通信的方式，适用于任意组件间通信。
2. 安装全局事件总线：
		new Vue({
			beforeCreate(）{
				Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
			},
			}）
3. 使用事件总线：
	1.接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身。
		methods(){
			demo(data){......}//提前配置好要传的数据
		}
		mounted(){
			this.$bus.$on('xxxx',this.demo)
		}
	2.提供数据:
this.$bus.$emit('xxxx',数据)
4.最好在beforeDestroy钩子中，用$off去解绑当前组件所用到的事件。
```

```html
<script>
    beforeCreate(){Vue.prototype.$bus = this} 
    this.$bus.$on()
    this.$bus.$emit()
    beforeDestroy(){this.$bus.$off('hello')}
</script>
```

##### 54.消息订阅与发布

实现组件间的通信

###### 推荐使用第三方库：pubsub-js

使用该库，可以在任意框架中实现消息的订阅和发布。

安装命令：

```
npm i pubsub-js
```

引入第三方库：(引进来的是一个对象)

```
//在哪使用消息订阅和发布就在哪引入
import pubsub from 'pubsub-js'
```

###### pubsub的API使用

> + 订阅消息
>
> ```js
> import pubsub from 'pubsub-js'
> mounted(){
>     //该API第一个参数是消息名字，第二个参数是回调函数，当消息发布时，该回调函数会被执行
>    pubsub.subscribe('hello',function(msgName,data){	//回调函数有两个参数，第一个是消息的名字，第二个是发布者传过来的数据
>        console.log("pubsub订阅一个hello消息")
>    }) 
> }
> ```
>
> + 发布消息
>
> ```html
> <button @click="pubmsg">
>     点击发布消息
> </button>
> <script>
> import pubsub from 'pubsub-js'
> methods:{
>     pubmsg(){
>         pubsub.publish('hello',666)//第一个参数是发布消息的名字，第二个参数是传过去的数据
>     }
> }
> 
> </script>
> 
> ```
>
> + 消息的取消订阅
>
> ```html
> <script>
> import pubsub from 'pubsub-js'
> mounted(){//这里需要新建一个变量来承接这个消息订阅的id（同计时器原理）
>    this.pubId = pubsub.subscribe('hello',function(msgName,data){	
>        //注意在该回调函数中的this并不指向vue，如果想让其指向vue，可以使用箭头函数。或者先在methods中声明一个函数在这里调用。
>        console.log("pubsub订阅一个hello消息")
>    }) 
> },
>     beforeDestroy(){//在这个钩子里面触发消息解除订阅
>         pubsub.unsubscribe(this.pubId)
>     }
> </script>
> ```

###### 总结：

```
消息订阅与发布（pubsub）
1.一种组件间通信的方式，适用于任意组件间通信。
2.使用步骤：
1.安装pubsub:
npm i pubsub-js
2.引入: import pubsub from 'pubsub-js'
3. 接收数据：A组件想接收数据，则在A组件中订阅消息，
订阅的回调留在A组件自身。
methods(){
	demo(data){......}
},
mounted() {
	this .pid = pubsub.subscribe('xxx',this.demo) //订阅消息
}
4. 提供数据：pubsub.publish('xxx',数据）
5.最好在beforeDestroy钩子中，用PubSub.unsubscribe(pid)去取消订阅。
```

###### 小技巧：判断一个对象身上是否有某个属性的API

对象.hasOwnProperty('想判断的属性名')

##### 55.$nextTick(常用)

这个API可以接一个回调函数，并且这个回调函数会在DOM节点渲染完成后再调用

```html
//下面是是实现新增输入框后获取输入框焦点的功能
<script>
nethods:{
    show(){
        this.$nextTick(function(){
            this.$refs.input.focus()
        })
    }
}
</script>
```

总结：

```
nextTick
1.语法:
this.$nextTick(回调函数)
2. 作用：在下一次 DOM 更新结束后执行其指定的回调。
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。
```

##### 56.Vue动画效果（transition）

###### 单个元素动画实现思路：

> + 先定义好动画，并给想要施加动画的元素用<transition>标签套起来
> + 然后起两个Vue指定类名：
>
> ```html
> <transition>
> <h1 v-show="show">你好</h1>
> </transition>
> <style>
> .v-enter-active{
>     //入场动画
> }
> .v-leave-active{
>     //离场动画
> }
> @keyframe demo{
> 
> }
> </style>
> ```

###### 多个动画元素动画实现思路

与单个元素实现动画的区别在于，给每个<transition>标签添加一个name属性，通过该属性来实现指定元素的动画效果

不要忘记改css中的引用名

```html
<transition name="hello">
<h1 v-show="show">你好</h1>
</transition>
<style>
.hello-enter-active{
    //入场动画
}
.hello-leave-active{
    //离场动画
}
@keyframe demo{
    
}
</style>
```

###### 页面加载时自动执行动画

只需要给transition标签新增一个appear配置项，并把值设置为true

```html
<transition name="hello" :appear="true">//注意这里绑定布尔值的写法
    //简写方法
    <transition name="hello" appear>
<h1 v-show="show">你好</h1>
</transition>
<style>
.hello-enter-active{
    //入场动画
}
.hello-leave-active{
    //离场动画
}
@keyframe demo{
    
}
</style>
```

##### 57.Vue过度效果

###### 单个元素过度

```html
<transition name="hello">
<h1 v-show="show">你好</h1>
</transition>
<style>
    h1{
        transition:0.5s linear;
    }
.hello-enter{
    //进入的起点
    transform:translateX(-100%);
}
    .hello-enter-to{
        //进入的终点
        transform:translateX(0);
    }
.hello-leave{
    //离开的起点
    transform:translateX(0);
}
    .hello-leave-to{
        //离开的终点
        transform:translateX(-100%);
    }
</style>
```

简写效果：

```html
<transition name="hello">
<h1 v-show="show">你好</h1>
</transition>
<style>
 .hello-enter,
 .hello-leave-to{
    transform:translateX(-100%);
    }
    
    .hello-enter-active,
    .hello-leave-active{
        transition: 0.5s linear;
    }
    
    .hello-enter-to,
    .hello-leave{
        transform:translateX(0);
    }

</style>
```

###### 多个元素过度

与单个元素过度的唯一区别就是transition标签变成transition-group,并且每个子元素要有唯一的key值

```html
<transition-group name="hello">
<h1 v-show="show" key="1">你好1</h1>
<h1 v-show="show" key="2">你好2</h1>
</transition-group>
<style>
 .hello-enter,
 .hello-leave-to{
    transform:translateX(-100%);
    }
    
    .hello-enter-active,
    .hello-leave-active{
        transition: 0.5s linear;
    }
    
    .hello-enter-to,
    .hello-leave{
        transform:translateX(0);
    }

</style>
```

##### 58.Vue集成第三方动画库

###### 推荐动画库：animate.css

安装命令：

```
npm install animate.css
```

引入包：

```
//与引入js包不一样
//在那个界面用就在那个界面使用
import 'animate.css'
```

###### 在Vue中使用

配置项：

```
name="animate__animated animate__bounce"//使用了这个库
 enter-active-class="进入时使用的动画名字"
 leave-active-class="离开时动画的名字"
```

```html
<transition-group
 appear
 name="animate__animated animate__bounce"//使用了这个库
 enter-active-class="animate__rubberBand"//进入时使用的动画名字
 leave-active-class="animate__backOutRight"//离开时动画的名字              
                  >
<h1 v-show="show" key="1">你好1</h1>
<h1 v-show="show" key="2">你好2</h1>
</transition-group>
<style>
    h1{
        background-color:red;
    }
</style>
```

###### 总结：

```
Vue封装的过度与动画
1.作用：在插入、更新或移除DOM元素时，在合适的时候给元素添加类名样式
2.图示
3.写法:
1.准备好样式:
元素进入的样式：
1. v-enter：进入的起点
2. v-enter-active：进入过程中
3. v-enter-to：进入的终点
元素离开的样式：
1. v-leave：离开的起点
2. v-leave-active：离开过程中
3. v-leave-to：离开的终点
2.使用<transition>包裹要过度的元素，并配置name属性:
<transition name="hello">
<h1 v-show="isShow">你好啊!</h1>
</transition>
3. 备注：若有多个元素需要过度，则需要使用：
<transition-group>，且每个元素都要指定 key 值。
```

​																																	图

<img src="https://yulearn.top/webSource/img/Vue%E5%8A%A8%E7%94%BB.png" style="zoom:50%;" />

##### 59.Vue中的Ajax请求

使用axios发送请求

> + 安装axios包
>
> ```
> npm i axios
> ```
>
> + 导入包
>
> ```
> import axios from 'axios'
> ```
>
> + 请求语法
>
> ```html
> <button @click="getMsg">
>     点击
> </button>
> <script>
> import axios from 'axios'
>     export default{
>         name:'App',
>         mathods:{
>             getMsg(){
>                 axios.get('https://.......').then(//返回的是promise形式的值
>                 response =>{//响应成功(response是一个对象)这里是axios的固定写法
>                     console.log("响应成功了",response.data)
>                 },
>                     error =>{//响应失败
>                          console.log("响应失败了",error.message)
>                     }
>                 )
>             }
>         }
>     }
> </script>
> ```

###### 跨域问题

> + 出现原因
>
>   协议名，主机名，端口号有一样不同时都会发生跨域

> + 前端解决跨域问题常用方法
>
>   使用代理服务器

###### 使用Vue脚手架开启代理服务器

[配置参考 | Vue CLI (vuejs.org)](https://cli.vuejs.org/zh/config/#devserver-proxy)

###### 方式一

> + 在Vue的配置文件中（vue.config.js）

```js
module.exports = {
    lintOnsave:false, //关闭语法校验
      devServer: { //开启代理服务器
    proxy: 'http://localhost:5000'//这里写的是被请求的地址
  }
}
```

> + 在需要请求的页面更改请求地址

```js
import axios from 'axios'
    export default{
        name:'App',
        mathods:{
            getMsg(){		//这里写的是代理服务器地址
                axios.get('https://.......:8080').then(
                response =>{
                    console.log("响应成功了",response.data)
                },
                    error =>{
                         console.log("响应失败了",error.message)
                    }
                )
            }
        }
    }
```

缺点：当请求的数据代理服务器本身就有，则代理服务器不会向指定地址发送请求，并且只能配置一台代理服务器。

###### 方式二

> + 在Vue配置文件中

```js
module.exports = {
  devServer: {
    proxy: {
        //第一个代理
      '/api': {//自定义请求前缀，只要请求前缀是api就转发到下面地址（这里的请求头可以任意写）
        target: 'http://localhost:5000'//这里写的是转发的地址（基础路径）
        pathRewrite:{'^/api':''}//这里是配置代理服务器的请求路径（通过正则表达式去除自定义的请求头）
        ws:true,//用于支持websocket
        changeOrigin: true //用于控制请求中的host值
      },
      //第二个代理
       '/demo': {
        target: 'http://localhost:5001'
        pathRewrite:{'^/demo':''}
        ws:true,
        changeOrigin: true 
      }
    
    }
  }
}
```

> + 在需要请求的界面更改请求地址（配置请求头，请求头紧挨端口号）

```js
import axios from 'axios'
    export default{
        name:'App',
        mathods:{
            getMsg(){		//这里写的是代理服务器地址
                axios.get('https://.......:8080/api/student').then(
                response =>{
                    console.log("响应成功了",response.data)
                },
                    error =>{
                         console.log("响应失败了",error.message)
                    }
                )
            }
        }
    }
```

##### 60.Ajax请求案例

###### 用到第三方UI库时（例如bootstrap）

方法一

> + 可以在src文件夹下建立一个assets文件夹，在assets文件夹下新建css文件夹，把库中的资源放进去
> + 在APP.vue文件中引用该css库
>
> ```html
> <script>
> import '文件路径'
> </script>
> ```
>
> 但是这里存在一个问题，使用import引入时，Vue会对引入的内容严格检查，当出现未引用的东西就会报错，例如
>
> 在bootstrap的css中会引用字体，如果没下字体就会报错。

方法二

> + 可以在public文件夹下新建一个css文件夹，再把第三方库文件放进去
> + 在index.html中用link标签引入，这样即使有不存在的内容也不会报错
>
> ```html
> <link rel="stylesheet" href="<%= BASE_URL %>css/bootstrap.css">
> ```

###### 发送get请求并携带参数

（使用ES6语法）

```html
<script>
    import axios from 'axios'
    data(){
      return {
          keyword:''
      }  
    },
methods:{
    search(){					//这里通过``符号和${}来实现传参请求
        axios.get(`https://api.github.com/search/users?q=${this.keyword}`).then(
        response =>{
            conslole.log(response.data)
        },
            error =>{
                console.log(error.message)
            }
        )
    }
}
</script>
```

小技巧：可以用ES6的解构语法来替换对象中的数据

##### 60.vue-resource插件库

（发送Ajax请求，现在用的很少了）

> + 插件安装
>
> ```
> npm i vue-resource
> ```
>
> + 引入插件(引入后vue原型身上多了个$http属性)
>
> ```js
> //在main.js中
> import vueResource from 'vue-resource'
> Vue.use(vueResource)
> ```
>
> + 使用插件(用法同axios)
>
> ```js
> <script>
>     import axios from 'axios'
>     data(){
>       return {
>           keyword:''
>       }  
>     },
> methods:{
>     search(){					//这里通过``符号和${}来实现传参请求
>         this.$http.get(`https://api.github.com/search/users?q=${this.keyword}`).then(
>         response =>{
>             conslole.log(response.data)
>         },
>             error =>{
>                 console.log(error.message)
>             }
>         )
>     }
> }
> </script>
> ```

##### 61.slot插槽

###### 默认插槽

使用<slot>标签过程：

> + 在组件标签中套入其他标签(定义插槽)

```html
//App界面
<School>
<img src="xxx.png">
</School>
```

> + 在组件中使用<slot>标签(告诉Vue该把上述插入的标签放在哪里)
>
> ```html
> //School界面
> <div>
>     <h1>hello</h1>
>     <slot></slot>
> </div>
> ```

###### 具名插槽

使用方法：

> + 给插槽添加name属性

```html
//School界面
<div>
    <h1>hello</h1>
    <slot name="hello1"></slot>
     <slot name="hello2"></slot>
</div>
```

> + 给组件标签中的内容添加slot属性

```html
//App界面
<School>
<img slot="hello1" src="xxx.png">
   <a slot="hello2">hello</a>//可以在这里追加同名的name并不会产生覆盖
<a slot="hello2">hello</a>
</School>
```

小技巧：可以使用template标签来包裹多个插槽元素。

```html
<School>
    <template slot="hello">
    	<img src="xxx.png">
   		<a>hello</a>
		<a>hello</a>
    </template>
</School>
```

进阶写法：(仅限在template标签中)

```html
<School>
    <template v-slot:hello>//这个写法仅适用于template标签
    	<img src="xxx.png">
   		<a>hello</a>
		<a>hello</a>
    </template>
</School>
```

###### 作用域插槽

解决插槽中渲染的数据的作用域问题

```html
//插槽的定义界(Game组件)面发送数据
<template>
<slot :games="games"></slot>//在此处把数据传给插槽的使用者
</template>
<script>
export default {
    data(){
        return{
            games:['aaa','bbb','ccc']
        }
    }
}
</script>
```

```html
//插槽的使用者界面
<template>
	<Game>//此处必须用template标签套住
    <template scope="hello">//这里是接收传来的数据（数据名随便起），这里传来的是一个对象，插槽构造者传来的数据作为该对象的一个属性
        <ul>
            <li v-for="(g,index) in hello.games" :key="index">{{g}}</li>
        </ul>
    </template>
    </Game>
</template>
```

###### 总结：

```
插槽
1. 作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于 父组件===>子组件
2. 分类：默认插槽、具名插槽、作用域插槽
3. 使用方式：
1.默认插槽:
父组件中：
		<Category>
			<div>html结构1</div>
		</Category>
子组件中：
		<template>
			<div>
		<!-- 定义插槽 -->
			<slot>插槽默认内容...</slot>
			</div>
		</template>
2.具名插槽:
父组件中：
		<Category>
		<template slot="center">
			<div>html结构1</div>
		</template>
		<template v-slot:footer>
			<div>html结构2</div>
		</template>
		</Category>
子组件中：
        <template>
        <div>
        <!-- 定义插槽 --!>
        <slot name="center'>插槽默认内容...</slot>
        <slot name="footer">插槽默认内容...</slot>
        </div>
        </template>
3.作用域插槽:
1. 理解：数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。(games数据在Category组件中，但使用
数据所遍历出来的结构由App组件决定)
2.具体编码:
父组件中：
        <Category>
        <template scope="scopeData">
        <!-- 生成的是ul列表--!>
       		 <ul>
        		<li v-for="g in scopeData.games" :key="g">{{g}}</li>
        	</ul>
        </template>
        </Category>
        子组件中：
        <template>
        <div>
        <slot :games="games"></slot>
        </div>
        </template>

        <script>
        export default {
        		name:'Category',
            	props:['title'],
       			 //数据在子组件自身
       		 data() {
        return{
        games:['红色警戒','穿越火线','劲舞团'，超级玛丽
        		}
        }，
        </script>
```

##### 62.Vuex

​		在Vue中实现集中式状态（数据）管理的Vue插件，对Vue应用中多个组件中的共享状态进行集中式管理（读/写），也是一种组件间的通信方式，适用于任意组件之间。

###### 搭建Vuex环境

> + 安装
>
> ```
> npm i vuex@3      //在这里指定3版本
> //注意在Vue2中只可以用Vuex的3版本，Vue3中要用Vuex的4版本（现在默认是使用和Vue3配套的vuex，所以在使用2开发时，应该指定vuex的版本号，否则会报错）
> ```
>
> + 引用插件
>
> ```
> //在main文件中引用
> import Vuex from 'vuex'
> 	Vue.use(vuex)
> 	//在use使用后，Vue原型身上就多了个store配置项
> ```
>
> + 创建文件夹
>
> ```
> 在src文件夹中，创建store文件夹，在里面新建index.js文件
> ```
>
> + 在index文件中配置
>
> ```js
> import Vue from 'vue'
> import Vuex from 'vuex'//引入Vuex
> const actions = {}//用于响应组件中的动作
> const mutations = {} //用于操作数据
> const state = {}//用于存储数据
> Vue.use(Vuex)//在这里应用Vuex插件，main文件中的use可以不用引入了（会报错）
> const store = new Vuex.Store({
>     actions,
>     mutations,
>     state,
> })//创建store
> export default store //导出
> -------------------------------------------
> //简写方式
>     export default new Vuex.Store({
>    	actions,
>     mutations,
>     state,
> })
> ```
>
> + 在main中引用
>
> ```js
> import store from './store'	//在此处用小写
> new Vue({
>     el:'#app',
>     render: h=>h (App),
>     store,
> })
> ```

###### 总结：

```
一、概念
在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。
二、何时使用？
多个组件需要共享数据时
三、搭建vuex环境
1. 创建文件： src/store/index.js
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)
//准备actions对象一响应组件中用户的动作
const actions = {}
//准备mutations对象—修改state中的数据
const mutations = {}
//准备state对象—保存具体的数据
const state = {}
//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
})
2.在main.js中创建vm时传入 store配置项
//引入store
import store from ./store
//创建vm
	new
	Vue({
		el:'#app',
		render: h => h(App),
		store
	}）
```

##### 63.Vuex的使用

###### Vuex实现流程

<img src="https://yulearn.top/webSource/img/Vuex%E6%B5%81%E7%A8%8B.png" style="zoom: 50%;" />

###### Vuex第一次使用

```html
//组件界面
<h1>{{$store.state.sum}}</h1>//在这里注意从哪里拿到的数据
<button @click="addNumber">加一</button>
<button @click="addLazy">等一会加一</button>
<script>
export default {
    data(){
        return{
            n:1
        }
    },
    methods:{
        addnumber(){
            this.$store.dispatch('jia',this.n)//在此处把“jia”方法和n数据传递到actions中
        },
        addLazy(){
            this.$store.dispatch('lazy',this.n)
        }
    }
}
</script>
```

```js
//在Vuex配置的index文件中
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

const actions = {
    jia(context,value){//这里会收到两个参数，第一个参数是一个对象，并且具有store对象身上的部分属性（类似为store的阉割版），第二个参数是组件传过来的数据
      context.commit('JIA',value)  //在这里把数据传给mutations对象(这里的函数最好写成大写，为了区分是actions中的还是mutations中的函数)
    },
    lazy(context,value){
        setTimeout(()=>{	//所有的业务逻辑都应该在actions中写完
            context.commit('LAZY',value)
        },1000)
    }
}

const mutations = {
    JIA(state,value){//这里会收到两个参数，第一个是存放数据的state（已经匹配好get和set方法），第二个是传过来的数据
        state.sum += value //在mutations中进行对数据的处理
    },
    LAZY(state,value){
        state.sum += value
    }
} 

const state = {
    sum:0
}

    export default new Vuex.Store({
   		actions,
    	mutations,
    	state
})
```

注意：最好只在actions中写业务逻辑，不推荐在mutations中写

###### 当不需要actions对象中的操作

即没有业务逻辑时，可以直接在组件页面把数据传递给mutations对象

```html
<h1>{{sum}}</h1>
<button @click="addNum">点击加一</button>
<script>
export default{
    data(){
        return{
            n:1
        }
    },
    methods:{
        addNum(){
            this.$store.commit('JIA',this.n)
        }
    }
}
</script>
```

```js
//在Vuex配置的index文件中
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

const actions = {}

const mutations = {
    JIA(state,value){
        state.sum += value 
    }
} 

const state = {
    sum:0
}

    export default new Vuex.Store({
   		actions,
    	mutations,
    	state
})
```

###### Vuex开发者工具的使用

Vuex可以在vuedevtools插件中查看

###### 总结：

```
1.组件中读取vuex中的数据：$store.state.sum
2.组件中修改vuex中的数据:
$store.dispatch('action中的方法名',数据）或 $store.commit('mutations中的方法名',数据)
备注：若是没有网络请求或者其他的业务逻辑，组件中也可以越过actions，即不写dispatch，直接commit
```

##### 64.store中的getters配置项

不是必须使用的配置项，他用于对states数据进行二次加工（例如扩大十倍等操作）

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const actions = {}
const mutations = {} 
const state = {}
const getters = {//准备一个getters，用于将state中的数据进行加工
    bigSum(state){//定义一个方法，在里面加工数据
        return state.sum*10//这里需要return回去，就像组件的computed属性一样
    }
}
    export default new Vuex.Store({
   	actions,
    mutations,
    state,
    getters//不要忘记在这里导出getters配置项
})
```

```html
//组件页面中的使用
<h1>{{$store.getters.bigSum}}</h1>
```

##### 65.mapState

（映射）

> + 先在需要使用state数据的组件中引用mapState
>
> ```
> import {mapState} from 'vuex'
> ```

小技巧：

在ES6语法中，进行对象嵌套对象，并把子对象的内容展开为父对象中的key和value

```js
let obj1 = {x:1,y:2}
let obj2 = {
    ...obj1,//在这里就相当于把obj1中的内容展开到obj2中
    c:3
}
//此时输出obj2为：{x:1,y:2,c:3}
```

> + 再使用mapState，来定义在组件中使用state中的数据名称
>
> 对象写法
>
> ```html
> <h1>{{xuexiao}}</h1>
> <h1>{{xueke}}</h1>
> <script>
>     import {mapState} from 'vuex'
>     computed:{
>         //借助mapState生成计算属性，从state中读取数据
>         ...mapState({xuexiao:'school',xueke;'object'})//这里注意变量应该套引号（school和object数据已经在state当中定义好）
>         //如果不使用这个生成函数，则应该手动写
>         xuexiao(){
>             return this.$store.state.school
>         },
>             xueke(){
>                 return this.$store.state.object
>             }
>     }
> </script>
> ```
>
> 数组写法
>
> ```html
> <h1>{{school}}</h1>
> <h1>{{object}}</h1>
> <script>
>     import {mapState} from 'vuex'
>     computed:{
>         ...mapState(['school','object'])//这里可以这么写的条件是：组件中想使用的数据名和state中存储的数据名相同
> 
>             }
> </script>
> ```

优势：减少书写重复代码

##### 66.mapGetters

> + 先在需要使用state数据的组件中引用mapGetters
>
> ```
> import {mapState,mapGetters} from 'vuex'
> ```

> + 再使用mapGetters，来定义在组件中使用getters中的数据名称
>
> 对象写法
>
> ```js
> computed:{
>     ...mapGetters({bigSum:'bigSum'})
> }
> ```
>
> 数组写法
>
> ```js
> computed:{
>     ...mapGetters([bigSum])
> }
> ```

##### 67.mapMutations

（代替手动定义commit）

> + 先在需要使用state数据的组件中引用mapMutations
>
> ```
> import {mapMutations} from 'vuex'
> ```

> + 再使用mapMutations，来批量生成methods中直接调用的commit方法的函数
>
> 对象方法
>
> ```html
> <button @click="increment(n)">+1</button>//在这里要给函数传个参数，这个参数就是想要传给mutations的值
> <button @click="decrement(n)">-1</button>
> <script>
>     export default{
>         data(){
>             return{
>                 n:1
>             }
>         },
>         methods:{
>     ...mapMutations({increment:'JIA',decrement:'JIAN'})
>     //如果不调用该方法，则需要手写下面的代码
>     increment(){
>         this.$store.commit('JIA',this.n)
>     },
>         decrement(){
>             this.$store.commit('JIAN',this.n)
>         }
>     }
> 
> }
> </script>
> ```
>
> 数组写法
>
> ```html
> <button @click="increment(n)">+1</button>//在这里要给函数传个参数，这个参数就是想要传给mutations的值
> <button @click="decrement(n)">-1</button>
> <script>
>     export default{
>         data(){
>             return{
>                 n:1
>             }
>         },
>         methods:{
>     ...mapMutations(['increment','decrement'])//用数组方法的条件是在组件中定义的方法名和mutations对象中的方法名字一样
>     }
> 
> }
> </script>
> ```

优势：借助mapMutations生成对应的方法来联系mutations

##### 68.mapActions

（代替手动定义dispatch）

> + 先在需要使用state数据的组件中引用mapActions
>
> ```
> import {mapActions} from 'vuex'
> ```

> + 再使用mapActions，来批量生成methods中直接调用的dispatch方法的函数
>
> 对象写法
>
> ```html
> <button @click="incrementOdd(n)">+1</button>//在这里要给函数传个参数，这个参数就是想要传给actions的值
> <button @click="decrementOdd(n)">-1</button>
> <script>
>     export default{
>         data(){
>             return{
>                 n:1
>             }
>         },
>         methods:{
>     ...mapActions({incrementOdd:'jiaOdd',decrementOdd:'jianOdd'})
>     //如果不调用该方法，则需要手写下面的代码
>     incrementOdd(){
>         this.$store.dispatch('jiaOdd',this.n)
>     },
>         decrementOdd(){
>             this.$store.dispatch('jianOdd',this.n)
>         }
>     }
> 
> }
> </script>
> ```
>
> 数组写法
>
> ```html
> <button @click="incrementOdd(n)">+1</button>//在这里要给函数传个参数，这个参数就是想要传给actions的值
> <button @click="decrementOdd(n)">-1</button>
> <script>
>     export default{
>         data(){
>             return{
>                 n:1
>             }
>         },
>         methods:{
>     ...mapActions(['incrementOdd','decrementOdd'])//这里的使用条件是actions中的方法名和组件中调用的方法名是同一个
>     }
> 
> }
> </script>
> ```

优势：借助mapActions生成对应的方法来联系actions

##### 69.Vuex实现多组件共享数据

##### 70.Vuex模块化编码

把实现不同功能的函数放在不同mutations中

把不同的数据放在不同的state中

把实现不同功能的函数放在不同actions中

###### 计算属性模块化方法一

```js
//在vuex的index界面
//求和功能的vuex部分
const countOptions = {
    actions:{},//负责哪个功能的代码就放在哪个部分区域
    mutations:{},
    state:{
        f:1
    },
    getters:{}
}
//列表功能的vuex部分
const listOptinos = {
    actions:{},
    mutations:{},
    state:{
        g:2
    },
    getters:{}
}
export default new Vuex.Store({
    modules:{//用这个配置
        a:countOptions,//在这里定义好名字，以便在组件中使用
        b:listOptinos
    }
})
```

```html
//组件页面
<h1>{{a.f}}</h1>
<h1>{{b.g}}</h1>
<script>
    import {mapState} from 'vuex'
export default{
    computed:{
        ...mapState('a','b')
    }
}
</script>
```

###### 计算属性模块化方法二

```js
//在vuex的index界面
//求和功能的vuex部分
const countOptions = {
    namespaced:true,//在这里新增命名空间配置项
    actions:{},
    mutations:{},
    state:{
        num1:1
    },
    getters:{}
}
//列表功能的vuex部分
const listOptinos = {
    namespaced:true,
    actions:{},
    mutations:{},
    state:{
        num2:2
    },
    getters:{}
}
export default new Vuex.Store({
    modules:{
        count:countOptions,
        list:listOptinos
    }
})
```

```html
//组件页面
<h1>{{num1}}</h1>//在此更改引用的名字
<h1>{{num2}}</h1>
<script>
    import {mapState} from 'vuex'
export default{
    computed:{
        //在这里新增参数用来指定读取count模块中的state中的数据，这样在界面引用的时候就可以不写数据的上级名称（a.b这样）
        ...mapState('count',['num1'])//前面是单独空间的名字，后面是单独空间的数据
        ...mapState('list',['num2'])//可以复用
    }
}
</script>
```

###### 方法属性模块化方法一

在vuex的index中配置方法和上述计算属性一样

```html
//组件页面
<button @click="add(n)">+1</button>
<button @click="reduce(n)">-1</button>
<script>
export default{
    name:'demo',
    data(){
        return{
            n:1
        }
    },
    methods:{
        ...mapMutations('count',{add:'JIA'})//在这里指明作用空间
        ...mapMutations('list',{reduce:'JIAN'})
    }
}
</script>
```

对于actions中的配置同上述一样

###### 方法属性模块化方法二

```html
//组件界面
<button @click="add(n)">+1</button>
<script>
export default{
    name:'demo',
    data(){
        return{
            n:1
        }
    },
    methods:{
       add(){
           this.$store.commit('listOptinos/ADD',this.n)//斜杠前面的是空间名称，斜杠后面是函数名称。后面是传的参数
       }
    }
}
</script>
```

###### 模块化总结：

```
模块化+命名空间
1.目的：让代码更好维护，让多种数据分类更加明确。
2.修改store.js
	const countAbout ={
		namespaced:true,//开启命名空间
		state:{x:1},
		mutations:{ ... },
		actions: { ... },
        getters:{
        bigSum(state){
        return state.sum
        		}
        	}
        }
const personAbout = {
        namespaced:true,//开启命名空间
        state:{ ...}，
        mutations:{... },
        actions:{...},
        const store = new Vuex.Store({
        modules: {
        countAbout,
        personAbout
			}
		})
3.开启命名空间后，组件中读取state数据:
		//方式一：自己直接读取
	this.$store.state.personAbout.list
		//方式二：借助mapState读取：
	...mapState('countAbout',['sum','school','subject']),
4. 开启命名空间后，组件中读取getters数据:
		//方式一：自己直接读取
	this .$store .getters['personAbout/firstPersonName']
		//方式二：借助mapGetters读取：
	...mapGetters('countAbout',["bigSum!])
5. 开启命名空间后，组件中调用dispatch
		//方式一：自己直接dispatch
	this.$store.dispatch('personAbout/addPersonWang',person)
		//方式二：借助mapActions：
	...mapActions('countAbout'{incrementOdd:'jiaodd',incrementWait:'jiaWait'})
6. 开启命名空间后，组件中调用commit
		//方式一：自己直接commit
	this .$store .commit('personAbout/ADD_PERSON',person)
		//方式二：借助mapMutations：
	...mapMutations('countAbout'{increment:'JIA',decrement:'JIAN'}),
```

##### 71.路由

vue-router库，他是vue的一个插件库

适用于spa应用（一个应用只有一个完整的页面）

###### 搭建路由

> + 安装路由
>
> ```
> npm i vue-router@3	//在这里注意：现在的默认vue版本是3，所以不指定版本就下载的话会默认安装路由4。但是vue2并不能使用路由4，因此想要在vue2中使用路由，则应该指定第3版本。
> ```
>
> + 创建路由文件
>
> ```js
> //在src下创建router文件夹，在里面新建index.js文件（下面是该文件下的内容）
> import VueRouter from 'vue-router'
> //导入组件
> import About from '../pages/About'
> import Home from '../pages/Home'
> //创建一个路由器
> export default new VueRouter({
>     routes:[
>         {
>             path:'/about',
>             component:About//组件名
>         },
>         {
>             path:'/home',
>             component:Home
>         }
>     ]
> })
> ```
>
> + 引用插件和路由文件
>
> ```js
> //main.js中
> import VueRouter from 'vue-router'	//引入路由插件
> import router from './router'	//引入路由文件
> Vue.use(VueRouter)	//使用插件
> nem Vue({
>     el:'app',
>     render:h => h(App),
>     router:router //在这里写上配置项
> })
> ```
>
> + 使用router-link标签和to属性来实现跳转
>
> ```html
> //想要使用路由的界面
> //这里的active-class指的是该元素被激活时的样式(这里以bootstrap框架为例)
> <router-link to="/about" class="list-group-item" active-class="active">About</router-link>//这里to指向的是路由中配置的path属性
> <router-link to="/home" class="list-group-item" active-class="active">Home</router-link>
> ```
>
> + 使用router-view标签来展示路由控制的界面
>
> ```html
> //这里是展示路由控制的组件内容的界面
> <router-view></router-view>
> ```

###### 总结：

```
路由
1.理解：一个路由（route）就是一组映射关系（key-value），多个路由需要路由器（router）进行管理。
2.前端路由：key是路径，value是组件。
1.基本使用
1.安装vue-router，
命令:npm i vue-router
2. 应用插件：Vue.use(VueRouter)
3.编写router配置项:
//引入VueRouter
import VueRouter from 'vue-router'
//引入Luyou 组件
import About from '../components/About'
import Home from '../components/Home
//创建router实例对象，去管理一组一组的路由规则
const router = new VueRouter({
        routes:[
        {
       path:'/about',
       component:About
        }，
        {
       path:'/home',
       component : Home
        }
        ]
	}）
4.实现切换（active-class可配置高亮样式）
<router-link active-class="active" to="/about">About</router-link>
5.指定展示位置
<router-view></router-view>
```

##### 72.使用路由的注意事项

###### 组件的分类：

> + 路由组件
>
> 由路由器控制的组件，一般放在src文件下的pages文件夹中
>
> + 一般组件
>
> 需要自己手写组件标签调用的组件，还是放在components文件夹中

被切换走的组件其实是被销毁了，新切换来的组件被挂载

###### 总结：

```
几个注意点:
1. 路由组件通常存放在 pages 文件夹，一般组件通常存放在 components 文件夹。
2. 通过切换,“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
3. 每个组件都有自己的 $route 属性，里面存储着自己的路由信息。
4.整个应用只有一个router，可以通过组件的 $router 属性获取到。
```

##### 73.嵌套路由（多级路由）

> + 配置二级路由信息
>
> ```js
> import VueRouter from 'vue-router'
> import About from '../pages/About'
> import Home from '../pages/Home'
> import News from '../pages/News'
> import Messages from '../pages/Messages'
> export default new VueRouter({
>     routes:[//直接写在这里面的是一级路由
>         {
>             path:'/about',
>             component:About//组件名
>         },
>         {
>             path:'/home',
>             component:Home,
>             children:[//写在该配置项里面的是二级路由（数组套对象形式）
>                 {
>                     path:'news',//此处不应该写"/"
>                     component:News
>                 },
> 
>                 {
>                     path:'messages',//此处不应该写"/"
>                     component:Messages
>                 },
>             ]
>         }
>     ]
> })
> ```
>
> + 在一路由组件中使用二级路由
>
> ```html
> //这里是Home组件
> <router-link to="/home/news" class="list-group-item" active-class="active">News</router-link>//在这里写路径要带上父路由
> <router-link to="/home/messages" class="list-group-item" active-class="active">Messages</router-link>
> //在该页面引用
> <router-view></router-view>
> ```

多级跳转也一样层层嵌套children属性。

###### 总结：

```
3.多级路由（多级路由）
1.配置路由规则，使用children配置项：
       	routes:[
        path:'/about'.
        component:About,
        },
        {
        path:'/home',
        component:Home,
        children:[ //通过children配置子级路由
        {
        path:'news', //此处一定不要写：/news
        component:News
        }，
        {
        path:'message',//此处一定不要写：/message
        component:Message
        }
2. 跳转 (要写完整路径)
<router-link to="/home/news">News</router-link>
```

##### 74.路由的传参（query参数）

###### 跳转路由并携带query参数,to的字符串写法

（在路径后面用？传参）

> + 传递query参数

```html
<router-link to="/home/messages/detail?id=666&title=hello" class="list-group-item" active-class="active">消息一</router-link>
```

> + 获取传递过来的query参数($route获取)

```html
//接收参数界面
<template>
	<ul>
    <li>消息名称：{{$route.query.titlt}}</li>
    <li>消息编号：{{$route.query.id}}</li>
    </ul>
</template>
```

但是上述传递的参数是固定的，如果要传递动态数据应该用下面方法

```html
<template>
	<ul>
    <li v-for="m in messagelist" :key="m.id">
        <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link>
    </li>
    </ul>
</template>
<script>
export default{
    name:'Message',
    data(){
        return{
            messagelist:[
                {id:001,title:"001号消息"},
                {id:002,title:"002号消息"},
                {id:003,title:"003号消息"},
                {id:004,title:"004号消息"},
            ]
        }
    }
}
</script>
```

###### 跳转路由并携带query参数,to的对象写法

```html
<template>
	<ul>
    <li v-for="m in messagelist" :key="m.id">
        <router-link :to="{
             path:'/home/message/detail',
             query:{
                id:m.id,
                title:m.title   
                   }
        }">
            {{m.title}}
        </router-link>
    </li>
    </ul>
</template>
<script>
export default{
    name:'Message',
    data(){
        return{
            messagelist:[
                {id:001,title:"001号消息"},
                {id:002,title:"002号消息"},
                {id:003,title:"003号消息"},
                {id:004,title:"004号消息"},
            ]
        }
    }
}
</script>
```

##### 75.命名路由

新增name属性，简化路由的跳转

```js
import VueRouter from 'vue-router'
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
export default new VueRouter({
    routes:[
        {	name:'about',//新增name属性
            path:'/about',
            component:About
        },
        {	name:'home'//新增属性
            path:'/home',
            component:Home,
            children:[
                {	name:'news',//新增属性
                    path:'news',
                    component:News
                },
                {
                    path:'messages',
                    component:Messages
                },
            ]
        }
    ]
})
```

引用路径时:

```html
<template>
	<ul>
    <li v-for="m in messagelist" :key="m.id">
        <router-link :to="{
              name:'news', //这里不再写path（但是使用path也可以，为了简化书写才使用name属性）
            	query:{
                id:m.id,
                title:m.title   
                   }
        }">
            {{m.title}}
        </router-link>
    </li>
    </ul>
</template>
<script>
export default{
    name:'Message',
    data(){
        return{
            messagelist:[
                {id:001,title:"001号消息"},
                {id:002,title:"002号消息"},
                {id:003,title:"003号消息"},
                {id:004,title:"004号消息"},
            ]
        }
    }
}
</script>
```

##### 76.路由的传参（params参数）

直接把参数用/写在符号后面

```html
<router-link to="/home/messages/detail/666/hello" class="list-group-item" active-class="active">消息一</router-link>
```

在路由的配置中声明

```js
import VueRouter from 'vue-router'
import About from '../components/About'
import Home from '../components/Home'
export default new VueRouter({
    routes:[
        {	name:'about',
            path:'/about',
            component:About
        },
        {	name:'home'
            path:'/home',
            component:Home,
            children:[
                {	name:'news',
                    path:'news/:id/:title',//在这里声明一下传过去的参数
                    component:News
                },
                {
                    path:'messages',
                    component:Messages
                },
            ]
        }
    ]
})
```

在接收数据的界面

```html
<template>
	<ul>
    <li>消息名称：{{$route.params.titlt}}</li>
    <li>消息编号：{{$route.params.id}}</li>
    </ul>
</template>
```

但是上述传递的是固定的数据，现在要传变量应该如下

> + 方法一：字符串写法

```html
<template>
	<ul>
    <li v-for="m in messagelist" :key="m.id">
        <router-link :to="`/home/message/detail/id=${m.id}/title=${m.title}`">{{m.title}}</router-link>//这里改变
    </li>
    </ul>
</template>
<script>
export default{
    name:'Message',
    data(){
        return{
            messagelist:[
                {id:001,title:"001号消息"},
                {id:002,title:"002号消息"},
                {id:003,title:"003号消息"},
                {id:004,title:"004号消息"},
            ]
        }
    }
}
</script>
```

> + 方法二：对象写法

```html
<template>
	<ul>
    <li v-for="m in messagelist" :key="m.id">
        <router-link :to="{
                              name:'news',//这里必须用name属性，用path属性会报错
                              params:{
                                  id:m.id,
                                  title:m.title
                              }
                          }">
            {{m.title}}
        </router-link>//这里改变
    </li>
    </ul>
</template>
<script>
export default{
    name:'Message',
    data(){
        return{
            messagelist:[
                {id:001,title:"001号消息"},
                {id:002,title:"002号消息"},
                {id:003,title:"003号消息"},
                {id:004,title:"004号消息"},
            ]
        }
    }
}
</script>
```

###### 特别注意：

如果使用params方法传递参数，并用to的对象写法，只能使用name属性来指定路径，不可以使用path属性来指定

##### 77.路由的props配置项

谁接收东西就在谁里面写上这个配置项

###### 写法一

props是对象，对象中的所有key-value都以props的形式传给该组件

```js
	 routes:[
        {	name:'about',
            path:'/about',
            component:About
        },
        {	name:'home'
            path:'/home',
            component:Home,
            children:[
                {	name:'news',
                    path:'news/:id/:title',
                    component:News,
         			props:{a:1,b:2}//在该处配置
                },
                {
                    path:'messages',
                    component:Messages
                },
            ]
        }
    ]
})
```

在被配置该属性当中用props承接

```html
//news组件界面
<h1>{{props.a}}</h1>
<h1>{{props.b}}</h1>
<script>
export default{
    props:['a','b']
}
</script>
```

###### 写法二

props的值是布尔值，如果布尔值为真，则就会把该路由组件收到的所有params参数以props的形式传给该组件

```js
	 routes:[
        {	name:'about',
            path:'/about',
            component:About
        },
        {	name:'home'
            path:'/home',
            component:Home,
            children:[
                {	name:'news',
                    path:'news/:id/:title',
                    component:News,
         			props:true//在该处配置
                },
                {
                    path:'messages',
                    component:Messages
                },
            ]
        }
    ]
})
```

在被配置该属性当中用props承接

```html
//news组件界面
<h1>{{props.a}}</h1>
<h1>{{props.b}}</h1>
<script>
export default{
    props:['id','title']
}
</script>
```

###### 写法三

（最好用）

props的值为函数,并把该路由组件收到的所有参数（params和query均可以）以props的形式传给该组件

```js
	 routes:[
        {	name:'about',
            path:'/about',
            component:About
        },
        {	name:'home'
            path:'/home',
            component:Home,
            children:[
                {	name:'news',
                    path:'news/:id/:title',
                    component:News,
         				//在该处配置
         			props($route){
         				return {id:'$route.params.id',title:'$route.params.title'}
         			}
                },
                {
                    path:'messages',
                    component:Messages
                },
            ]
        }
    ]
})
```

在被配置该属性当中用props承接

```html
//news组件界面
<h1>{{props.a}}</h1>
<h1>{{props.b}}</h1>
<script>
export default{
    props:['id','title']
}
</script>
```

##### 78.router-link的replace属性

不断的替换浏览器的历史记录，只保持当前页面的历史记录在栈内存中（不可以通过浏览器导航栏进行前进或者后退）

实现方法：给router-link标签添加replace属性

```html
<router-link replace to="/home/messages/detail" class="list-group-item" active-class="active">消息一</router-link>
```

##### 79.编程式路由导航

不借助router-link实现的导航(通过按钮或者其他元素实现跳转)

###### 使用$router身上的push和replace方法，来实现跳转

```html
 <li v-for="m in messagelist" :key="m.id">
<button @click="pushShow(m)">push跳转</button>//在这里可以传递参数
</li>
<script>
export default{
    methods:{
        data(){
            return{
              messagelist:[
                {id:001,title:"001号消息"},
                {id:002,title:"002号消息"},
                {id:003,title:"003号消息"},
                {id:004,title:"004号消息"},
           				]
            }
        },
        pushShow(m){
            this.$router.push({//这里是找router身上的配置项,如果要使用replace方法跳转，直接替换掉push就好，其余配置和push相同
                name:'news', //这里的配置项和通过router-link标签跳转的一样
            	query:{
                id:m.id,
                title:m.title   
                   }
            })
        }
    }
} 
</script>
```

###### 使用$router身上的back和forword方法实现前进或者回退

```html
<button @click="back">后退</button>
<button @click="forward">前进</button>
<script>
export default{
    methods:{
        back(){
            this.$router.back()
        },
        forward(){
            this.$router.forword()
        }
    }
}
</script>
```

###### $router身上的go方法实现灵活跳转

```html
<button @click="testGo">按钮</button>
<script>
export default{
    methods:{
        testGo(){
            this.$router.go(2)//这里传入一个参数，参数为正时就是前进页面几步，参数为负时就是后退界面几步。
        }
    }
}
</script>
```

##### 80.缓存路由组件

因为组件切换以后，会被销毁。现在想实现不被销毁（即缓存），就需要借助keep-alive标签来实现

使用keep-alive包裹住router-view标签

```html
<keep-alive >
<router-view></router-view>
</keep-alive>
```

还可以引入keep-alive标签的include属性来指定被缓存的界面

```html
<keep-alive include="News">//这里就指定缓存了News组件，其他的同级组件并未被缓存。注意这里使用的是组件名（在组件的name属性中写的名字）
<router-view></router-view>
</keep-alive>
```

当想指定缓存多个组件时，可以用数组来实现

```html
<keep-alive :include="['News','Message']">
<router-view></router-view>
</keep-alive>
```

##### 81.路由组件的生命周期钩子

> + 激活钩子

activated( ){ }	当路由组件被展示时，该钩子被触发

> + 失活钩子

deactivated( ){ }	当路由组件被切换走时，该钩子被触发

##### 82.全局路由守卫

作用：校验用户是否有权限看到某个界面（需要在路由器中进行配置）

###### 全局前置路由守卫beforeEach

全局前置路由守卫(beforeEach)，初始化的时候也被调用，在每一次路由切换之前都会被调用。

```js
import VueRouter from 'vue-router'
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
const router = new VueRouter({//在这里不能立即暴露
    routes:[
        {	name:'about',
            path:'/about',
            component:About
        },
        {	name:'home'
            path:'/home',
            component:Home,
            children:[
                {	name:'news',
                    path:'news',
                    component:News
                },
                {	name:'message',
                    path:'messages',
                    component:Messages
                },
            ]
        }
    ]
})
//在这个位置做路由守卫工作
router.beforeEach((to,from,next)=>{//to参数是要去哪的信息，from参数是从哪来的信息，next是切换到下一个界面
    //next()方法是放行
    //可以在这里做判断，来实现控制界面是否可以跳转
    if(to.path === '/home/news' || to.path === '/home/message'){//这里的判断实现只对news和message组件的切换做校验，而不对他的上级路由组件切换做校验。除了用路径做校验，还可以用name属性来做校验
        if(localStorage.getItem('school')==='atguigu'){//这里以从本地调用校验信息为例，如果满足才放行切换，不满足就不切换放行
         next()   
        }
    }else{
        next()
    }
})

export default router
```

校验该组件是否应该被校验的简化方法（不使用path或者name属性）

往$router上的meta容器中添加路由元信息（特殊标识），来校验该组件是否应该被加上权限

```js
import VueRouter from 'vue-router'
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
const router = new VueRouter({
    routes:[
        {	name:'about',
            path:'/about',
            component:About,
         	meta:{isAuth:false}//在这里声明
        },
        {	name:'home'
            path:'/home',
            component:Home,
         	meta:{isAuth:false}，
            children:[
                {	name:'news',
                    path:'news',
                    component:News
                },
                {	name:'message',
                    path:'messages',
                    component:Messages
                },
            ]
})
router.beforeEach((to,from,next)=>{
    if(to.meta.isAuth){//在这里判断是否需要鉴权
         if(localStorage.getItem('school')==='atguigu'){
        			 			next()   
        					}
    					}else{
       				 next()
   						}
})
export default router
```



###### 全局后置路由守卫afterEach

全局后置路由守卫，在初始化的时候会被调用，在组件被切换后会被调用

```js
import VueRouter from 'vue-router'
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
const router = new VueRouter({
    routes:[
        {	name:'about',
            path:'/about',
            component:About,
         	meta:{title:'关于'}
        },
        {	name:'home'
            path:'/home',
            component:Home,
         	meta:{title:'主页'},
            children:[
                 {  name:'news',
                    path:'news',
                    component:News,
        			meta:{title:'新闻'}
                },
                 {  name:'message',
                    path:'messages',
                    component:Messages,
                    meta:{title:'消息'}
                }
            ]
        }
    ]
})
//在这个位置做路由守卫工作
router.afterEach((to,from)=>{//to参数是要去哪的信息，from参数是从哪来的信息，这里没有next参数
  document.title = to.meta.title || '系统'//在这里实现。网页标签名称可以动态改变的功能
})

export default router
```

##### 83.独享路由守卫(只有前置守卫)

作用：单独限制某个路由组件是否有权被切换

```js
import VueRouter from 'vue-router'
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
const router = new VueRouter({//在这里不能立即暴露
    routes:[
        {	name:'about',
            path:'/about',
            component:About,
         	meta:{isAuth:false}
        },
        {	name:'home'
            path:'/home',
            component:Home,
         	meta:{isAuth:false}
            children:[
                {	name:'news',
                    path:'news',
                    component:News
                },
                {	name:'message',
                    path:'messages',
                    component:Messages,
                 	beforeEnter:(to,from,next) =>{//在这里配置，里面的逻辑和全局守卫一样
                      if(to.meta.isAuth){//在这里判断是否需要鉴权
         				if(localStorage.getItem('school')==='atguigu'){
        			 			next()   
        					}
    						}else{
       						 	next()
   							}
                 								  }
                },
            ]
        }
    ]
})
```

##### 84.组件内的路由守卫

（写在组件内部的守卫）

###### beforeRouteEnter

调用时间：通过路由规则进入该组件页面后被调用

```html
//组件界面
<div>
    <h1>hello</h1>
</div>
<script>
export default{
    methods:{},
    beforeRouterEnter(to,from,next){
          if(to.meta.isAuth){//在这里判断是否需要鉴权
          if(localStorage.getItem('school')==='atguigu'){
        			 			next()
          						}else{
       						 	next()
   								}
    					}
}
</script>
```

###### beforeRouteLeave

调用时间：通过路由规则离开该组件页面后被调用

```html
//组件界面
<div>
    <h1>hello</h1>
</div>
<script>
export default{
    methods:{},
    beforeRouterLeave(to,from,next){
          next()//离开的时候要放行
    }
}
</script>
```

##### 85.路由器的工作模式

路径#后面的值都是哈希值，其中所有的内容都不会发给服务器

###### 哈希模式（默认）

路径显示不太好看（路径有#分隔），兼容性好，适配更多浏览器.在项目部署到服务器上的时候不会出路径问题

###### history模式

路径显示好看，但是对浏览器的兼容性比哈希模式略差，部署到服务器上容易出现路径问题

```js
//在router的配置文件进行配置
export default new VueRouter({
    mode:'history',
    routes:[
        
    ]
})
```

上述出现的部署路径问题可以通过中间件解决

使用node部署的解决该问题的中间件：connect-history-api-fallback

###### 总结：

```
1.对于一个url来说，什么是hash值？——#及其后面的内容就是hash值。
2. hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器。
3.hash模式:
1.地址中永远带着#号，不美观。
2.若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法。
3.兼容性较好。
4.history模式：
1.地址干净，美观 。
2. 兼容性和hash模式相比略差。
3. 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题。
```

##### 86.Vue的UI组件库

###### 移动端UI组件库推荐：

> + Vant
> + Cube UI
> + Mint UI

###### PC端UI组件库推荐：

> + Element UI
> + IView UI

elementui按需引入操作

> + 安装bable-plugin-component

> + 修改.babelrc 文件，但是在vue最新版本中该文件夹已经不存在了。所以现在应该修改vue项目中的bable.config.js文件
>
> 注意该文件中vue的原有配置不能改动，只能追加UI库属性。

但是由于UI库可能不再适配最新vue脚手架，官方文档也未及时更新，在按需引入的时候会出现问题

例如下面的这段应该在bable.config中配置的代码就会出现问题

```js
{
  "presets": [["es2015", { "modules": false }]],
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
-------------------------------------------------------------------------
//下面是正确方法
{
  "presets": [["@babel/preset-env", { "modules": false }]],//这里应该这样更改
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
```

还有可能出现缺少各种包的问题，缺啥直接npm下载就好。

##### 87.Vue3

