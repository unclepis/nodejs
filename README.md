# nodejs
basic concept for nodejs

## npm
- npm是随着nodejs一起安装的node的包管理工具，能解决nodejs代码部署上的很多问题：
  - 允许用户从npm服务器下载别人的第三方包到本地使用
  - 允许用户从npm服务器下载别人的命令行程序到本地使用
  - 允许用户将自己编写的包或者命令上传到npm服务器上
- 检测node路径和版本
  - which node
  - node -v
- 对安装的旧版本的npm进行升级
  - npm install npm 
- 模块的安装
  - npm install xxx
- 本地安装和全局安装
  - npm i xxx //local
    - 本地安装就是把包放在../node_modules下，如果没有这个文件，就绪在执行npm命令的时候自动生成一个；
    - 通过使用require()来引入本地安装的包到目标文件，否则找不到
  - npm i xxx -g // global
    - 全局安装放在一个目录下，可以使用 npm config get prefix来查看全局路径的prefix路径
    - 可以直接在命令行里面使用
- 查看安装信息
  - npm list -g 查看所有在全局安装的模块
### package.json
- 可以通过npm init命令对package.json文件进行初始化
- node_modules中加载的包里面每一个都有一个package.json文件，用于定义包的属性
- 属性说明:
  - name:包名称
  - version:包的版本
    - 版本号:
    npm使用语义版本号来管理代码，简单的说语义版本号分为X,Y,Z三位，分别代表主版本号，次版本号和补丁版本号，当代码变更时，就会按照一下原则进行更新：
    1.如果只是更新了bug，需要更新z位置；
    2.如果时新增了功能，但是向下兼容，需要更新y位置；
    3.如果有大变动，向下不兼容，需要更新x位；
  - description:包的描述
  - homepage:包的官网url
  - author:包的作者姓名
  - contributors:包的贡献者名称
  - dependencies:依赖包的列表，如果依赖包没有安装，npm会自动把他们安装在node_modules目录下
  - repository:包代码存放的地方的类型
  - main:指定了程序的主入口文件，require('moduleName')就会加载这个文件,默认
  是根目录下的index.js
  - keywords: 关键字
### 模块卸载
- npm uninstall xxx
- 卸载后查看包是否存在：
  - npm ls
### 更新模块：
- npm update xxx
### 搜索模块
- npm search xxx
### 创建模块
1. npm init 生成package.json文件管理包的属性
2. npm adduser 在npm资源库中注册用户
3. npm publish 发布模块/npm unpublish xxx@version可以撤销发布的版本
## nodejs  REPL交互式解释器
- Node.js REPL(Read Eval Print Loop:交互式解释器) 表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。
- node自带交互解释器，可以执行：
 1.读取用户输入：解析输入了Javascript 数据结构并存储在内存中。
 2.执行输入的数据结构
 3.打印输出结构
 4.循环操作以上步骤知道用户点击两次ctrl+c退出
- 开启node的终端输入node就可以像在浏览器的控制台一样进行输入了
### nodejs异步编程之回调函数
- nodejs异步编程的直接体现就是回调，但是不能说回调后程序就异步化了
- 回调函数在完成任务之后就被调用，node使用了大量的回调函数，node所有的api都支持回调函数
- 这样在执行代码时就没有阻塞或等待文件i/o操作，极大提高了nodejs的性能，可以处理大量的并发请求。
 ~~~
  // demo.txt
  'This is a test'
  // demo.js
  var fs = require('fs');
  var data = fs.readFile('demo.txt',function(err,res){
    if(err)console.log(err);
    else console.log(res.toString());
  });
  console.log('finish to read the file');
  // shell 
  node main.js
  输出结果：先输出 ‘finish to read the file’
          在输出 ‘This is a test’
  说明非阻塞调用，不需要等待文件读取完，就这样就可以在读取文件时同时执行接下来的代码，大大提高了程序的性能
 ~~~
 ## npm脚本
 ### npm允许在package.json文件里面，使用srcipts字段定义脚本命令。
 - 命令行下使用npm run xxx就可以执行在srcipts字段里面定义好的脚本
 - 这些定义在package.json文件中的脚本就叫做npm脚本
 - 优点: 1.项目的相关脚本可以放在一个地方；2.不同项目的脚本命令，只要功能相同，就可以有相同的对外接口，用户不需要制动怎么测试你的项目，只需要在想要执行测试的时候运行npm run test；3.可以利用npm提供的很多辅助功能
 - 可以使用npm run查看所有的npm脚本命令

 ### 原理
 - 每当npm run运行，就会新建一个shell，在这个shell里面执行指定的脚本命令
 - npm run新建的这个shell，会将当前目录的node_modules/.bin子目录加入PATH变量，执行结束后，再将PATH变量恢复原样。这意味着，当前目录的node_modules/.bin子目录里面的所有脚本，都可以直接用脚本的名称调用，不需要加上路径。
 - 所以例如当前的项目的依赖里面有Mocha，你就可以在package.json文件的npm脚本中定义npm run test 执行 ‘mocha test’，而不用加载文件的路径‘./node_modules/.bin/mocha’
 - 另外备注一下：./是指根目录，../指上层目录
 
 ### 通配符
 - ‘test’: '*/js' *表示任意文件名
 - 'test':'**/*.js' **表示任意一层子目录
 - ‘test’: 'test/\*.js' 如果要将通配符传入原始命令，防止被shell转义，需要将*转义
 
 ### 传参
 - 向npm传入参数，需要使用 -- 标明
 
 ### 执行顺序
 - 如果npm脚本里面需要执行多个任务，那么需要明确他们的顺序
 - 同时并行执行使用&: npm run test1 & npm run test2
 - 上一个执行完成成功，才执行下一个任务的继发执行使用&& : npm run test1 && npm run test2
 
 ### 默认值
 - 一般来说，npm脚本由用户提供，但是npm对两个脚本提供了默认值，也就是说这两个脚本不用定义,就可以直接使用：
 
 ~~~
 "start":"node server.js" //如果项目根目录下有server.js这个脚本 
 "install":"node-gyp rebuild" // 如果项目下有binding.gyp文件
 ~~~
