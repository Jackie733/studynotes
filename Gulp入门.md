# Gulp

1. 安装nodejs

2. 全局安装Gulp

   1. 安装：命令提示符执行npm install gulp -g
   2. 查看是否正确安装：命令提示符执行gulp -v ,出现版本号即为正确安装

3. 新建package.json文件

   命令提示符执行npm init(注意：json文件内是不能写注释的，复制下列内容情删除注释)

   ```json
   {
     "name": "test",   //项目名称（必须）
     "version": "1.0.0",   //项目版本（必须）
     "description": "This is for study gulp project !",   //项目描述（必须）
     "homepage": "",   //项目主页
     "repository": {    //项目资源库
       "type": "git",
       "url": "https://git.oschina.net/xxxx"
     },
     "author": {    //项目作者信息
       "name": "surging",
       "email": "surging2@qq.com"
     },
     "license": "ISC",    //项目许可协议
     "devDependencies": {    //项目依赖的插件
       "gulp": "^3.8.11",
       "gulp-less": "^3.0.0"
     }
   }
   ```

4. 本地安装Gulp插件

   1. 定位目录命令后提示符执行npm install --save-dev(例如安装gulp-less为例，命令提示符执行npm install gulp-less --save-dev)
   2. 将会安装在node_modules的gulp-less目录下，该目录下有一个gulp-less的使用帮助文档README.md
   3. 为了能正常使用，我们还得本地安装gulp：npm install gulp --save-dev

5. 新建gulpfile.js文件（重要）

   1. gulpfile.js是gulp项目的配置文件，是位于项目根目录的普通js文件（其实将gulpfile.js放入其他文件夹下亦可）。
   2. 它大概是这样一个js文件

   ```javascript
   //导入工具包 require('node_modules里对应模块')
   var gulp = require('gulp'), //本地安装gulp所用到的地方
       less = require('gulp-less');
    
   //定义一个testLess任务（自定义任务名称）
   gulp.task('testLess', function () {
       gulp.src('src/less/index.less') //该任务针对的文件
           .pipe(less()) //该任务调用的模块
           .pipe(gulp.dest('src/css')); //将会在src/css下生成index.css
   });
    
   gulp.task('default',['testLess', 'elseTask']); //定义默认任务 elseTask为其他任务，该示例没有定义elseTask任务
    
   //gulp.task(name[, deps], fn) 定义任务  name：任务名称 deps：依赖任务名称 fn：回调函数
   //gulp.src(globs[, options]) 执行任务处理的文件  globs：处理的文件路径(字符串或者字符串数组) 
   //gulp.dest(path[, options]) 处理完后文件生成路径
   ```

   ​

6. 运行gulp

   命令提示符执行**gulp 任务名称**

