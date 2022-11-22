**npm常用的命令：**

1. 安装包（如果没有指定版本号，那么安装最新版本）

　　`npm install -g 包名称（全局安装）`

　　`npm install包名称（本地安装）`

2. 安装包的时候可以指定版本

　　`npm install -g 包名称@版本号`

3. 卸载包

　　`npm uninstall -g 包名称`

4. 更新包（更新到最新版本）

　　`npm update -g 包名称`

5. 查看全局模块列表

   `npm list -g`

6. 查看包所依赖的Node的版本 

   ` npm view 包名称 engines`

7. 卸载node模块

   `npm uninstall 包名称`

8. 更新node模块

   `npm uninstall 包名称`

9. 查看包源文件地址

   `npm view 包名称 repository.url`

开发环境（平时开发使用的环境）

生产环境（项目部署上线之后的服务器环境）

--save 向生产环境添加依赖 dependencies

--save-dev 向开发环境添加依赖 devDependencies

***区别：***

npm install xxx 安装到项目目录的node_modules文件夹下，但是不会添加任何依赖（依赖写在package.json文件中）

npm install xxx -g 全局安装，安装到你安装npm的磁盘目录下

npm install xxx --save 安装到项目目录的node_modules文件夹下，添加到dependencies的依赖下（生产环境中：项目发布到服务器上必须要使用的包）

npm install xxx --save-dev 安装到项目目录的node_modules文件夹下，添加到devdependencies的依赖下（开发环境中：测试、管理，只在程序员模式下需要）

