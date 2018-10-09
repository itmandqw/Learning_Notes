# gulp

gulp使用技巧:(<https://www.cnblogs.com/2050/p/4198792.html>)

<https://www.cnblogs.com/Tom-yi/p/8036730.html>

使用gulp，仅需知道4个API即可：`gulp.task()`,`gulp.src()`,`gulp.dest()`,`gulp.watch()`

## 1.gulp.src()

在Gulp中，使用的是Nodejs中的[stream](http://nodejs.org/api/stream.html)(流)，首先获取到需要的stream，然后可以通过stream的`pipe()`方法把流导入到你想要的地方，比如Gulp的插件中，经过插件处理后的流又可以继续导入到其他插件中，当然也可以把流写入到文件中。所以Gulp是以stream为媒介的，它不需要频繁的生成临时文件，这也是Gulp的速度比Grunt快的一个原因。再回到正题上来，`gulp.src()`方法正是用来获取流的，但要注意这个流里的内容不是原始的文件流，而是一个虚拟文件对象流([Vinyl files](https://github.com/wearefractal/vinyl-fs))，这个虚拟文件对象中存储着原始文件的路径、文件名、内容等信息，这个我们暂时不用去深入理解，你只需简单的理解可以用这个方法来读取你需要操作的文件就行了。其语法为：

```javascript
gulp.src(globs[, options])
globs 参数是文件匹配模式(类似于正则表达式),用来匹配文件路径（包括文件名），当然这里也可以直接指定某个具体的文件路径。当有多个匹配模式时，该参数可以为一个数组。
options 为可选参数，通常情况下用不到。
匹配技巧见：    https://www.cnblogs.com/2050/p/4198792.html
```

## 2.gulp.dest()

`gulp.dest()方法是用来写文件的，其语法为：`

```javascript
gulp.dest(path[, options])
path为写入文件的路径
options为一个可选的参数对象，通常也用不到.
```

gulp的使用流程一般是这样子的：首先通过`gulp.src()`方法获取到我们想要处理的文件流，然后把文件流通过pipe方法导入到gulp的插件中，最后把经过插件处理后的流再通过pipe方法导入到`gulp.dest()`中，`gulp.dest()`方法则把流中的内容写入到文件中，这里首先需要弄清楚的一点是，我们给`gulp.dest()`传入的路径参数，只能用来指定要生成的文件的目录，而不能指定生成文件的文件名，它生成文件的文件名使用的是导入到它的文件流自身的文件名，所以**生成的文件名是由导入到它的文件流决定的**，即使我们给它传入一个带有文件名的路径参数，然后它也会把这个文件名当做是目录名，例如：

```javascript
var gulp = require('gulp');
gulp.src('script/jquery.js')
    .pipe(gulp.dest('dist/foo.js'));
//最终生成的文件路径为 dist/foo.js/jquery.js,而不是dist/foo.js
```

```javascript
gulp.src(script/lib/*.js) //没有配置base参数，此时默认的base路径为script/lib
    //假设匹配到的文件为script/lib/jquery.js
    .pipe(gulp.dest('build')) //生成的文件路径为 build/jquery.js

gulp.src(script/lib/*.js, {base:'script'}) //配置了base参数，此时base路径为script
    //假设匹配到的文件为script/lib/jquery.js
    .pipe(gulp.dest('build')) //此时生成的文件路径为 build/lib/jquery.js    
```

## 3.gulp.task()

`gulp.task`方法用来定义任务，内部使用的是[Orchestrator](https://github.com/robrich/orchestrator)，其语法为：

```javascript
gulp.task(name, [, deps], fn)
name 为任务名
deps 是当前定义的任务所依赖的其他任务，是一个数组。当前定义的任务会在所有的依赖任务执行完毕后才开始执行。如果没有依赖，则可省略这个参数。
fn 为任务函数，我们把任务要执行的代码写在里面。该参数为可选


```

## 4.gulp.watch()

`gulp.watch()`用来监视文件的变化，当文件发生变化后，我们可以利用它来执行相应的任务，例如压缩文件等，其语法为：

```javascript
gulp.watch(glob[, opts], tasks)
glob 为要监视的文件匹配模式，规则和用法与gulp.src()方法中的glob相同
opts 为一个可选的配置对象， 通常不需要用到。
tasks 为文件变化后要执行的任务，为一个数组。
eg: 
gulp.task("uglify",function(){
  //do something;
})
gulp.watch("js/**/*.js", ['uglify']);
```

# 利用gulp处理代码的代码

## 1,htmlmin压缩HTML

```javascript
var htmlmin = require('gulp-htmlmin');
gulp.task('html',function(){
    gulp.src('*.html')
    .pipe(htmlmin({
        collapseWhitespace : true,
        removeComments : true
    }))
    //最后把你建立的html文件压缩到自动创建的dist文件里;
    .pipe(gulp.dest('dist'))
})
```

## 2,如果使用sass变异css,gulp预处理sass

```javascript
var scss = require('gulp-sass');
var cssnano = require('gulp-cssnano');
//因为我用的是scss,所以这里注册任务写成了scss;
gulp.task('scss',function(){
    gulp.src('*.scss')
    .pipe(scss())
    .pipe(gulp.dest("dist"))
    .pipe(cssnano())
    .pipe(gulp.dest('dist/css'))
});
```

## 3.图片处理

```
var scss = require('gulp-sass');
var cssnano = require('gulp-cssnano');
//因为我用的是scss,所以这里注册任务写成了scss;
gulp.task('scss',function(){
    gulp.src('*.scss')
    .pipe(scss())
    .pipe(gulp.dest("dist"))
    .pipe(cssnano())
    .pipe(gulp.dest('dist/css'))
});
```

## 4.js压缩

```
var uglify = require('gulp-uglify');
gulp.task('js',function(){
    gulp.src('js/*.js')
    .pipe(uglify())
    .pipe(gulp.dist('dist/js'))
});
```

## 监听编写文件的变化，实时改变

```
gulp.task('watch',['scss','js','html','image'],function(){
    gulp.watch('*.scss',['scss']);
    gulp.watch('js/*.js',['js']);
    gulp.watch('img/*.*',['image']);
    gulp.watch('*.html',['html']);
})
最后你就可以在dos命令里执行gulp watch 按下回车，就可以开始你的工程了.
```

