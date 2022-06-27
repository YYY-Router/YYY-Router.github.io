##### 1.初识node.js

个人理解：他为JavaScript提供了后端的运行环境，即满足了使用JavaScript开发后端的需求。

###### node简介

> + 什么是node.js：他是一个基于Chrome V8 引擎的JavaScript运行环境

> + node.js的组成部分：V8引擎，内置API

> + node无法调用DOM和BOM等浏览器的内置API

###### node用途

> + 基于express框架可以快速构建出web应用
> + 基于electron框架可以构建跨平台桌面应用
> + 基于restify框架可以快速构建API接口项目
> + 读写操作数据库，创建实用的命令行辅助工具等

##### 2.安装node.js环境

node官网：[Node.js (nodejs.org)](https://nodejs.org/zh-cn/)

> + 版本选择：LTS版本是长期稳定版，Current是新特性版本,可能会存在漏洞
> + 一般最好不要更改安装路径，一路默认安装即可
> + 在终端中输入node -v后即可以查看安装的node版本

##### 3.常用终端命令

> + 切换文件路径：cd +空格+想要切换的的路径

##### 4.nodejs基础操作

###### node打开js文件

> + 方法一：先在终端切换到文件所在位置，然后在终端输入：node +空格+ 要执行的js文件存放的路径
> + 方法二：在js文件中，点击shift+右键，选择打开在该处的命令行窗口

###### 终端中常用的快捷键

> + 在终端中按 “PgUp”（键盘上键），直接就可以定位到上一次执行的命令（在光标处补全上一次的命令）
> + 使用“Tap”键，可以自动补全文件路径（直接敲第一个文字，然后点击tap键，就可以自动补全路径）
> + 使用“Esc”键可以快速清空当前已经输入的命令
> + 使用“cls”命令，可以清空前面的命令

##### 5.fs文件系统模块

fs文件系统模块是node提供的用来操作文件的模块，他提供了一系列的方法和属性，来满足用户对文件的操作需求.(node自动安装好了)

###### 在js代码中使用fs模块

> + 导入fs模块
>
> ```js
> const fs = require('fs')
> ```
>
> + fs.readFile()方法可以读取指定文件中的内容
>
> ```js
> fs.readFile(path[,options],callback)//在[]中的参数，表示不是必须参数
> //path参数是表示读取文件的路径，options表示文件的编码格式，callback表示读取完成（成功或失败）后对文件的操作,无论成功还是失败都会执行回调函数
> ```
>
> 示例：
>
> ```js
> //新建一个js文件，下面是在js、文件中的内容
> const fs = require('fs')
> fs.readFile('./files/11.txt','utf8',function(err,dataStr)[
>     //第一个参数是读取是读取失败的结果，第二个是读取成功的结果
>     //如果读取成功，则err的值为null，dataStr为文件中的内容
>     //如果读取失败，err的值为错误对象，dataStr的值为undefined
>         if(err){
>             return console.log('读取失败',err.message)
>         }
>           console.log('读取文件成功',dataStr)
> ])
> ```
>
> + fs.writeFile()方法向指定文件写入内容
>
> ```js
> //如果没有存放的文件，则执行该方法后，他会自动创建。但是注意的是，该方法只可以创建文件，不可以创建路径（文件的上级路径文件不会被创建）。
> //如果重复对一个文件写入，只会覆盖，并不会生成重复文件
> fs.eriteFile(path,data[,options],callback)
> //参数一是指想要写入的文件路径。参数二是想要写入的内容。参数三是非必要参数，表示写入的文件格式。参数四是文件写入完成后的回调函数
> ```
>
> 示例：
>
> ```js
> const fs = require('fs')
> fs.writeFile('./files/demo.txt','abcd',function(err){//第三个参数可不写，默认是utf-8
>     //如果文件写入成功后则err的值是null
>     //如果写入失败，则err的值是一个对象
>     if(err){
>            console.log('文件写入失败',err.message)
>     }
>     	console.log('文件写入成功')
> })
> ```

文件读写案例：实现一个成绩整理的功能

```js
const fs = require('fs')
fs.readFile('./files/原成绩.txt','utf8',function(err,dataStr){
    if(err){
        return console.log('读取失败',err.message)
    }else{
        //把读取的成绩数据按照空格分割
        const arrOld = dataStr.split(' ')
        //循环切割后的数组,对每一项数据进行字符串操作
       const arrNew = []
        arrOld.forEach(item =>{
            arrNew.push(item.replace('=',':'))
        })
        //把数组中的每一项加上换行，并返回字符串
        const newStr = arrNew.join('\r\n')
        //把整理好的数据写入到新的文件
        fs.writeFile('./files/ok.txt',newStr,function(err){
            if(err){
                console.log("成绩写入失败",err.message)
            }
            console.log("成绩写入成功")
        })
    }
})
```

###### fs模块路径动态拼接错误问题

> + 以相对路径（./）拼接路径容易出现错误（代码在运行的时候，会执行以node命令所处目录，动态拼接出操作文件的完整路径）

解决办法一：在使用fs模块时，提供一个完整的路径，这样就避免了路径拼接错误

```js
fs.readFile('D:\\代码仓库\\A学习代码\\A日常练习\\js练习\\text.txt','utf8',function(err.dataStr){//一个斜线代表转义，所以应该补全一个斜线
    
})
```

但是上述方法不利于后期的维护，移植性比较差。

解决办法二：使用'__dirname'表示这个文件所处的目录，他的值不会跟随node执行的位置而发生变化

```js
fs.readFile(__dirname+'./files/text.txt','utf8',function(err.dataStr){//一个斜线代表转义，所以应该补全一个斜线
    
})
```

##### 6.path路径模块

path模块是node提供的，用来处理路径的模块，他提供了一系列的方法和属性，来满足对路径处理的需求

###### 在js中使用path模块

> + 在文件中导入path模块
>
> ```js
> const path = require('path')
> ```
>
> + path.join()把多个路径片段拼接成完整的路径字符串
>
> ```js
> const pathStr = path.join('/a','/b/c','../','./d','e')	//注意‘../’会抵消一层他前面的路径
> //则此时的psthStr为：\a\b\d\e
> const pathStr2 = path.join(__dirname,'./files/1.txt')
> //此时输出pathStr2位：当前文件所处的目录\files\1.txt
> ```
>
> 今后凡是涉及到路径拼接的操作，都要使用path.join()方法进行处理，不要使用+进行字符串拼接
>
> ```js
> fs.readFile(path.join(__dirname,'./files/1.txt'),'utf8',function(err.dataStr){
> 
> })
> ```
>
> + path.basename()获取路径中的最后一部分，经常通过这个方法来获取路径中的文件名
>
> ```js
> path.basename(path[,ext])
> //第一个参数是文件路径，第二个参数是想要去除的文件拓展名
> const fpath = '/a/b/c/index.html'
> var fullName = path.basename(fpath)
> //此时输出fullName为index.html
> var nameWithoutExt = path.basename('fpath','.html')
> //此时输出nameWithoutExt为index（把扩展名删掉，只输出文件名）
> ```
>
> + path.extname()获取路径中扩展名部分
>
> ```js
> const fpath = '/a/b/c/index.html'
> const fext = path.extname(fpath)
> //此时输出fext的名为.html
> ```

正则表达式的匹配方法

```js
  var reg = /<style[^>]*>[\s\S]*<\/style>/gi;
const r1 = reg.exec(需要被匹配的内容)	//这里的r1是一个数组
```

##### 7.http模块

服务器和客户端的区别：是否装了web服务器软件

​		http模块就是专门用来创建web服务器的模块，通过http模块提供的http.createServer()方法，就可以把一台普通的电脑，变成一台web服务器，从而对外提供服务资源。

###### 服务器的相关概念

> + IP地址：每台计算机的唯一地址，IP地址有固定的格式（0到255之间）
>
>   ​	ping+网址，既可以查看该网址的IP地址。
>
> + 域名和域名服务器
>
>   ​	IP和域名是一一对应的关系，这份对应关系存放在一种叫做域名服务器（DNS）的电脑中，使用者只需要通过好记的域名访问对应的服务器即可，对应的转换工作由域名服务器实现。因此，域名服务器就是提供IP和域名之间转换的服务器。
>
> + 端口号
>
>   ​	在一台服务器中可以运行多个web服务，每个web服务都有唯一一个与之对应的端口号。客户端发过来的网络请求通过端口号，可以准确的交给web服务器处理（每个端口号不可以同时被多个web服务占用）。在实际应用中url中的80端口可以被忽略
>
> 

###### 使用http模块创建基本web服务器

> + 导入http模块
>
> ```js
> const http = require('http')
> ```
>
> + 创建web服务实例
>
> ```js
> const server = http.createServer()
> ```
>
> + 为服务器绑定request事件，监听客户端请求
>
> ```js
> server.on('request',function(req,res){
>     console.log('服务器被请求了')
>     //req参数存储的是与客户端相关的数据或者属性，例如：他的url属性就是客户端请求地址，method属性就是客户端请求方法
>     const url = req.url
>     const method = req.method
>     //res参数是服务器响应的内容数据或者属性，可以通过end()方法返回
>     //调用res.end()方法向客户端发送指定内容，并结束这次请求处理过程
>     const str =`你的访问地址为${url},你的访问方式是${method}`
>    	res.end(str)
> })
> ```
>
> + 启动服务器
>
> ```js
> server.listen(80,function(){
>     console.log('服务器被启动了，127.0.0.1:8080')
> })
> ```
>
> 在编译器终端启动服务器：node+文件名

###### 解决中文乱码问题

如果在request的函数中直接返回给客户端中文的话，浏览器会出现乱码问题，这时候就需要设置请求头

```js
res.setHeader('Content-Type','text/html; charset=utf-8')
```

```js
server.on('request',function(req,res){
    console.log('服务器被请求了')
    const url = req.url
    const method = req.method
    const str =`你的访问地址为${url},你的访问方式是${method}`
    res.setHeader('Content-Type','text/html; charset=utf-8')	//在这里设置请求头，解决中文乱码问题
   	res.end(str)
})
```

###### 根据不同的url响应不同的内容

> + 获取url地址
> + 设置默认响应内容为404 Not found
> + 判断用户是否请求为/或者/index.html首页
> + 判断用户是否请求为/about.html关于界面
> + 设置Content-Type响应头，防止中文乱码
> + 使用res.end()把内容响应给客户端

```js
const http = require('http')
const server = http.createServer()
server.on('request',(req,res)=>{
    const url = req.url
    let content = `<h1>404 Not found</h1>`
    if(url === '/' || url === 'index.html'){
        content = `<h1>首页</h1>`
    }else if(url === '/about'){
        content = `<h1>关于</h1>`
    }
    res.setHeader('Content-Type','text/html; charset=utf-8')
    res.end(content)
})
server.listen(80,()=>{
    console.log('服务器被启动了')
})
```

###### 把web资源放在模拟服务器上

> + 核心思路
>
> 把web资源文件的实际存放路，径作为每个资源的请求的url地址。（服务器正在浏览器和磁盘目录之间起到一个媒介的作用）
>
> + 实现步骤
>
>   1.导入需要的模块	2.创建基本的web服务器	3.将资源的请求url地址映射为文件的存放路径	4.读取文件并把资源返回给客户端	
>
> ```js
> const http = require('http')
> const fs = require('fs')
> const path = require('path')
> const server = http.createServer()
> server.on('request',(req,res)=>{
>     const url = req.url//这个客户端请求的路径就是文件名，并没有前面的网址
>     //clock/index.html
>     //clock/index.css
>     //clock/index.js
>     //把请求的url地址映射为本地存放的路径（拼接出完整路径）
>     const fpath = path.join(__dirname,url)
>     fs.readFile(fpath,'utf8',(err,dataStr)=>{
>         if(err)return res.end('404 not found')
>         res.end(dataStr)
>     })
> })
> server.listen(80,()=>{
>     console.log('server runing at http://127.0.0.1')
> })
> ```
>
> + 优化资源的路径（不希望路径过长）
>
> 设置一个默人的首页，并把clock文件路径给拼接上
>
> ```js
> const http = require('http')
> const fs = require('fs')
> const path = require('path')
> const server = http.createServer()
> server.on('request',(req,res)=>{
>     const url = req.url
>    	let fpath = ''
>     if(url === '/'){
>         fpath = path.join(__dirname,'/clock/index.html')//这里就设置了默认首页
>     }else{
>         fpath = path.join(__dirname,'/clock',url)//帮助用户来书写路径，这样用户就不需要手写过长的路径
>     }
> 
>     fs.readFile(fpath,'utf8',(err,dataStr)=>{
>         if(err)return res.end('404 not found')
>         res.end(dataStr)
>     })
> })
> server.listen(80,()=>{
>     console.log('server runing at http://127.0.0.1')
> })
> ```

##### 8.模块化

> + 编程中的模块化，指的是遵守固定规则，把一个大文件拆成独立并相互依赖的多个小模块。

###### node中的模块分类

> + 内置模块（由node官方提供的，例如fs，path等）
> + 自定义模块（用户创建的每一个js文件）
> + 第三方模块（并非官方开发出来的模块，也不是用户创建的模块，需要先下载使用）

###### 加载模块

> + 使用require（）方法，加载三种模块
>
> 注意：加载自定义模块的时候需要写文件路径（文件的后缀名可写可不写），而加载官方和第三方模块直接写模块名字即可
>
> + 在使用require（）方法时，会自动执行加载模块中的代码

###### node中的模块作用域

与函数的作用域类似，在自定义模块中定义的变量只能在该模块内被访问。这样就避免了全局变量污染的问题。

###### 向外共享模块作用域中的成员

> + module对象
>
> 在每个自定义的js模块中都有一个module对象，他里面存储了和当前模块有关的信息
>
> + module.exports对象
>
> 在自定义模块中可以使用module.export对象，将模块内的成员共享出去，共外界使用。外部用require（）方法导入自定义模块时，得到的就是module.exports所指的对象
>
> + 使用module.exports对象来暴露模块内容
>
> ```js
> module.exports.name = 'Tom'
> //此时在外部文件导入该模块时，就会得到该该模块的name属性
> ```
>
> 注意：使用require（）方法导入模块时，导入的结果永远是以module.exports指向的对象为准
>
> ```js
> module.exports.name = 'Tom'//给module.exports挂在name属性
> module.exports = {//给module.exports指向新对象
>     nickName:'Jerry'
> }
> //此时在外部导入该模块时，会得到nickName，而不是name，因为对象的最终指向是下面的对象，旧对象则会被覆盖掉
> ```
>
> + exports对象
>
> 由于 module.exports 单词写起来比较复杂，为了简化向外共享成员的代码，Node 提供了exports对象。默认情况下, exports 和 module.exports 指向同一个对象。*最终向外共享的结果，还是以 module.exports 指向的对象为准*。
>
> ```js
> const username = 'Tom'
> exports.username = username
> export.age = 20
> export.sayHello = function(){
>     console.log('Hello')
> }
> ```
>
> + exports和module.exports使用的误区
>
> 时刻谨记，导入的模块永远都是module.expores里面的内容
>
> ```js
> exports.username = 'Tom'
> module.exports = {
>     nickName:'Jerry'
> }
> //此时咋外部使用该模块的时候，该模块里面的内容是 nickName:'Jerry'，并没有Tom
> ```
>
> ```js
> module.exports.username = 'Tom'
> exports = {
>     nickName:'Jerry'
> }
> //此时外部使用该模块时，该模块里面的内容是username = 'Tom'，并没有Jerry
> ```
>
> ```js
> module.exports.username = 'Tom'
> exports.gender= '男'
> //此时在外部引用该模块时，会得到上面两个属性
> ```
>
> ```js
> exports = {
>   age:18
> }
> module.exports = exports
> module.exports.gender= '男'
> //此时在外部引用该模块时，会得到上面两个属性
> ```
>
> 注意：为了防止混乱最好不要在同一个模块中，同时使用module.exports和exports

###### node中的模块化规范

他遵循了Common.js模块化规范

Common.js规定：

> + 每个模块内部，module变量代表当前模块
> + module变量是一个对象，他的exports属性（即module.exports）是对外的接口
> + 加载某个模块，其实就是加载该模块的module.exports属性，require（）方法用于加载模块

##### 9.npm与包

> + 包：node.js中的第三方模块叫做包
> + 包的来源：由第三方提供（个人或者团队），免费且开源
> + 为什么需要包：node内置的包功能有限，使用第三方包（基于内置模块封装），可以提高开发效率
> + 包的搜索网站：https://www.npmjs.com	(全球最大的包共享平台)
> + 包的下载服务器：https://registry.npmjs.org（浏览器打不开）

###### 如何下载包

> + npm包管理工具，这个工具在安装node的时候就已经被装上了

> + npm初体验
>
>   例如：使用格式化时间的包：moment
>
>   先用npm安装，再引入包，再看包的API文档来实现功能

注意：装完包以后，需要进行导入才能使用，导入包的名称是字符串，并且和下载的包名一样

##### 10.npm装包

> + 初次装包后多了哪些文件
>
> 初次装包完成后，在项目文件夹下多一个叫做 node_modules 的文件夹和 package-lockjson 的配置文件。
>
> node_modules 文件夹用来存放所有已安装到项目中的包。require() 导入第三方包时，就是从这个目录中查找并加载包。
>
> package-lockjson配置文件用来记录node_modules 目录下的每一个包的下载信息，例如包的名字、版本号、下载地址等。
>
> 注意：千万不要去手动修改上面两个文件夹中的内容

> + 安装指定版本的包（默认情况安装最新版的）
>
> ```
> npm i moment@2.22.2	 使用@来指定版本
> ```
>
> 即使安装不同版本的包，包也不会重复存在，后面安装的会自动覆盖前面安装的内容。

> + 包版本号的解读
>
>   第一位数字：大版本
>
>   第二位数字：功能版本
>
>   第三位数字：bug修复版本
>
>   版本号提升规则：只要前面的版本号增长了，则后面的版本号归零

> + 包管理配置文件
>
> npm规定，在项目的根目录中，必须提供一个叫做package.json的包管理配置文件，用来记录有关的项目配置信息，例如：
>
> 项目的名称、版本号、描述等。项目中都用到了哪些包 哪些包只在开发期间会用到 那些包在开发和部署时都需要用到

> + 多人协作，包体积过大问题
>
>   在项目的根目录中，创建一个叫做packages.json的配置文件，即可以用来记录项目中安装了哪些包，从而方便剔除node_modules目录之后，在团队成员之间共享项目代码
>
> 注意：在今后的开发中，一定要把node_modules文件夹，添加到.gitignore忽略文件夹

###### 快速创建package.json包管理文件

```js
//执行命令
npm init -y	//在可执行命令所处的目录中使用该命令
```

注意：

> + 该命令只可以在英文目录下才可以运行成功！！！所以项目文件夹的名字一定要用英文名，不可以出现中文或者空格
> + 创建package.json以后，再使用npm装包时，会自动把包的信息写在该文件中，不需要手动写入

> + package.json中的dependencies节点，存储着安装过的包的信息。
> + 同时安装多个包
>
> ```
> npm i 包1名 包2名···
> ```

> + package.json中的devDependencies节点,中存储着只在开发中的使用的包，项目上线时并不会使用
>
>   需要如下命令会把包记录在devDependencies中
>
>   ```
>   npm i 包名 -D
>   npm i -D 包名
>   等价于下面写法
>   npm install 包名 --save-dev
>   ```
>
>   注意：在包的文档处就有说明，该包的信息是记录在dependencies中还是devDependencies中

###### 拿到剔除了node_modules文件的项目

当我们拿到一个剔除了 node_modules 的项目之后，需要先把所有的包下载到项目中，才能将项目运行起来。

```
运行命令：npm install (或者简写为：npm i )即可一次性安装所有的依赖包
```

他是根据package.json中的dependencies节点中的内容进行安装的（包名称和版本号）

###### 卸载包

```
npm uninstall 包名
```

注意：当卸载完成后，会自动把被卸载的包的信息从package.json中的dependencies节点中移除

###### 解决下包速度慢的问题

> + 淘宝NPM镜像服务器
>
>   切换npm下包镜像源
>
> ```js
>     //查看当前的下包镜像源
>     npm config get registry
>     //将下包镜像源切换为淘宝镜像源
>     npm config set registry=https://registry.npm.taobao.org/
>     //检查镜像源是否下载成功
>     npm config get registry
> ```

> + nrm工具
>
>   为了更方便的切换下包的镜像源，我们可以安装nrm 这个小工具，利用nrm提供的终端命令，可以查看和切换下包的镜像源。
>
> ```js
>      //安装nrm工具
>     npm i nrm -g
>     //查看所有可用的镜像源
>     nrm ls
>     //把下包的镜像源切换为taobao镜像
>     nrm use taobao 
> ```

##### 11.包的分类

> + 项目包（被安装到node_modules目录中的包，都是项目包）
>
> 项目包又分两类：
>
> 开发依赖包(被记录到 devDependencies 节点中的包，只在开发期间会用到)
> 核心依赖包（被记录到 dependencies 节点中的包，在开发期间和项目上线之后都会用到)

> + 全局包
>
> 在执行npm install 命令时，如果提供-g参数，则会把包安装为全局包
>
> 全局包会被安装到：C:\User\用户目录\AppData\Roaming\npm\node_modules目录下
>
> ```js
> //安装全局包
> npm i 包名 -g
> //卸载全局安装包
> npm uninstall 包名 -g
> ```

注意：只有工具性的包，才有必要全局安装。因为他们提供了好用的命令工具。

​			判断一个包是否需要全局安装，参看官方文档即可。

###### i5ting_toc

他是一个可以把md文档转化为html页面的小工具，使用步骤如下

```js
//把i5ting_toc安装为全局安装包
npm install -g i5ting_toc
//调用i5ting_toc，实现md转html的功能
i5ting_toc -f 要转换的md文件路径 -o
//可以先切换到md文件所在路径，然后在-f后输入文件名称（-o表示转化完成后，自动在浏览器打开）
```

###### 规范的包结构

一个规范的包，他的结构必须符合如下条件：

```
包必须以单独的目录而存在
包的顶级目录下要必须包含 package.json 这个包管理配置文件
package.json 中必须包含 name，version，main 这三个属性，分别代表包的名字、版本号、包的入口。
```

###### 开发一个属于自己的包

> + 新建一个文件夹demo-tool，作为包的根目录
>
> + 在demo-tool文件夹中新增三个文件：
>
>   package.json	(包的管理配置文件)
>
>   index.js	(包的入口文件)
>
>   README.md	(包的说明文档)
>
> + 初始化package.json文件
>
> ```js
> {
> "name":"demo-tool",//配置包的名称，以供下载（这个属性和存包的文件夹名称无关），并且包名不可以重复
> "version":"1.0.0",
> "main":"index.js",//入口文件
> "description":"提供了xxx功能",//简短的描述信息
> "keywords":["demo-tool"，"demo-tool"],//搜索的关键词
> "license":"ISC"//开源许可协议
> }
> ```
>
> + 在index.js文件中写逻辑
>
> + 将不同逻辑的代码进行拆分
>
>   新建src文件夹，在该文件夹下 新建不同功能的js文件，并把实现不同功能的代码放进去。
>
>   然后在各自的js文件中导出， 并在index文件中进行导入导出（index.js是入口文件）
>
>   ```js
>   //这是在index.js入口文件中
>   const data1 = require('./src/demo1')
>   const data2 = require('./src/demo2')
>   module.export = {
>       ...data1,//使用es6语法，把对象属性展开给入口文件
>       ...data2
>   }
>   ```
>
> + 在readme文档中写出自己所创建包的内容
>
> + 发布包，把包发布到npm上
>
>   注册npm账号
>
>   登陆npm账号：在终端里面登陆，并不是网站登陆
>
>   ```js
>   输入：npm login
>   //输入相关信息即可登陆
>   ！！！注意，在运行登陆之前一定要把下载包的服务地址切换到npm官方的服务器，否则会出现发布失败
>   ```
>
>   在终端上切换到包的根目录，然后运行
>
>   ```
>   npm publish
>   ```
>
>   就可以把包发布到npm上。（注意：包名不可以雷同）
>
> + 删除已经发布的包
>
> ```
> npm unpublish 包名 --force	
> ```
>
> ```js
> //注意：
> npm unpublish只能删除除 72 小时以内发布的包
> npm unpublish 删除的包，在 24 小时内不允许重复发布
> 避免发布无意义的包
> ```

##### 12.模块的加载机制

> + 模块优先从缓存中加载
>
> ```
> 模块在第一次加载后会被缓存。 这也意味着多次调用 require() 不会导致模块的代码被执行多次。
> 注意：不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率
> ```
>
> + 内置模块加载机制
>
>   内置模块的加载优先级是最高的，即出现外部模块和内置模块重名时，优先加载内置模块
>
> + 自定义模块加载机制
>
> ```
> 使用 require0 加载自定义模块时，必须指定以./ 或 ../ 开头的路径标识符。
> 在加载自定义模块时，如果没有指定上述那样的路径标识符，则 node 会把它当作内置模块或第三方模块进行加载。
> ```
>
> + 导入时省略文件扩展名，自定义模块的加载机制
>
> ```
> 在使用 require0 导入自定义模块时，如果省略了文件的扩展名，则Node.js会按顺序分别尝试加载下列文件：
> ① 按照确切的文件名进行加载
> ② 补全 .js 扩展名进行加载
> ③ 补全 .json 扩展名进行加载
> ④ 补全 .node 扩展名进行加载
> 如果均加载失败，则会在终端中报错
> ```
>
> + 第三方模块加载机制
>
> ```
> 如果传递给 require0 的模块标识符不是一个内置模块，也没有以 './'或者'../'开头，则 Node.js 会从当前模块的父目录开始，尝试从/node_modules文件夹中加载第三方模块。
> 如果没有找到对应的第三方模块，则移动到再上一层的父目录中，进行加载，直到文件系统的根目录。
> 如果最终找不到，就会报错
> ```
>
> + 把目录作为模块加载
>
> ```
> 当把目录作为模块标识符，传递给 require0 进行加载的时候，有三种加载方式：
> ① 在被加载的目录下查找一个叫做 package.json 的文件，并寻找 main 属性，作为 require0 加载的入口
> ② 如果目录里没有 package.json 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的 index.js 文件。
> ③ 如果以上两步都失败了，则 Node.js 会在终端打印错误消息，报告模块的缺失：Error: Cannot find module 'xxx'
> ```

##### 13.初识express

> + 什么是Express
>
>   Express的作用和Node.js内置的http模块类似，是专门用来创建Web服务器的框架（express和http模块的关系参考jq和js）
>
> + Express本质：
>
>   就是一个npm上的包，提供了快速创建Web服务器的便捷方法
>
> + Express可以做什么
>
>   常见的服务器：
>
>   Web网站服务器：专门对外提供网页资源的服务器
>
>   API接口服务器：专门对外提供API接口的服务器
>
>   使用Express可以快速创建上面两种服务器

###### Express基本使用

> + Express的安装
>
> ```js
> //在项目所处的目录中，执行下面命令
> npm i express@4.17.1	//在后面指定版本
> ```
>
> + 创建基本的web服务器
>
> ```js
> //导入express
> const express = require('express')
> //创建web服务器
> const app = express()	//这个返回值就是服务器的实例
> //调用app.listen(端口号，启动成功的回调函数)，启动服务器
> app.listen(80,()=>{
>     console.log('服务器启动成功 http://127.0.0.1')
> })
> ```
>
> + express监听get请求
>
> ```js
> app.get('请求的URl',function(req,res){
>     //参数一是请求的url，参数二是请求完成后的处理
> })
> ```
>
> + express监听post请求
>
> ```js
> app.post('请求的URl',function(req,res){
>     //参数一是请求的url，参数二是请求完成后的处理
> })
> ```
>
> + 把内容响应给客户啊
>
> ```js
> //get请求
> app.get('/user',function(req,res){
>    res.send({name:'zs',age:18})	//在这里用send方法，他既可以响应一个文本字符串，又可以响应一个JSON对象
> })
> -----------------------------------------
> //post请求
> app.post('/user',function(req,res){
>    res.send('请求成功')
> })
> ```
>
> + 获取URL中携带的查询参数
>
> ```js
> //通过req.query可以访问到客户端通过查询字符串的形式，发送到服务器的参数
> app.get('/',function(req,res){
>  	//req.query默认是一个空对象
>     //客户端使用 ?name=zs&age=20 这种查询字符串形式，发送到服务器的参数
>     //可以通过 req.query 对象访问，例如;
>     //req.query.name	req.query.age
>     console.log(req.query)
> })
> ```
>
> + 获取URL中的动态参数
>
> ```js
> //通过req.params对象，可以访问到URL中，通过：匹配到的动态参数
> app.get('/user/:id',function(req,res){//这里的：id是一个动态参数（http://127.0.0.1/user/1）
>  	//req.params默认是一个空对象
>     //里面存放着通过：动态匹配的参数值
>     console.log(res.params)
> })
> ```
>
> 注意：
>
> ```js
> //请求路径中的id不是固定写法，也可以换成其他的名称
> //可以同时匹配多个参数
> app.get('/user/:id/:name',function(req,res){
>     console.log(res.params)
> })
> ```

##### 14.Express托管静态资源

###### express.static()

```js
app.use(express.static('public'))//public是指定的资源存放目录
//注意在该处引用的静态文件目录并不会出现在对外的url中
```

```js
const express = require('express')
const app = express()
app.use(express.static('public'))//该文件夹名字不会出现在访问路径中
app.listen(80,()=>{
    console.log('serve runing')
})
```

> + 拖管多个静态资源目录
>
> 多次调用express.static()即可；注意：会依次从上至下寻找文件夹，找到了就不再寻找
>
> ```js
> app.use(express.static('demo1'))
> app.use(express.static('demo2'))
> app.use(express.static('demo3'))
> ```

> + 挂载路径前缀
>
> 因为express.ststic()中的文件夹名称默认是不显示的，在客户端访问的url中，现在希望他显示，需要如下做法：
>
> ```js
> app.use('/public',express.static('public'))//在前面新增参数，即可在url中增添资源的前缀（这个前缀可以任意写，没必要和资源文件夹重名）
> ```

##### 15.nodemon工具的使用

```
在编写调试 Node.js 项目的时候，如果修改了项目代码，则需要频繁的手动关闭然后启动，非常繁琐。现在，我们可以使用nodemon（https://www.npmjs.com/package/nodemon）这个工具，它能够监听项目文件的变动，当代码被修改后，nodemon 会自动帮我们重启项目，极大方便了开发和调试。
```

> + n安装noddemon
>
> ```js
> npm install -g nodemon
> ```
>
> + 使用nodemon
>
> ```js
> //使用nodemon+项目启动项目名
> nodemon app.js
> //这样就可以在更改项目后自动重启项目
> ```

##### 16.express中的路由

express路由概念：路由就是指客户端请求和服务器处理函数之间的映射关系

express路由组成部分：请求类型，请求url地址，处理函数

```js
app.METHOD(PATH,HANDLER)
```

路由匹配规则：从上到下匹配。客户端的url和请求方式均与服务器的匹配上，才会执行处理函数。

```js
//案例
const express = require('express')
const app = express()
//挂载路由
app.get('/',(req,res)=>{
    res.send('hello')
})
app.listen(80,()=>{
    
})
```

注意：很少使用上述方法挂载路由，而是使用模块化挂载路由

###### 模块化路由

> + 创建路由模块对应的js文件
> + 调用express.Router()创建路由的对象
> + 向路由身上挂载路由
> + 使用model.exports向外共享路由对象
> + 使用app.use()来注册路由模块

```js
//新建的路由模块的js文件
const express = require('express')
const router = express.Router()//在这里调用Router方法
router.get('/main',(req,res)=>{
    res.send('hello')
})
router.post('/user',(req,res)=>{
    res.send('world')
})
module.exports = router
```

```js
//主文件中
const express = require('express')
const useRouter = require('./router/user.js')//这里引入路由文件
const app = express()
app.use(useRouter)//挂载路由
app.listen(80,()=>{
    
})
//注意：app.use()的作用就是用来注册全局中间件
```

> + 为路由模块添加访问路径前缀
>
> ```js
> //主文件中
> const express = require('express')
> const useRouter = require('./router/user.js')
> const app = express()
> app.use('/user',useRouter)//在这里添加统一访问前缀
> app.listen(80,()=>{
> 
> })
> ```

##### 17.express中间件

###### 中间件简介

中间件就是对请求的预处理

<img src="https://yulearn.top/webSource/img/express%E4%B8%AD%E9%97%B4%E4%BB%B6.png" style="zoom:50%;" />



express的中间件，本质上就是一个function处理函数，他的形参中必须包含一个next参数（还有req，res参数）

```js
app.get('/',function(req,res,next){
		next()
})
```

next()函数时实现多个中间件连续调用的关键，他表示把流转关系转交给下一个中间件或者路由

###### 中间件使用

> ```js
> const express = require('express')
> const app = express()
> const mm = function(req,res,next){//在这里定义中间件
>  next()
> }
> app.listen(80,()=>{
> 
> })
> ```

###### 全局生效中间件

在客户端发起的任何请求，到达服务器后都会触发的中间件，叫做全局中间件。

通过调用app.use(中间件函数)，即可定义一个全局中间件

```js
const express = require('express')
const app = express()
const mm = function(req,res,next){//在这里定义中间件，当服务器被请求以后，中间件中的函数就会被调用
   console.log("我是中间件，我被调用了")
    next()
}
app.use(mm)//注册为全局生效中间件
app.get('/',(req,res)=>{
    //中间件执行完毕后，就执行这个路由
})
app.listen(80,()=>{
    
})
```

精简写法：

```js
const express = require('express')
const app = express()
app.use(function(req,res,next){
    next()
})
```

定义多个全局中间件

```js
const express = require('express')
const app = express()
app.use(function(req,res,next){
    next()
})
app.use((req,res,next)=>{
    next()
})
app.get('/user',(req,res)=>{
    res.send("我是返回的数据")
})
app.listen(80,()=>{
    console.log(http://127.0.0.1)
})
```

###### 中间件的作用

```
多个中间件之间，共享同一份 req 和 res。基于这样的特性，我们可以在上游的中间件中，统一为 req 或 res 对象添加自定义的属性或方法，供下游的中间件或路由进行使用。
```

```js
app.use((req,res,next)=>{
    const time = Data.now()//这里把事件属性传递给下面的路由
    req.time = time
    next()
})
app.get('/',(req,res)=>{
    res.send(req.time)
})
```

###### 局部生效的中间件

不使用app.use()生成的中间件叫做局部生效中间件

```js
const express = require('express')
const app = express()
const mm = (req,res,next)=>{//在这里承接中间件的名字
    next()
}
app.get('/user',mm,(req,res)=>{//在这里添加中间件名字参数
    res.send('hello world')
})
app.post('/',(req,res)=>{
    res.send('hello')
})
app.listen(80,()=>{
    
})
```

定义多个局部中间件

```js
const m1 = (req,res,next)=>{
    next()
}
const m2 = (req,res,next)=>{
    next()
}
const m3 = (req,res,next)=>{
    next()
}
app.get('/user',[m1,m2,m3],(req,res)=>{//这里写数组可以，直接写也可以
    res.send()
})
```

###### 中间件注意事项

```js
//一定要再路由之前注册中间件
//客户端发过来的请求，可以连续调用多个中间件
//不要忘记调用next()方法
//只要调用完next函数后，不要再写额外的代码，避免混乱
//多个中间件之间公用一套req和res
```

##### 18.中间件的分类

注意：除了错误级别中间件，其他的中间件必须在路由中间件之前配置

> + 应用级别的中间件
>
>   通过app.use()/get()/post()，绑定到app实例上的中间件，就叫应用级别的中间件
>
>   ```js
>   //全局中间件
>   app.use((req,res,next)=>{
>       const time = Data.now()//这里把事件属性传递给下面的路由
>       req.time = time
>       next()
>   })
>   //局部中间件
>   app.get('/user',[m1,m2,m3],(req,res)=>{//这里写数组可以，直接写也可以
>       res.send()
>   })
>   ```
>
> + 路由级别的中间件
>
>   帮定到express.Router()实例上的中间件
>
>   ```js
>   const router = express.Router()
>   router.use('/user',(req,res,next)=>{
>       next()
>   })
>   ```
>
> + 错误级别的中间件(app.use注册)
>
>   作用：专门用来捕获项目中发生异常的错误，从而防止项目异常崩溃
>
>   错误级别中间件的函数中，必须有四个参数（err,req,res,next）
>
>   ```js
>   app.get('/',(req,res)=>{
>       throw new Error('服务器发生错误')//人为制造错误，并抛出
>       res.send('Home')
>   })
>   app.use((err,req,res,next)=>{//错误级别中间件
>       console.log("服务器发生了错误"+err.message)//在服务器打印错误信息
>       res.send('Error:'+err.message)//向客户端响应内容
>   })
>   ```
>
>   注意：错误级别中间件，必须注册在所有的路由中间件之后
>
> + Express内置中间件
>
>   在express4.16.0以后，express内置了三个好用的中间件
>
>   ```js
>   //express.static	快速托管静态资源文件，无兼容性问题
>   //express.json	解析JSON格式的请求数据，有兼容性问题，仅在4.16.0以上的版本才可以使用
>   //express.urlencoded	解析URL-encoded格式的请求数据的，有兼容性问题，仅在4.16.0以上版本可以使用(键值对数据)
>   ```
>
>   ```js
>   //express.json()的用法
>   const express = require('express')
>   const app = express()
>   //在路由中间件之前配置json解析中间件
>   app.use(express.json())//通过这个中间件来解析表单中的json数据
>   app.post('/user',(req,res)=>{
>       //在服务器可以使用 req.body 来接收客户端发送的json数据
>       //但是如果不对JSON数据进行解析（使用中间件解析），则默认这个req.body 是undefined
>       console.log(req.body)
>       res.rend('Home')
>   })
>   app.listen(80,()=>{
>       console.log("serving run at http://127.0.0.1")
>   })
>   ```
>
>   ```js
>   //express.urlencoded()用法
>   const express = require('express')
>   const app = express()
>   app.use(express.urlencoded({extended:false}))//在这里进行配置,固定写法
>   app.post('/user',(req,res)=>{
>       //如果不配置express.urlencoded，则req.body默认是一个空对象
>       console.log(req.body)
>       res.send('ok')
>   })
>   app.listen(80,()=>{
>        console.log("serving run at http://127.0.0.1")
>   })
>   ```
>
> + 第三方中间件
>
>   第三方开发的中间件
>
>   例如：在express@4.16.0之前的开发版本中，经常使用body-parser这个第三方的中间件，来解析客户端发来的数据。使用步骤如下：
>
>   ```js
>   //运行 npm install body-parser安装
>   //使用require导入
>   //使用app.use使用中间件
>   const parser = require('body-parseer')
>   app.use(parser.urlencoded({extended:false}))
>   app.post('/user',(req,res)=>{
>       console.log(req.body)
>       res.send('ok')
>   })
>   ```
>
> + 自定义中间件
>
>   ```js
>   //实现一个对客户端传来的字符串格式化的中间件
>   const express = require('express')
>   const app = express()
>   const qs = require('querstring')
>   
>   //定义中间件
>   app.use((req,res,next)=>{
>   //监听req的data事件 
>   let str = ''
>   req.on('data',(chunk)=>{//使用on来监听事件
>       str += chunk	//进行拼接字符串
>   	})
>   //监听req的end事件
>   req.on('end',()=>{
>   //把字符串解析成对象(使用node内置的querystring模块)
>       const body = qs.parse(str)
>       req.body = body
>       next()
>   	})
>   })
>   app.get('/user'(req,res)=>{
>       res.send("请求成功")
>   })
>   app.listen(80,()=>{
>       console.log('serve runing at http://127.0.0.1')
>   })
>   ```
>
>   把中间件封装成一个模块
>
>   ```js
>   //这是新建的模块js文件
>   const qs = require('querystring')
>   function bodyParser(ewq,res,next){
>       //逻辑代码
>   }
>   module.exports = bodyParser
>   ------------------------------------
>   //这是主文件
>   const bodyParser = require('bodyParser')
>   app.use(bodyParser)
>   ```

##### 19.使用express写接口

###### 编写get接口

> + 创建路由模块apiRouter文件

```js
const express = require('express')
const router = express.Router()
//在这里挂载路由
router.get('/get',(req,res)=>{
    const query = req.query//拿到客户端发送的数据
res.send({
    status:0,	//0表示处理成功，1表示处理失败
	msg:"get响应成功",
    data:query //返回客户端发送的数据
})
    })
module.exports = router
```



```js
const express = require('express')
const app = express()
//引入路由模块
const router =  require('./apiRouter')
//挂载路由模块,并指定访问路径
app.use('/api',router)
app.listen(80,()=>{
    
})
```

###### 编写post接口

> + 创建接口路由模块

```js
const express = require('express')
const router = express.Router()
//在这里挂载路由
router.post('/post',(req,res)=>{
    const body = req.body//拿到客户端发送的数据
res.send({
    status:0,	//0表示处理成功，1表示处理失败
	msg:"post响应成功",
    data:body //返回客户端发送的数据
})
    })
module.exports = router
```

```js
const express = require('express')
const app = express()
const router =  require('./apiRouter')
//使用中间件来解析，客户端发送的数据
app.use(express.urlencoded({extended:false}))
app.use('/api',router)
app.listen(80,()=>{
    
})
```

###### 解决跨域问题

###### cors方案（主流）

​		使用express的cors中间件，结觉跨域问题步骤

> + 运行: npm install cors 安装中间件
> + 使用 const cors = require('cors') 导入中间件
> + 在路由之前调用 app.use(cors())来配置中间件
>
> ```js
> const express = require('express')
> const app = express()
> const router =  require('./apiRouter')
> const cors = require('cors')//引入跨域中间件
> app.use(express.urlencoded({extended:false}))
> //挂载中间件
> app.use(cors())
> app.use('/api',router)
> app.listen(80,()=>{
> 
> })
> ```
>
> 注意：cors只在服务器端中配置；cors存在兼容性问题，只有支持xmlhttprequestlevel2的浏览器，才支持

CORS响应头部：

Access-Control-Allow-Origin

```js
res.setHeader('Access-Control-Allow-Origin','*')//第二个参数是配置域名白名单（不发生跨域的域名）
```

Access-Control-Allow-Headers

```js
//默认cors仅仅支持客户端向服务器发送九种请求头，但是如果客户端向服务器发送了额外的请求头信息，则需要在服务器端 通过该属性进行配置声明，否则会发生错误
res.setHeader('Access-Control-Allow-Headers','Content-Type,X-Custom-Header')
```

Access-Control-Allow-Methods

```js
//默认情况下，cors仅支持客户端发起的GET，POST，HEAD请求。
//如果客户端希望通过PUL，DELETE等方式请求服务器的资源，则需要在服务端，透过该属性进行配置
res.Header('Access-Control-Allow-Methods','*')//这里是允许所有的请求方式，也可以通过指定具体请求名称来配置
```

请求头分类：

> + 简单请求

<img src="https://yulearn.top/webSource/img/%E8%AF%B7%E6%B1%82%E5%A4%B4%E4%BF%A1%E6%81%AF.png" style="zoom:50%;" />

> + 预检请求（与服务器之间发生两次请求）

<img src="https://yulearn.top/webSource/img/%E9%A2%84%E6%A3%80%E8%AF%B7%E6%B1%82.png" style="zoom: 50%;" />

###### jsonp方案 (不支持post请求)

概念：浏览器通过<script>标签的src属性，请求服务器上的数据，同时，服务器返回一个函数的调用。这种请求方式叫做jsonp.

特点：jsonp不是真正的Ajax请求，他只支持get方法。

注意事项：如果在项目中已经开启了cors跨域资源共享，为了防止冲突，必须在配置cors中间件之前声明jsonp接口，否则jsonp接口将会被处理成开启了cors接口

```js
//在配置cors中间件之前配置
app.get('/api/jsonp',(req,res)=>{//在这里手动声明路径，不属于cors了
    
})
app.use(cors())
```

> + 实现jsonp的步骤

1.获取客户端发过来的回调函数名字

2.得到要通过jsonp形式发送给客户端的数据

3.根据前两步得到的数据，拼出一个回调函数的字符串

4.把上一步的拼接得到的字符串，响应给客户端的<script>标签进行解析

```js
app.get('/api/jsonp',(req,res)=>{
    const funName = req.query.callback//获取客户端发送的函数名称
    const data = {name:'Tom',age:23}//定义要发送给客户端的对象
    const scriptStr = `${funName}(${JSON.stringify(data)})`//拼接出一个函数
    res.send(scriptStr)//把拼接出的字符串发送给客户端
})
```

##### 20.数据库

> + 相关概念

数据库的基本概念：数据库（database）是用来组织，存储和管理数据的仓库

常见数据库及其分类：

MySQL（社区版免费），Qracle（收费），SQL Server（收费），MongoDB（社区版免费）

前面三个是关系型数据库（传统型数据库），后者是新型数据库（非关系型数据库）。两种数据库是相互弥补的关系。

> + 传统型数据库的数据组织结构

（与Excel类似）

数据库，数据表，数据行（行），字段（列）每一个字段都有数据类型

> + 库，表，行，字段的关系

1.不同项目对应不同的数据库

2.不同数据要存放到不同的数据表中，例如user和books

3.表中存储那些信息，由字段来决定，例如：可以为user表设计，id，name，password这三个字段（数据类型）

4.表中的行代表每一条数据

##### 21.安装配置MySQL

安装软件：

MySQL Server：专门用来提供数据存储和服务的软件

MySQL Workbench：可视化的MySQL管理工具，通过他可以方便的操作存储在MySQL Server中的数据

###### 创建数据表

> + DataType数据类型
>
>   int 整数
>
>   varchar(len)字符串
>
>   tinyint(1)布尔值

> + 字段的特殊标识
>
>   PK （primary key）主键，唯一 标识
>
>   NN（not null）值不允许为空
>
>   UQ（unique）值唯一
>
>   AI（auto increment）值自动增长

<img src="https://yulearn.top/webSource/img/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A1%A8%E8%AE%BE%E8%AE%A1.png" style="zoom: 50%;" />

> + 向表中写数据
>
>   右键想插入数据的表，选择select rows - limit 1000，便可以填写。（有默认值，或者自增的值不用手动写），添加完成后，点击apply即可完成写入。

##### 22.SQL

> + sql概念：
>
>   SQL本身就是一门结构化查询语言，是专门用来处理和访问数据库的语言。可以让我们以编程的形式，操作数据库里面的数据。

​	注意：SQL只能在关系型数据库当中使用。非关系型数据库不支持SQL语言。

> + SQL的用途
>
>   从数据库当中查询数据，向数据库插入数据，更新旧数据，从数据库中删除数据，创建新数据库等操作。

##### 23.SQL语句

> + SELECT语句
>
>   用于从表中查询数据，执行的结果存在一个表中（称为结果集）
>
>   ```sql
>   SELECT * FROM 表名称 -- 查询所有的列
>   SELECT 列名称 FROM 表名称 -- 列名称就是表头
>   -- 注意：SQL语句对大小写不敏感，大小写都等效
>   select password from users
>   ```

> + INSERT INTO语句
>
>   用于向数据表中插入新的数据行
>
>   ```sql
>   INSERT INTO 表名 (列1，列2...） VALUES (值1，值2...)
>   -- 列和值要一一对应
>   insert into users (username,password) value ('tony','098123')
>   ```

> + UPDATE语句
>
>   用于修改表中的数据
>
>   ```sql
>   UPDATE 表名 SET 列名称 = 新值 WHERE 列名称 = 某值 
>   -- 第一组列名称应该写想要更改的属性值，和他的新value值。第二组列名称指的是需要更改数据的条件（id唯一标识）
>   update users set password = '111111' where id = 2
>   -- 更新一行中的若干列，可以使用‘，’来进行分隔
>   update users set password = '111111',status = 1 where id = 2
>   ```

> + DELETE语句
>
>   用于删除表中的行
>
>   ```sql
>   DELETE FROM 表名 WHERE 列名 = 值
>   -- 这里的列名指的是唯一标识
>   delete from users where id = 3
>   ```

注意：一定要加上WHERE条件

> + where子句
>
>   用于限定选择条件，在select，update，delete语句中均可使用WHERE子句来限定执行条件
>
> + 可以在WHERE中使用的运算符
>
>   ```sql
>   等于(=)，大于(>)，小于(<)，不等于(<>或者!=)，大于等于(>=)，小于等于(<=)，在某个范围内(BETWEEN)，搜索某种模式(LIKE)
>   select * from users where status = 1
>   select * from users where id>2
>   select * from users where username<>'ls'
>   select * from users where uername!='zs'
>   ```

> + sql中的AND和OR运算符
>
>   and表示必须同时满足多个条件（相当于js中的&&）
>
>   or表示只要满足一个条件即可（相当于js中的||）
>
>   ```sql
>   select * from users where status = 0 and id < 3
>   select * from users where status = 1 or username = 'ls'
>   ```

> + sql中的ORDER BY 语句
>
>   order by 用于根据指定的列来对结果进行排序
>
>   默认情况下，会进行升序排列（asc关键字）。如果想降序，可以使用desc关键字
>
>   ```sql
>   select * from users order by status -- 表示按照status字段进行升序
>   select * from users order by id desc -- 表示按照id来降序
>   ```

> + order by 子句进行多重排序（使用逗号来分隔）
>
>   ```sql
>   select * from users order by status desc,username asc -- 这里表示先按照status降序，再根据username进行升序（asc不加也可以）
>   ```

> + sql中的COUNT(*)函数
>
>   用来查询结构的总数据条数
>
>   ```sql
>   -- select count(*) from 表名
>   select count(*) from users
>   select count(*) from users where status = 0 -- 查询指定状态的数据条数
>   ```
>
> + 使用AS关键字来为查询出来的结果设置个别名
>
>   ```sql
>   select count(*) as total from users where status = 0 -- 这里就使用as设置了一个total别名
>   ```
>
> + 还可以使用as关键字，来为列起别名
>
>   ```sql
>   select pasword as upwd from users
>   ```

##### 24.在项目中操作数据库

> + 安装第三方模块（mysql）
>
>   ```js
>   npm install mysql
>   ```
>
> + 通过mysql连接到MySQL数据库
>
>   ```js
>   const mysql = require('mysql')//导入数据库模块
>   const db = mysql.createPool({//创建和数据库的连接
>       host:127.0.0.1,//数据库的ip地址
>       user:'root',//登陆数据库的账号
>       password:'admin123'//登陆数据库的密码
>       database:'my_db_01'//指定要操作哪个数据库
>   })
>   ```
>
> + 通过mysql模块来执行SQL语句 
>
>   ```js
>   db.query('select 1',(err,results)=>{//第一个参数是执行的SQL语句，第二参数个是拿到执行结果的函数
>       if(err) return console.log(err.message)
>       console.log(results)
>   })
>   ```

###### 使用mysql来实现查询数据

select语句执行成功会返回一个数组

```js
const mysql = require('mysql')
const db = masql.createPool({
    host:127.0.0.1,
    user:'root',
    password:'admin123',
    database:'my_db_01'
})
const str = 'select * from users'//查询user表中的所有数据
db.query(str,(err,results)=>{
    if(err) return console.log(err.message)
    console.log(results)//如果成功，则会打印出一个数组
})
```

###### 使用mysql插入数据

（使用？作为占位符）

insert into执行成功后会返回一个对象，可以通过该对象的affectedRows属性来判断是否插入成功（为1就成功）

```js
const mysql = require('mysql')
const db = masql.createPool({
    host:127.0.0.1,
    user:'root',
    password:'admin123',
    database:'my_db_01'
})
const user = {username:'demo1',password:'123456'}//定义一个要插入数据库中的对象
const str = 'insert into users (username,password) value (?,?)'//定义待执行的SQL语句，使用？来作为占位符
db.query(str,[user.username,user.password],(err,results)=>{//使用数组的形式，依次为？占位符所指定的位置
    if(err) return console.log(err.message)
    if(results.affectedRows === 1){//根据affectedRows属性看插入数据是否成功
        console.log('插入数据成功')
    }
})
```

> + 插入数据的便捷写法
>
>   ```js
>   //在向表中新增数据时，如果数据的对象的每个属性和数据表的字段一一对应，则可以通过以下方式快速插入数据
>   const user = {username:'demo1',password:'123345'}//该对象中的属性名和数据库中字段一样
>   const str = 'insert into user set ?'//这里使用set方法，并用？占位
>   db.query(str,user,(err,results)=>{//这里把上面定义的对象作为第二个参数
>       if(err) return console.log(err.message)
>       if(results.affectedRows === 1){
>           console.log('插入数据成功')
>       }
>   })
>   ```

###### 使用mysql更新数据

update执行成功后会返回一个对象，可以通过该对象的affectedRows属性来判断是否插入成功（为1就成功）

```js
const user = {id:7,username:'aaa',password:‘000}//定义要更新的对象
const str = 'update users set username=?,password=? where id=?'//定义要执行的SQL语句
db.query(str,[user.username,user.password,user.id],(err,results)=>{//使用数组来承接上面需要更新的数据
    if(err) return console.log(err.message)
    if(results.affectedRows === 1){
        console.log('更新数据成功')
    }
})
```

> + 更新数据的便捷写法
>
>   ```js
>   //更新数据表时，如果数据的对象的每个属性和数据表的字段一一对应，则可以通过以下方式快速插入数据
>   const user = {id:7,username:'aaa',password:‘000}
>   const str = 'update users set ? where id=?'//定义要执行的SQL语句
>   db.query(str,[user,user.id],(err,results)=>{//使用数组来承接上面需要更新的数据
>       if(err) return console.log(err.message)
>       if(results.affectedRows === 1){
>           console.log('更新数据成功')
>       }
>   })
>   ```

###### 使用mysql删除数据

推荐使用id这样的唯一标识，来进行数据删除。

delete执行成功后会返回一个对象，可以通过该对象的affectedRows属性来判断是否插入成功（为1就成功）

```js
const str = 'delete from users where id = ?'
db.query(str,7,(err,results)=>{
    if(err) return console.log(err.message)
    if(results.affectedRows === 1){
        console.log('删除数据成功')
    }
})
```

###### 标记语句删除

使用delete语句会真正的把数据从数据库当中删除。为了保险起见，推荐使用标记删除的形式，来模拟删除动作

所谓的标记删除，就是在表中设置了类似status这样的字段，用来标记这条数据是否被删除。当用户执行了删除动作以后，我们并没有执行delete语句，而是执行了update，把这条数据的status字段标记为删除即可。

```js
const str = 'update users set status = 1 where id = ?'
db.query(str,[6],(err,results)=>{
       if(err) return console.log(err.message)
    if(results.affectedRows === 1){
        console.log('标记删除成功')
    }
})
```

##### 25.Web开发模式

###### 服务端渲染的web开发模式

服务端渲染：服务器发送给客户端的html界面，是在服务器通过字符串拼接，动态生成的。因此客户端不需要使用Ajax这样的技术额外请求页面数据

###### 前后端分离的web开发模式

后端负责提供api接口，前端利用Ajax调用数据。

###### 如何选择开发模式

根据把不同的业务场景来选择开发模式。

##### 26.身份认证

身份认证：通过一定手段来完成对用户身份的确认。

###### 对于服务器端渲染推荐使用Session认证机制

> + http协议的无状态性
>
>   客户端每次的http请求都是独立的，服务器不会主动地去记录http请求。
>
> + 突破http无状态限制
>
> 

###### 对于前后端分离推荐使用JWT认证机制

