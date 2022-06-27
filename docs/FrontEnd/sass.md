##### 1.sass插件的设置

在vscode中安装Live Sass Compiler插件，并在setting.json中进行配置。

##### 2.sass的样式扩展

（相较于css来说）

> + 在代码中实行嵌套关系，使选择器关系更直观
>
>   ```scss
>   .container{
>   	.header{
>   		.title{
>   
>   		}
>   	}
>   	.footer{
>   
>   	}
>   }
>   ```
>
> + &符号代指父元素
>
>   ```scss
>   .container{
>   	.a{
>   		color:red;
>   		&:hover{	//这里的&就代指a，即这个设置的是a元素的伪类
>   		color:blue;
>   		}
>   	}
>   }
>   ---------------------------------------------
>   .container{
>   	.top{
>   		color:red;
>   		&-left{		//这里就相当于类选择器top-left的名字
>   			color:yellow;
>   		}
>   	}
>   }
>   ```
>
> + 可以直接进行拼接属性名，进行属性嵌套（当属性名前缀相同时）
>
>   ```scss
>   .container{
>   	a{
>   		color:red;
>   		font: {	//注意：后要有空格，这里面的属性就相当于font-size，font-family，font-weight
>   			size:20px;
>   			family:sans-serif;
>   			weight:bold;
>   		}
>   	}
>   }
>   ```
>
> + %base（占位符）选择器的使用
>
>   ```scss
>   //使用这种符号写的样式不会编译出来，只有使用@extend %base引用才会编译
>   .button%base{		//这里的样式不会转化
>   	color:red;
>   	font-size:20px;
>   }
>   .btn01{
>   	@extend %base;		//在这里引用时，才会进行编译
>   	border-color:yellow;
>   }
>   ```

##### 3.sass注释

在css中的注释：/**/

sass中的注释：(不唯一)

> + /* */ 	多行注释
>
>   会编译到最后的css文件当中
>
> + //        单行注释（和js一样）
>
>   但是单行注释不会在最终生成的css文件中生成

##### 4.sass变量

###### css中定义变量

> + 在 :root当中定义
>
>   ```css
>   :root{
>   	--color:red;   //这里需要用--来写
>   }
>   //在root当中定义完，就可以在任何选择器当中来使用
>   ```
>
> + 在body当中定义
>
>   ```css
>   body{
>   	--bac-color:red;
>   }
>   //在body中定义的变量在body包括的选择器当中都可以使用(同root)
>   ```
>
> + 在选择器当中定义
>
>   ```css
>   .header{
>   	--bac-color：red;
>   }
>   //只可以在该类当中使用
>   ```

###### css当中使用自定义变量

```css
:root{
	--color:red;  
}
p{
	color:var(--color);	//使用var()来调用自定义变量
}
```

注意：当在单个类当中定义变量时，只能先定义后使用（出现的顺序有先后）

###### sass变量的定义

```scss
$color:red;	//使用$来定义变量
$border-color:red;
$border_color:yellow;//上面这两个的是等价的（即使一个是横线，一个是下划线），并且后面会覆盖前面
.container{
	color:$color;	//直接引用即可
}
```

注意：应该先定义变量，再使用变量。

> + 变量的定义规则
>
>   ```scss
>   1.变量以美元符号$开头，后面跟变量名：
>   2.变量名是不以数字开头的可包含字母、数字、下划线、横线（连接符）
>   3.写法同CSS,即变量名和值之间用冒号(:)分隔：
>   4.变量一定要先定义，后使用：
>   ```
>
> + 变量作用域
>
>   ```scss
>   //局部变量
>   定义在选择器外部的就是全局变量；
>   使用 !global关键字的也是全局变量。
>   $color:red;		//这个是全局变量
>   .container{
>   	$border-color:yellow !global	//这个也是全局变量	
>   }
>   ```
>
>   ```scss
>   //局部变量
>   定义在单个类内部的变量（没有!global关键字）
>   ```
>
> + 变量值的类型
>
>   ```scss
>   变量值的类型可以有很多种,SASS支特6种主要的数据类型
>   数字:1,2,13,10px	(不需要加引号)
>   字符串:有引号字符串与无引号字符串，"foo”bar,baz
>   颜色:blue,04a3f9,rgba(255,0,0,0.5)
>   布尔型:true,false
>   空值:null
>   数组(list):用空格或逗号作分隔符1.5em 1em 0 2em,Helvetica,Arial,sans-serif
>   maps:相当于JavaScript的object,(key1:value1,key2:value2)
>   ```
>
> + 自定义变量的使用
>
>   ```scss
>   1.自定义布尔值的使用
>   $mode:true;
>   @if $mode {
>   	background-color:black;
>   }
>   @else {
>   	background-color:red;
>   }
>   2.其他类型引用
>   $var:null;
>   $font:(color1:red,color2:blue);
>   content:type-of($var);		//判断类型的函数
>   content:length($var);		//判断长度的函数
>   color:map-get($font,color1);	//这里的map-get是一个函数
>   ```
>
> + sass的默认值
>
>   ```scss
>   使用 !default关键字，如果已经定义了，那就使用定义值，如果没定义那么偶就使用默认值
>   $color:red;
>   $color:blue !default;
>   .container{
>   	color:$color;
>   }
>   ```

##### 4.sass导入功能（模块化）

###### css的导入功能

```css
@import url(css.css)
```

###### sass导入功能

```scss
@import 'public.scss';	//文件尾缀可以省略
//想要导入scss文件，必须使用这种方式导入
```

但是应该注意的是，当我们新建的scss文件只想用作引用功能，并不想把他编译为css文件，那么可以在新建的文件名称前加上下划线。

```scss
//新建一个_public.scss文件
//在sass.scss文件当中引用
@import 'public';	//这里引用的时候加不加下划线都可以（这也告诉我们，不能同时命名一个带下划线，一个不带下划线的文件名）
```

##### 5.sass混合指令

混合指令用于定义可重复使用的css样式，定义一次，无限制地引用。

> + 定义混合指令
>
>   ```scss
>   @mixin name {
>   //内部书写所有的css样式
>   }
>   //-----------------------
>   @mixin block {
>       color:red;
>   }
>   ```
>
> + 使用混合指令
>
>   ```scss
>   .container{
>       @include block;		//使用@include关键字来引用
>   }
>   ```
>
> + 混合指令当中嵌套样式
>
>   ```scss
>   @mixin warning {
>       .warn-text{
>           font-size:red;
>       }
>   }
>   .container{
>       @include warning;	//这里直接引用，会在编译的时候把warn-text转到和container同一个级别
>   }
>   ```
>
> + 混合指令传参
>
>   ```scss
>   @mixin block($item) {	//定义参数
>       aligin-items:$item;
>       -webkit-box-aligin:$item;
>   }
>   .container{
>       @include block($item:center)//在指定时传入参数
>       @include block(center);	//和上面效果相同，传参的简写形式
>   }
>   ```
>
> + 混合指令传递多个参数
>
>   ```scss
>   @mixin padding($top,$right,$left){//定义多个参数
>       padding-top:$top;
>       padding-right:$right;
>       padding-left:$left;
>   }
>   .container{
>       @include padding(10px,20px,10px)//传入多个参数
>   }
>   ```
>
>   ```scss
>   //注意：使用简写方式时，上面定义多少个参数，下面就得传入多少个参数
>   @mixin padding($top,$right,$left){
>       padding-top:$top;
>       padding-right:$right;
>       padding-left:$left;
>   }
>   .container{
>       @include padding(10px,20px,10px)
>   }
>   //当不使用简写方式引用时，在引用的时候可以不用写全参数
>   .container{
>       @include padding($left:20px,$right:20px)
>   }
>   //还可以在定义的时候传入默认值(这个方法是最好的)
>   @mixin padding($top:0,$right:0,$left:0){
>       padding-top:$top;
>       padding-right:$right;
>       padding-left:$left;
>   }
>   .container{
>       @include padding(10px,20px,10px)
>   }
>   ```
>
> + 混合指令传入一个包含多个值的参数
>
>   ```scss
>   @mixin linear($gradients...) {//这里使用...来表示一个多值参数
>       background-color:nth($gradients,1)//这里表示使用传入的多个元素的第一个值
>   }
>   .container{
>       @include linear(red,yellow,blue)
>   }
>   ```

混入使用场景：很多地方都会用，但是应该根据不同场景来确定样式（微信小程序，uniapp，vue当中都有混入指令）

##### 6.sass的@extend（继承）指令

使用场景：默认有一个基础类，并根据不同的条件，在这个基础类的基础上添加新的类

```scss
//先定义一个基础类
.alert{
    width:15px;
    height:20px;
}
//再分别把基础样式引入其他类
.alert-danger{
    @extend .alert;		//使用@extend关键字，继承了基础类样式
    background-color:yellow;
}
.alert-warning{
    @extend .alert;
    background-color:red;
}
//当需要继承多个样式时，直接写即可
.container{
    @extend .demo1;
    @extend .demo2;
}

```

当我们不想让基础样式在编译后的css文件当中引用时，可以使用占位符 % 来操作

```scss
%alert{	//在这里使用百分号定义（引用的时候也要使用百分号），那么在最终编译出来的css文件当中就不会出现alert这个类名
    width:15px;
    height:20px;
}
.alert-danger{
    @extend %alert;		
    background-color:yellow;
}
.alert-warning{
    @extend %alert;
    background-color:red;
}
```

注意：混入和继承可以实现相同的功能，但是有的时候继承会更简洁。

##### 7.sass运算符使用

> + 等号运算符
>
>   ```scss
>   //等于运算符
>   $theme:"red";
>   @if $theme=="red" {
>       background-color:red;
>   }
>   @else {
>       background-color:blue;
>   }
>   //不等于运算符：!=
>   //用法同上
>   ```
>
> + 比较运算符
>
>   ```scss
>   // >=
>   // <=
>   // >
>   // <
>   ```
>
> + 逻辑运算符
>
>   ```scss
>   //1.and(左右同时成立)
>   //2.or(左右成立一个即可)
>   //3.not(取反)
>   $height:100;
>   $weight:200;
>   $last:true;
>   $index:5;
>   @if $height>50 and $weight<200 {
>       font-size:16px
>   }
>   @else {
>       ront-size:12px;
>   }
>   @if not ($index > 5) {	//当后面是表达式的时候，应该用括号括起来
>       background-color:red;
>   }
>   @else {
>       background-color:blue;
>   }
>   ```
>
> + 数字操作符
>
>   ```scss
>   //数字类型包括：纯数字（1,23,），百分号（30%），css部分单位（10px，20pt···）
>   //1.加号
>   .container{
>       width:50 + 50;
>       width:50 + 50%;	//这里最终的编译结果是100%（自动转化为百分数）
>       width:50% + 50%;
>       width:50 + 50px; //这里最终的编译结果是100px（自动转化为单位）
>       width:10px + 10pt; //这里会自动进行单位转化（生成统一单位）
>       //注意百分号和单位符号不可以运算（width：10px + 10% 会报错）
>   }
>   //-----------------------------------------------------------------
>   //2.减号
>   .container{
>       width:50 - 30;
>       width:50 - 30%;//最终生成20%
>       width:20px - 10px;
>       width:20px - 20pt;//最终会换算单位
>       width:50px - 10;
>       //注意百分号和单位符号不可以运算
>   }
>   //-----------------------------------------------------------------
>   //3.乘号
>   .container{
>       width:5 * 5;
>       width:5 * 10%;//最终生成50%
>       width:10% * 50%;//这样是不允许的（两个百分号不可以相乘）
>       width:10px * 20px;//这样也是不允许的，会报错。（两个px不可以做乘运算）
>       width:20 * 10px;//最终会生成200px
>   }
>   //-----------------------------------------------------------------
>   //4.除号
>   .container{
>       width:10 / 5; //编译出来的不是2，仍旧是10/5表达式，因为这种情况下“/”没有被当做除法运算
>       width:(10/5);//加上个括号后就可以了
>   }
>   //以下三种情况/将被视为除法运算符号：
>   //如果值或值的一部分，是变量或者函数的返回值
>   //如果值被圆括号包裹
>   //如果值是算数表达式的一部分
>   .div{
>       width:($width/2);
>       z-index:round(10)/2;
>       height:(10px/2);
>       margin-left:5px + 8px/2px
>   }
>   //-----------------------------------------------------------------
>   //5.取模运算（%）
>   .container{
>       width:10 % 5;//最终输出0
>       width:50 % 3px;//最终输出2px（与带单位的做取模，最后算出来会补充单位）
>       width:50px % 3px;//最终输出2px（单位不会去除）
>       width:50% % 7;//最终输出1%
>   	width:50px % 7;//最终输出1px
>       width:50% % 2px;//会报错，不可以进行这样的运算
>       width:50px % 10pt;//(10pt=13.33333px)最终输出10px
>   }
>   ```
>
> + 字符串运算符
>
>   ```scss
>   // +
>   .container{
>       content:"foo" + bar;------->输出"foobar"
>       content:foo + "bar";------->输出foobar
>       content:foo + bar;------->输出foobar
>       content:"foo" + "bar";------->输出"foobar"
>   }
>   //加号右边的字符串有引号，那么结果字符串无引号
>   //反之，输出字符串有引号
>   //注意这里存在一个问题：当加号两边有一个值是选择器函数的返回值时，便不满足上述规则，无论引号是在左边还是右边都会给拼接出的字符串加上引号
>   ```

##### 8.sass的插值语句#{}

/（分隔符）常用在字体样式上面

```scss
.container{
    font:10px/30px Helvetica;	//第一个是字体大小，第二个是行高，第三个是字体样式
}
```

###### 使用插值语句动态决定属性(#{})

```scss
$font-size:20px;
$line-height:30px;
.container{
    font:#{$font-size}/#{$line-height} Helvetica;//这里使用#{}便动态绑定了数据
}
```

###### 插值语句用途

> + 应用于选择器
>
>   ```scss
>   $class-name:danger;
>   a.#{class-name} {
>       color:red;
>   }
>   ```
>
> + 应用于属性名
>
>   ```scss
>   $attr-name:color;
>   a.danger {
>       #{color}:red;
>       border-#{attr-name}:red;
>   }
>   ```
>
> + 应用于属性值
>
>   ```scss
>   $font-size:20px;
>   $line-height:30px;
>   .container{
>       font:#{$font-size}/#{$line-height} Helvetica;
>   }
>   ```
>
> + 应用于注释
>
> ```scss
> $author:"张三";
> /*
> @author:#{$author}
> */
> ```

##### 9.sass常见函数的基本使用

> + Color(颜色函数)
>
>   ```scss
>   //1.调亮函数:lighten(颜色，调亮程度)
>   p{
>       color:lighten(#5c7a29,30%);//这里代表把这个颜色调亮30%
>   }
>   //2.调暗函数：darken(颜色，调暗程度)
>   p{
>       color:darken(#5c7a29,30%);//这里代表把这个颜色调暗30%
>   }
>   //3.改变透明度函数：opacify(颜色，透明度)，（透明度在0到1之间）
>   p{
>       opactify(rgba(#5c7a29,0.3),0.5);//注意这两处的透明度之和不可以超过1
>   }
>   ```
>
> + String(字符串函数)
>
>   ```scss
>   //1.转化成字符串函数：quote(想要转成字符串的内容)
>   p{
>       &:after {
>           content:quote(这是一段文字)
>       }
>   }
>   //2.把字符串转化字符串函数：unquote(先要被转化的内容)
>   p{
>       background-color:unquote($string: "#F00")
>   }
>   //3.获取字符串长度函数：str-length(字符串)
>   p{
>       z-index:str-length("这是一段话")
>   }
>   ```
>
> + Math(数字函数)
>
>   ```scss
>   p{
>       z-index:abs($number:-15);//输出15.这个是取正值函数
>       z-index:ceil(5.8);//输出6，这个是向上取整函数
>       z-index:max(2,3,4);//输出4，这个是查找最大值函数
>       opacity:random();//生成一个0到1的随机数
>   }
>   ```
>
> + List(数组函数)
>
>   ```scss
>   p{
>       z-index:length(12px);//返回长度，这个输出结果是1
>       z-index:length(12px,23px,34px);//这个返回的长度是3
>       z-index:index(a s d f,s);//这个表示的含义是从前面的列表中找到s的位置，并返回他的索引值（注意这里的索引值是从1开始的），这个就输出2
>       padding:append(10px 20px,40px);//这个表示把40px添加到其那面的列表中（添加到后面），输出为10px 20px 40px
>       color:nth($list:red blue green,$n: 2);//这个表示输出指定位置的值，输出为blue
>   }
>   ```
>
> + Map(对象函数）
>
>   ```scss
>   $font-size:("small":12px,"large":20px);
>   $padding:(top;10px,right:20px,left:20px);
>   p{
>       font-size: map-get($font-size,small);//这里是获取$font-size对象中的small属性的值，输出为12px
>   
>       @if map-has-key($padding,"right") {//这里表示判断$padding对象中是否有right属性，如果有就返回true
>           padding-right:map-get($padding,"right")
>       }
>   
>       &:after {
>           content:map-key($font-size) + " " + map-values($padding) + " ";
>           //上述表示把指定对象中的属性和属性值都取出来
>       }
>   }
>   ```
>
> + selection(选择器函数)
>
>   ```scss
>   .header{
>       content:selector-append(".a",".b") + "";//最终输出content;".a.b.c";
>       content:selector-unify("a",".disable") + "";//最终输出content:"a.disabled";
>   }
>   ```
>
> + 自检函数
>
>   ```scss
>   $color:red;
>   @mixin padding($left:0,$top:0) {
>       padding:$top $left;
>   }
>   .container {
>       @if variable-exists(color){//判断是否存在$color变量（判断的时候$应该省略）
>           color:$color;
>       }
>       @if mixin-exists(padding){//判断是否存在padding混入
>           @include padding($left:20px,$top:30px);
>       }
>   }
>   ```

##### 10.sass流程控制指令

> + if指令
>
>   ```scss
>   $theme:"red";
>   @if $theme == "red" {
>       color:red;
>   }
>   @else if $theme == "blue" {
>       color:blue;
>   } 
>   @else $theme == "green" {
>       color:green;
>   }
>   ```
>
> + for指令(对有规律的选择器进行批量构造)
>
>   ```scss
>   //for指令可以在一定的范围内进行重复的输出
>   //1.使用to进行循环时，则循环不包含结束值
>   @for $i from 1 to 4 {
>       .p#{$i} {
>           width:10px * $i;
>           height:20px;
>           background:red;
>       }
>   }
>   //这个输出的结果是输出三个代码块，选择器名称分别是.p1,.p2,.p3,并且每一个宽度都不一样
>   //---------------------------------------------------------------------
>   //2.使用through进行循环时，包含最后一个
>   @for $i from 1 through 4 {
>       .p#{$i} {
>           width:10px * $i;
>           height:20px;
>           background:red;
>       }
>   }
>   //最终输出的选择器为四个，分别是：.p1,.p2,.p3,.p4
>   ```
>
> + each指令用法
>
>   ```scss
>   //each指令用来遍历列表（数组）
>   p{
>       width:10px;
>       height:10px;
>   }
>   $color-list:red green blue;
>   @each $color in $color-list {//这里是each指令的语法
>       $index : index($color-list,$color);//这里是获取索引值(从1开始)
>       .p#{$index - 1} {//这里减号必须有空格
>           background-color:$color;
>       }
>   }
>   //最终生成的效果是，生成了p0,p1,p2三个选择器，对应着三个不同的颜色
>   ```
>
> + while指令
>
>   ```scss
>   //持续执行循环，直到条件为false才会跳出循环
>   @while 条件 {
>   
>   }
>   //----------------------------------------------
>   $count:12;
>   @while $count > 0 {
>       .col-sm-#{$count} {
>           width:$count / 12 * 100%;
>       }
>       $count : $count - 1;
>   }
>   //最终输出12个不同名字的选择器，并且每个的宽度均不相同
>   ```





