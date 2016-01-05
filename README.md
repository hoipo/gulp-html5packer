# gulp-html5packer

移动H5页面打包工具

版权声明：本插件源码fork自filow的gulp_h5packer项目，版权归filow所有，本人修改了少量的bug并发布来便于本团队使用。

为了减少移动端H5页面的请求数量，往往会在发布前将页面依赖的JS, CSS文件嵌入html中，如果一些资源有全局资源包的支持，也会将其替换为CDN地址。

本插件可以根据link, script, img标签中的标记，选择将文件内容嵌入或者替换为CDN地址。

##功能

1.支持img标签内图片转化为BASE64

2.可以将外联js和CSS的压缩并且转换注入到内联标签里面

3.可以支持网络文件的嵌入和BASE64

## 用法

首先，在待处理的html文件中加入标记。标记共有三种：

### 内容替换标记

```html
<link rel="stylesheet" type="text/css" href="something.css" data-replace="true">
<script src="something.js" data-replace="true"></script>
```

这样就会读取资源文件，把内容嵌入在标签的位置。script标签自动会删去src和data-replace属性，并在标签内嵌入内容。link标签会在后面产生一个style节点，并嵌入内容。标签本身会被删除。

目前插件会自动压缩css和js文件，未来可以通过配置文件取消这一功能。

### 资源URL替换标记

```html
<link rel="stylesheet" type="text/css" href="local/amui.css" data-replace="amui">
<script src="zepto.js" data-replace="static-zepto"></script>
```

在data-replace中写入"static-全局资源包名称"，即可替换为相应的地址。

替换静态资源包需要先写好配置文件，请在初始化参数中传入：
```js
{
  staticUrl: {
    name: 'http://url.to.path'
  }
}
```

### 图片Base64转化标记
```html
<img src="fake.png" data-replace="base64">
```

注意： 目前Base64转码不考虑文件大小因素，请不要在大图片上加这个标记！

## 引用
设置完HTML后，在gulpfile.js中引用：

```js
var packer = require('gulp-html5packer');
gulp.task('pack', function (){
  
  gulp.src('./build/*/*.html')
    .pipe(packer())
    .pipe(gulp.dest('./pack'));
});
```
