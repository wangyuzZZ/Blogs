![less](https://user-images.githubusercontent.com/12387544/29506604-39a2658e-867f-11e7-88f5-ace2c8d21871.jpeg)

参考文档：

- [less 中文网](http://lesscss.cn/)
- [less 快速入门](http://less.bootcss.com/#getting-started)
- [less 指南](http://www.bootcss.com/p/lesscss/#guide)

#  简介

Less is More, Than CSS - 少即是多，比如CSS。

Less 是一门 CSS 预处理语言。

Less 是一种由Alexis Sellier设计的动态[层叠样式表](https://zh.wikipedia.org/wiki/%E5%B1%82%E5%8F%A0%E6%A0%B7%E5%BC%8F%E8%A1%A8)语言，受Sass所 影响，同时也影响了Sass的新语法：SCSS。

LESS是开源的，其第一个版本由Ruby写成，但在后续的版本当中，Ruby逐渐被替换为[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)。受益于JavaScript，LESS可以在客户端上运行（IE6+、Webkit、Firefox），也可以在服务端运行（Node.js、Rhino）

在语法方面，LESS与CSS较为接近，一个合法的CSS代码段本身也是一段合法的LESS代码段。LESS提供[变量](https://zh.wikipedia.org/wiki/%E5%8F%98%E9%87%8F)、嵌套、混合（mixin）、[操作符](https://zh.wikipedia.org/w/index.php?title=%E6%93%8D%E4%BD%9C%E7%AC%A6&action=edit&redlink=1)、[函数](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B0)等一般编程所需的抽象机制。

#  编译

## 1、nodeJS -> npm 

> 安装 [nodeJS](http://nodejs.cn/download/)

nodeJS 默认安装在  */usr/local/lib/node_modules* 下

nodeJS 默认自带 npm，无需再安装 npm 

npm 路径为 */usr/local/lib/node_modules/npm*

通过 `$ node -v` 可查看node版本

通过 `$ npm -v` 可查看npm版本

> 本地安装

```shell
$ npm i -D less
```

> 编译

编译 less 文件，首先你应该输入以下指令测试less编译指令是否可用：

```shell
$ ./node_modules/.bin/lessc -v
lessc 3.8.1 (Less Compiler) [JavaScript]
```

如果在命令输入后，下方出现的Less的版本号，那证明Less的这个lessc命令是可用的。

我们来新建一个项目，项目目录如下所示：

```
|--proj
|  |--dist
|     |--index.html
      |--static
         |-- images
         |-- css
         |-- js
         |-- pages              
|  |--src
|     |-- js
|     |-- less	
|         |-index.less
```

我们现在要编译less文件夹下的 *index.less* 文件，编译时需先定位到项目中的less文件目录，然后使用如下命令进行编译，这样就可以将 *index.less* 文件编译成 *index.css* 文件了。

```shell
$ ./node_modules/.bin/lessc ./src/less/index.less ./dist/static/css/index.css
```

> 编译压缩文件

首先你需要安装压缩插件

注意：安装路径需放置在less目录下（ */usr/local/lib/node_modules/less* ）

```shell
$ npm i -D less-plugin-clean-css
```

然后在编译的时候添加 `-clean-css` 即可，如下嗾使：

```shell
$ ./node_modules/.bin/lessc ./src/less/index.less ./dist/static/css/index.c
ss -clean-css
```

这样，你的less文件就被编译成压缩后的css文件了。

## 2、Webstorm 编译

使用 Webstorm 编译 less之前你需安装 node.js 与 less，具体步骤参考编译方法1。

这里假定你已经安装了 node.js 与 less，接下来我们就需要在Webstorm中进行相关配置就可以了。

具体操作步骤如下：

- 进入 webstorm -> settings -> File Watchers -> 点击 `+` ->选择 Less

![less_ws_config](https://user-images.githubusercontent.com/12387544/34650745-15a6b03c-f401-11e7-9805-6e1bfbb04cff.jpeg)

  其中：

  `Program`：为lessc编译路径，默认为 *usr/local/bin/lessc*，这里是自动获取的，我们不用设置。

  `Arguments`：为less文件路径，默认为 *--no-color $FileName$*

  `Output paths to refresh`：为编译后css放置路径，默认为 *$FileNameWithoutExtension$.css*

  到这一步，当我们编辑less时，webstorm将会为我们自动编译less文件，但通常情况下，我们都会去设置less编译后css放置的文件，如何设置呢？请参考后续步骤。

- 设置路径

  -> less文件放置目录为 *proj/public/styles/less/*

  -> less产出的css文件放置目录为 *proj/public/styles/css/*

  我们这里只需要设置 `Output paths to refresh` 路径即可：

  ```java
  $FileParentDir$/css/$FileNameWithoutExtension$.css
  ```

- 路径配置解读

  --no-color：禁用色彩输出

  --source-map：资源映射文件

  $FileName$：将要被编译的less文件

  $ProjectFileDir$：表示当前项目的根目录

  $FileParentDir$ ：表示的是当前文件所在文件夹的父级文件夹,而不是当前文件的父文件夹.

  $FileDirPathFromParent$：是获取指定文件下的目录

  $FileNameWithoutExtension$：获取不带后缀名的文件

- 压缩

  安装好插件之后，在配置webstorm less时，在 `Arguments` 中添加如下代码：

  ```javascript
  --plugin=less-plugin-clean-css
  ```

  完整路径如下：

  ```javascript
  --plugin=less-plugin-clean-css --no-color $FileName$
  ```

## 3、VSCode 编译

首先在VSCode中下载 “Easy Less” 插件，然后再.less文件第一行添加如下代码即可：

```less
// out: ../../dist/static/css/index.css, compress: false, sourceMap: false
```

> out：输出路径及文件名称
>
> compress：是否压缩
>
> sourceMap：是否生成资源映射文件便于谷歌浏览器中调试

或者：

```json
"less.compile": {
     // 是否压缩
     "compress": false,
  	 // 是否自动添加前缀
     "autoprefixer": "> 5%, last 2 Chrome versions, not ie 6-9",
     // 资源映射
     "sourceMap": false,
  	 // 输出路径
     "out": "${workspaceRoot}/dist/static/css/"
}
```

## 4、Koala 编译

如果希望用第三方自动编译器可以试试一个叫做“Koala”（考拉）的CSS预处理语言的自动化编译客户端软件，非常的方便好用支持LESS、SASS等多种语言。

官网下载地址：http://koala-app.com/index-zh.html

#  语法

## 1、注释

- /* 此注释方法会编译 */
- // 此注释方法不会被编译 

## 2、变量

声明语法：`@变量名:值;`，如：`@box_len:300px;`

```less
// 定义变量
@boxLen : 160px;
@bgColor: #000;
// 使用变量
.box {
  	width:  @boxLen;     
  	height: @boxLen;      
  	background: @bgColor; 
}
```

编译后的样式为：

```css
.box {
  width: 160px;
  height: 160px;
  background: #000;
}
```

## 3、混合

### 3.1、基本使用

我们先来看一个示例，帮助理解混合。

```less
// 定义变量
@boxLen : 160px;
@bgColor: #000;

// 定义.box样式
.box {
 	 // 使用变量
  	width:  @boxLen;
  	height: @boxLen;
  	background: @bgColor;
  	// 混合使用
  	.bordered;
}

.bordered {
 	 border: 5px solid #808080;
}
```

运行程序，我们可以发现黑色视图多了一个边框，我们知道，以前要实现这个边框，我们需要为 `.box`  添加一个类 `.bordered`，在 less 中，利用混合这一特性，我们可以直接在 `.box {}` 内使用 `.bordered` ，而无需再到html页面中为对应元素添加类名。

### 3.2、混合参数

混合还可以带参数，假设我们定义了一个边框，在后面的盒子中我们都会采用这个边框，但是每个盒子的边框粗细不一样，这个时候我们可以使用混合参数来实现这一功能，具体参考下面的示例：

```less
// 定义变量
@boxLen : 160px;
@bgColor: #000;

// 定义.box样式
.box {
    // 使用变量
    width:  @boxLen;
    height: @boxLen;
    background: @bgColor;
    // 混合使用
    // 传入参数
    .bordered(10px);
}

.bordered(@borderW) {
    border: @borderW solid #808080;
}

```

### 3.3、混合默认值

我们也可以为混合指定一个默认值，这样在调用时就无需传入参数， 但是切记，括号不能省略。

```less
.box {
	.bordered();
}
.bordered(@borderW:10px) {
  	border: @borderW solid pink;
}

```

提示：混合如果定义时，设置了参数，那么在调用时必须有圆括号，否则程序报错。

```less
.border-radius(@radius: 5px) {
    -webkit-border-radius: @radius;
    -moz-border-radius: @radius;
    border-radius: @radius;
}

```

### 3.4、arguments 变量

`@arguments `包含了所有传递进来的参数. 如果你不想单独处理每一个参数的话就可以像这样写:

```less
.box-shadow(@offsetX: 0px, @offsetY: 0px, @blur: 1px, @color: purple) {
    -webkit-box-shadow: @arguments;
    -moz-box-shadow: @arguments;
    box-shadow: @arguments;
}
.box {
    .box-shadow(6px, 8px);
}

```

输出为：

```css
.box {
    -webkit-box-shadow: 6px 8px 1px purple;
    -moz-box-shadow: 6px 8px 1px purple;
    box-shadow: 6px 8px 1px purple;
}

```

再来看一个示例：

```less
.border(@width:5px; @style: solid; @color: #000) {
  	// border: @width @style @color;
    border: @arguments;
}

```

## 4、模式匹配

相当于编程语言中的if语句，即如果满足条件，则执行。我们来看一个示例：

```less
.triangle(top; @border_width: 5px; @border_color: #000;) {
  border: @border_width solid transparent;
  border-bottom-color: @border_color;
}
.triangle(bottom; @border_width: 5px; @border_color: #000;) {
  border: @border_width solid transparent;
  border-top-color: @border_color;
}
.triangle(left; @border_width: 5px; @border_color: #000;) {
  border: @border_width solid transparent;
  border-right-color: @border_color;
}
.triangle(right; @border_width: 5px; @border_color: #000;) {
  border: @border_width solid transparent;
  border-left-color: @border_color;
}
// 默认执行(无论是否匹配，都会执行)
.triangle(@_; @border_width: 5px; @border_color: #000;) {
  width:   0px;
  height:  0px;
  overflow: hidden;
}
.box {
  .triangle(top);
}

```

上述示例中，调用 .triangle ，根据参数进行匹配，选择不同的样式。

## 5、引导

当我们想根据 **表达式** 进行匹配，而非根据值和参数匹配时，引导就显得非常有用。如果你对函数式编程非常熟悉，那么你很可能已经使用过导引。

为了尽可能地保留CSS的可声明性，LESS通过导引混合而非if/else语句来实现条件判断，因为前者已在@media query特性中被定义。

以此例做为开始：

```less
.mixin (@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin (@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin (@a) {
  color: @a;
}

```

when关键字用以定义一个引导序列(此例只有一个引导)。接下来我们运行下列代码：

```less
.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }

```

就会得到：

```css
.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}

```

导引中可用的全部比较运算有： **> >= = =< <**。此外，关键字`true`只表示布尔真值，下面两个混合是相同的：

```less
.truth (@a) when (@a) { ... }
.truth (@a) when (@a = true) { ... }

```

除去关键字true以外的值都被视示布尔假：

```less
.class {
  .truth(40); // Will not match any of the above definitions.
}

```

导引序列使用逗号‘`,`’—分割，当且仅当所有条件都符合时，才会被视为匹配成功。

```less
.mixin (@a) when (@a > 10), (@a < -10) { ... }

```

导引可以无参数，也可以对参数进行比较运算：

```less
@media: mobile;
.mixin (@a) when (@media = mobile) { ... }
.mixin (@a) when (@media = desktop) { ... }

.max (@a, @b) when (@a > @b) { width: @a }
.max (@a, @b) when (@a < @b) { width: @b }

```

最后，如果想基于值的类型进行匹配，我们就可以使用is*函式：

```less
.mixin (@a, @b: 0) when (isnumber(@b)) { ... }
.mixin (@a, @b: black) when (iscolor(@b)) { ... }

```

下面就是常见的检测函式：

- `iscolor`
- `isnumber`
- `isstring`
- `iskeyword`
- `isurl`

如果你想判断一个值是纯数字，还是某个单位量，可以使用下列函式：

- `ispixel`
- `ispercentage`
- `isem`

最后再补充一点，在导引序列中可以使用**and**关键字实现与条件：

```less
.mixin (@a) when (isnumber(@a)) and (@a > 0) { ... }

```

使用**not**关键字实现或条件

```less
.mixin (@b) when not (@b > 0) { ... }

```

## 6、运算

Less 支持加减乘除运算。

```less
@width: 100px;
.box {
    width: @width - 20;
}

```

## 7、嵌套规则

Less允许嵌套，我们来看一个示例：

```html
<ul class="list">
    <li><a href="javascript:;">测试链接</a><span>2017/08/21</span></li>
    <li><a href="javascript:;">测试链接</a><span>2017/08/21</span></li>
    <li><a href="javascript:;">测试链接</a><span>2017/08/21</span></li>
    <li><a href="javascript:;">测试链接</a><span>2017/08/21</span></li>
    <li><a href="javascript:;">测试链接</a><span>2017/08/21</span></li>
    <li><a href="javascript:;">测试链接</a><span>2017/08/21</span></li>
</ul>

```

使用css设计标签样式时，我们经常会这样写：

```css
.list {}
.list li {}
.list a {}
.list span {}

```

Less引入嵌套之后，我们可以这样来设计样式：

```css
.list {
  	li {}
  	a  {}
  	span {}
}

```

当然Less也允许使用`&` 符号添加伪类，比如，我们要给超链接添加一个hover效果，css会这样写：

```css
li a { text-decoreation: none; color: #000; }
li a:hover { text-decoreation: underline; color: red;}

```

这种方法有些复杂，Less实现则更加简单一些：

```less
li a {
  	text-decoreation: none; color: #000;
    // & 表示当前选择器
  	&:hover { text-decoreation: underline; color: red;}
}

```

## 8、函数

### 1、color 函数

LESS 提供了一系列的颜色运算函数. 颜色会先被转化成 *HSL* 色彩空间, 然后在通道级别操作:

```less
lighten(@color, 10%);     // return a color which is 10% *lighter* than @color
darken(@color, 10%);      // return a color which is 10% *darker* than @color

saturate(@color, 10%);    // return a color 10% *more* saturated than @color
desaturate(@color, 10%);  // return a color 10% *less* saturated than @color

fadein(@color, 10%);      // return a color 10% *less* transparent than @color
fadeout(@color, 10%);     // return a color 10% *more* transparent than @color
fade(@color, 50%);        // return @color with 50% transparency

spin(@color, 10);         // return a color with a 10 degree larger in hue than @color
spin(@color, -10);        // return a color with a 10 degree smaller hue than @color

mix(@color1, @color2);    // return a mix of @color1 and @color2

```

使用起来相当简单:

```less
@base: #f04615;

.class {
  color: saturate(@base, 5%);
  background-color: lighten(spin(@base, 8), 25%);
}

```

你还可以提取颜色信息:

```less
hue(@color);        // returns the `hue` channel of @color
saturation(@color); // returns the `saturation` channel of @color
lightness(@color);  // returns the 'lightness' channel of @color

```

如果你想在一种颜色的通道上创建另一种颜色，这些函数就显得那么的好用，例如:

```less
@new: hsl(hue(@old), 45%, 90%);

```

`@new` 将会保持 `@old`的 *色调*, 但是具有不同的饱和度和亮度.

### 2、math 函数

LESS提供了一组方便的数学函数，你可以使用它们处理一些数字类型的值:

```less
round(1.67); // returns `2`
ceil(2.4);   // returns `3`
floor(2.6);  // returns `2`

```

如果你想将一个值转化为百分比，你可以使用`percentage` 函数:

```less
percentage(0.5); // returns `50%`

```

## 9、命名空间

有时候，你可能为了更好组织CSS或者单纯是为了更好的封装，将一些变量或者混合模块打包起来, 你可以像下面这样在`#bundle`中定义一些属性集之后可以重复使用:

```less
#bundle {
  .button () {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover { background-color: white }
  }
  .tab { ... }
  .citation { ... }
}

```

你只需要在 `#header a`中像这样引入 `.button`:

```less
#header a {
  color: orange;
  #bundle > .button;
}

```

## 10、作用域

LESS 中的作用域跟其他编程语言非常类似，首先会从本地查找变量或者混合模块，如果没找到的话会去父级作用域中查找，直到找到为止.

```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}

#footer {
  color: @var; // red  
}

```

## 11、Importing

你可以在main文件中通过下面的形势引入 `.less` 文件, `.less` 后缀可带可不带:

```less
@import "lib.less";
@import "lib";

```

如果你想导入一个CSS文件而且不想LESS对它进行处理，只需要使用`.css`后缀就可以:

```less
@import "lib.css";

```

这样LESS就会跳过它不去处理它.

## 12、字符串插值

变量可以用类似ruby和php的方式嵌入到字符串中，像`@{name}`这样的结构:

```less
@base-url: "http://assets.fnord.com";
background-image: url("@{base-url}/images/bg.png");

```

> 提示：LESS中不能使用单引号`‘`；

## 13、避免编译

有时候我们需要输出一些不正确的CSS语法或者使用一些LESS不认识的专有语法。要输出这样的值我们可以在字符串前面加上一个：~ 。比如:

```less
width: ~'calc(100% - 35px)'

```

## 14、!important 关键字

类似于css中的 `!important`，提升样式优先级。

  