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
  - name :包名称
  - version：包的版本
    - 版本号：
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
