# HTML

## h5新特性

标签：

- 语意化标签 header、nav、main、article、section、aside、footer
  > 优点：易于用户阅读、有利于开发和维护、有利于SEO、方便屏幕阅读器解析

- 媒体播放的 video 和 audio
- 绘画 canvas
- 增强表单控件 calendar、date、time、email、url、search
- Form Data 对象

浏览器：

- 本地存储 localStorage 和 sessionStorage
- 离线应用 manifest
- 桌面通知 Notifications

JS：

- 新增选择器 document.querySelector、document.querySelectorAll
- 拖拽释放(Drag and drop) API
- 地理位置 Geolocation
- 历史管理 history
- 页面可见性改变事件 visibilitychange
- 跨窗口通信 PostMessage
- 全双工通信协议 websocket
- 跨域资源共享(CORS) Access-Control-Allow-Origin



## !DOCTYPE 标签：

告诉浏览器（解析器）应该以什么样（html或xhtml）的文档类型定义来解析文档

- **标准模式**：默认，严格模式，Strick mode，CSS1Compat
- **混杂模式**：比较宽松的向后兼容的方式显示，Quick mode，BackCompat



## Head标签

用于定义脚本、样式引用、提供元信息，描述了文档的各种属性和信息，其中title是必须的



## 常用meta标签

- charset，用来描述HTML文档的编码类型
```html
<meta charset="UTF-8" >
```

- name content 组合

	- keywords 关键词
  ```html
  <meta name="keywords" content="关键词" />
  ```
  
  - description 页面描述
  ```html
  <meta name="description" content="页面描述内容" />
  ```
  
  - author 作者
  ```html
  <meta name="author" content="作者" />
  ```
  
  - viewport 适配移动端，可以控制视口的大小和比例
  
    - `width viewport` ：宽度(数值/device-width)
    - `height viewport` ：高度(数值/device-height)
    - `initial-scale` ：初始缩放比例
    - `maximum-scale` ：最大缩放比例
    - `minimum-scale` ：最小缩放比例
	  - `user-scalable` ：是否允许用户缩放(yes/no）
  ```html
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
  ```
  
  - robots 搜索引擎索引方式（文件能否被检索？页面链接能否被查询？）
    - `all`：文件将被检索，且页面上的链接可以被查询；
    - `none`：文件将不被检索，且页面上的链接不可以被查询；
    - `index`：文件将被检索；
    - `noindex`：文件将不被检索；
    - `follow`：页面上的链接可以被查询；
    - `nofollow`：页面上的链接不可以被查询。
  ```html
  <meta name="robots" content="index,follow">
  ```
  
  - copyright 版权信息
  - renderer 指定双核浏览器渲染页面的方式
  - application name 应用名称
  
- http-equiv content组合
  
  - refresh 页面重定向和刷新
  ```html
  <meta http-equiv="refresh" content="0;url=" />
  ```
  
  - content-Type 可声明字符集
  ```html
  <meta http-equiv="content-Type" content="text/html charset=UTF-8" />
  ```
  
  - X-UA-Compatible 指定IE和Chrome使用最新版渲染页面
  ```html
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  ```
  
  - cache-control 指定所有缓存机制
  ```html
  <meta http-equiv="cache-control" content="no-cache" />
  ```



## script标签中defer和async的区别

默认两者没有：同步，会堵塞文档加载，直到下载加载完。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b0a8a139519f46dfa2d1992c58eb5397~tplv-k3u1fbpfcp-watermark.awebp)

defer：html4.0，延迟加载，边下载边加载，等文档加载完再加载它

async：html5.0，优先级更高，一边下载一边加载文档，下载后暂停解析文档加载它

相同点：都是异步，加载时不堵塞页面渲染、当script中间有代码都不起作用、脚本不能调用document.write方法、有onload事件回调



## JavaScript脚本延迟加载的方式有哪些

1. defer 属性： 给 js 脚本添加 defer 属性，这个属性会让脚本的加载与文档的解析同步解析，然后在文档解析完成后再执行这个脚本文件，这样的话就能使页面的渲染不被阻塞。多个设置了 defer 属性的脚本按规范来说最后是顺序执行的，但是在一些浏览器中可能不是这样。
2. async 属性： 给 js 脚本添加 async 属性，这个属性会使脚本异步加载，不会阻塞页面的解析过程，但是当脚本加载完成后立即执行 js 脚本，这个时候如果文档没有解析完成的话同样会阻塞。多个 async 属性的脚本的执行顺序是不可预测的，一般不会按照代码的顺序依次执行。
3. 动态创建 DOM 方式： 动态创建 DOM 标签的方式，可以对文档的加载事件进行监听，当文档加载完成后再动态的创建 script 标签来引入 js 脚本。
4. 使用 setTimeout 延迟方法： 设置一个定时器来延迟加载js脚本文件
5. 让 JS 最后加载： 将 js 脚本放在文档的底部，来使 js 脚本尽可能的在最后来加载执行。



## img的srcset属性的作⽤

响应式页面中经常用到根据屏幕密度设置不同的图片。这时就用到了 img 标签的srcset属性。srcset属性用于设置不同屏幕密度下，img 会自动加载不同的图片。用法如下：

```
<img src="image-128.png" srcset="image-256.png 2x" />
```

使用上面的代码，就能实现在屏幕密度为1x的情况下加载image-128.png, 屏幕密度为2x时加载image-256.png。

按照上面的实现，不同的屏幕密度都要设置图片地址，目前的屏幕密度有1x,2x,3x,4x四种，如果每一个图片都设置4张图片，加载就会很慢。所以就有了新的srcset标准。代码如下：

```
<img src="image-128.png" srcset="image-128.png 128w, image-256.png 256w, image-512.png 512w" sizes="(max-width: 360px) 340px, 128px" />
```

其中srcset指定图片的地址和对应的图片质量。sizes用来设置图片的尺寸零界点。对于 srcset 中的 w 单位，可以理解成图片质量。如果可视区域小于这个质量的值，就可以使用。浏览器会自动选择一个最小的可用图片。

sizes语法如下：sizes="[media query] [length], [media query] [length] ... "

sizes="[media query] [length], [media query] [length] ... "



## 空元素无闭合标签

br、hr、img、input、link、meta



## Web worker

由于js是单线程运行的，只能同时执行一个脚本，web worker可以创建出另外一个线程来运行，不会堵塞主进程，并通过postMessage来跟主线程进行通讯，把结果传回主线程  

创建web worker：检测兼容性、创建 web worker 文件（js，回传函数等）、创建web worker对象



## 离线储存 Manifest

操作：
 - 1.创建一个xxx.appcache文件，html标签加上manifest="xxx.manifest"

 - 2.在 xxx.manifest 文件中编写需要离线存储的资源：

  ```
    CACHE MANIFEST
        #v0.11
        CACHE:
        js/app.js
        css/style.css
        NETWORK:
        resourse/logo.png
        FALLBACK:
        / /offline.html
  ```

	 CACHE：离线存储的资源列表
	 NETWORK：只有在在线的情况下才能访问
	 FALLBACK：访问第一个资源失败，那么就使用第二个资源来替换  

 - 更新缓存：更新 xxx.manifest 文件、通过js操作、清空浏览器缓存
 - 资源管理和加载：
   - 在线：请求服务器的 manifest 文件对比
   - 离线：直接使用离线存储资源



## 浏览器乱码的原因

- html源代码的编码和浏览器解析的编码不一致，可通过编辑器修改源代码编码
- 数据库和html编码不一致，可在后台转码后传给前端
- 浏览器不能自动识别编码，可在浏览器设置里手动设置编码



## 渐进增强和优雅降级之间的区别

- 逐渐增强：针对低版本浏览器进行页面重构，保证基本功能情况下，再针对高版本浏览器进行效果，交互等方面改进和追加功能，达到更好的用户体验

- 优雅降级：一开始就构建完整的功能，然后再针对低版本的浏览器进行兼容



## 拖拽Drag Api

- **dragstart**：事件主体是**被拖放**元素，在开始拖放被拖放元素时触发。
- **darg**：事件主体是**被拖放**元素，在正在拖放被拖放元素时触发。
- **dragend**：事件主体是**被拖放**元素，在整个拖放操作结束时触发。
- **dragenter**：事件主体是**目标**元素，在被拖放元素进入某元素时触发。
- **dragover**：事件主体是**目标**元素，在被拖放在某元素内移动时触发。
- **dragleave**：事件主体是**目标**元素，在被拖放元素移出目标元素是触发。
- **drop**：事件主体是**目标**元素，在目标元素完全接受被拖放元素时触发。
