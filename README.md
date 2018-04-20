### 移动web开发适配

**特性介绍**

- 跑在手机端的web页面（H5页面）
- 跨平台（ios,pc，移动端）
- 基于webview
- 告别IE拥抱webkit
- 更高的适配性和性能要求（适配性：苹果屏幕就几种，安卓的屏幕尺寸太多，性能：手机的网络状况也是需要考虑的，pc就还好）

**移动web开发与适配**

常见移动web适配方法：

PC常见做法:

- 给个960pxwidth or 1000px宽度 --给最外层容器一个宽度，同时让容器居中
- 在开发过程中会经常使用到盒子模型，给盒子定宽，定高
- 会经常采用display:inline-block属性（让div横着排列）

移动web常见做法（3种）：

- 定高，宽度百分比（1-1图，百分比案例） 
>   百分比只能实现一些简单的布局，适配方面有些不足 

- flex布局
- media query (图1-2，媒体查询)
>   flex + media query 就可以实现基本的响应式布局 

```html
    <!-- 百分比小案例(宽度设置百分比，定高) -->
    <style type="text/css" media="screen">
        .box {
            width: 100%;
            height: 100px;
            background: red;
        }
        .inner {
            width: 25%;
            height: 100px;
            background: blue;
        }
    </style>
    <div class="box">
        <div class="inner"></div>
    </div>
```
<center>1-1图，百分比案例</center>

**媒体查询**

```css
    @media 媒体类型 and (媒体特性) {
        /*css样式*/
    }

    // div布局按照上图 1-1图，百分比案例
    @media screen and (max-width: 320px) { // 屏幕宽度<=320px 才会生效
        .box {
            background: pink;
        }
        .inner {
            background: red;
        }
    }
    @media screen and (min-width: 321px) { // 屏幕宽度>=321px 才会生效
        .inner {
            width: 160px;
            height: 100px;
            background-color: yellow;
        }
    }

```
<center>图1-2，媒体查询</center>
*媒体类型*： screen， print... *媒体特性* max-width,min-width,max-height,min-height等

###rem原理与简介

- 字体单位
>   根据html根元素大小而定，同样可以作为高度、宽度等单位（图1-3 rem小案例）
>   动态修改html的font-size大小：2种方式（media query方法，js方法）
- 适配原理
>   将px替换成rem，动态修改html的font-size适配
- 兼容性
>   ios6以上和android2.1以上，基本覆盖所有主流手机系统

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <!-- 注意，一定要加这行代码 -->
    <meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title></title>
    <style>
        html {
            font-size: 20px;
        }
        .box {
            width: 2rem; /*等价于 40px=2 x html元素的font-size*/
            height: 3rem; /*60px*/
            background: red;
        }

        /*
            1rem = html根元素的font-size大小
        */
    </style>
</head>
<body>
    <div class="box">
        <p>hello rem</p>
    </div>
</body>
</html>
    
```
<center>图1-3 rem小案例</center>

```html
    <style type="text/css" media="screen">
        @media screen and (max-width: 320px) {
            html {
                font-size: 20px;
            }
        }
        @media screen and (min-width: 321px) {
            html {
                font-size: 24px;
            }
        }
    </style>
```
<center>1-4,采用media query改变html的font-size</center>

```js
    <script type="text/javascript">
        // 获取视窗的宽度
        let htmlWidht = document.documentElement.clientWidth || document.body.clientWidth;
        
        let htmlDOM = document.getElementByTagName("html")[0];
        
        // htmlDOM.style.fontSize = "20px";
        htmlDOM.style.fontSize = htmlWidht / 10 + "px";
    </script>

```
<center>1.5, 使用js动态改变html的font-size</center>

###采用rem适配页面实战

- rem基准值的计算
    html的font-size=20px  1rem = 20px;
- rem数值计算与构建
    160px换算成rem  160px / 20 = 8rem
- rem与sass结合使用

    npm install node-sass

    node-sass a.sass b.css

```sass
    @function px2rem($px) {
        $rem: 37.5px // iphone66/7/8是375px宽，375/10 基准值取10px
        @return (@px/$rem) + rem;
    }

    .hello {
        width: px2rem(100px);
        height: px2rem(100px);
    }
```

- rem适配实战

**rem高仿网易新闻H5版新闻列表页**

步骤列表

1. 页面框架搭建 （构建，scss）

2. 页面样式编写

3. rem计算代码编写

4. 适配多种机型大小，resize完善

###总结

------

## 番外篇

### scss

**tips** 在定义方法的时候，要定义在使用的前面
```css
// 放到前面
@function px2rem($px) {
    $rem: 37.5px; // 注意，这里带单位
    @return ($px/$rem) + rem;
}

.helle {
    width: px2rem(100px);
    height: px2rem(200px);
    &.b {
        width: px2rem(50px);
        height: px2rem(50px);

    }
    // &相当与父元素
}
// 编译后的结果

.helle {
  width: 2.66667rem;
  height: 5.33333rem; }
  .helle.b {
    width: 1.33333rem;
    height: 1.33333rem; }
```

初次使用，将方法放到后面，编译能够通过，但是，单位并没有变。


### node-sass使用

>   将.scss文件快速编译成css文件

命令行用法：

1. npm install -g node-sass 安装

2. node-sass -v 查看安装版本

    >   node-sass -v  控制台返回如下信息
    >   
    >   node-sass   4.8.3   (Wrapper)   [JavaScript] 
    >   
    >   libsass     3.5.2.beta.2    (Sass Compiler) [C/C++]

3. node-sass [options] <input原始文件> [output 输出文件]

    或者<\input> | node-sass >output 
    >   案例： node-sass src/style.scss dest/bundle.css

    >   input可以是.scss文件、.sass文件、还可以是一个目录，如果是目录，则必须指定--output标志
    >   
    >   --importer这个参数目前不了解作用
    >   
    >   --source-map 不知道怎么用
    
**options**

```text
    -w, --watch                监听文件或目录变化
    demo： node-sass --watch a.scss build.css

    -r, --recursive            Recursively watch directories or files
    -o, --output               Output directory
    -x, --omit-source-map-url  Omit source map URL comment from output
    -i, --indented-syntax      Treat data from stdin as sass code (versus scss)
    -q, --quiet                Suppress log output except on error
    -v, --version              Prints version info
    --output-style             CSS output style (nested | expanded | compact | compressed)
    --indent-type              Indent type for output CSS (space | tab)
    --indent-width             Indent width; number of spaces or tabs (maximum value: 10)
    --linefeed                 Linefeed style (cr | crlf | lf | lfcr)
    --source-comments          Include debug info in output
    --source-map               Emit source map
    --source-map-contents      Embed include contents in map
    --source-map-embed         Embed sourceMappingUrl as data URI
    --source-map-root          Base path, will be emitted in source-map as is
    --include-path             Path to look for imported files
    --follow                   Follow symlinked directories
    --precision                The amount of precision allowed in decimal numbers
    --error-bell               Output a bell character on errors
    --importer                 Path to .js file containing custom importer
    --functions                Path to .js file containing custom functions
    --help                     Print usage info
```

示例：

```js
    node-sass a.scss b.css  直接将a.scss文件编译成b.css文件

    node-sass --watch --recursive --output css scss 运行以下命令会持续观察文件变化并即时编译

    node-sass -wro css scss 和上面的命令等同 wro就是watch、recursive、output缩写
```



