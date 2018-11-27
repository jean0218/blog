前端工程师是个背锅的存在
- 怎么没数据了？——>找前端
- 页面怎么又点不动了?——>找前端
- 不同的机型（IOS/ANDRIA）页面展示效果不一样，还是——>找前端
前端是最接近用户的工种，尤其是Hybrid App涉及到三方协作，任何一方出了问题，第一个找的就是前端。这些问题中有些并不是前端错误引起的BUG，如何定位问题出在哪一方呢？





## 调试工具——chrome开发者工具
我们先来看看前端工程师必备法宝——chrome开发者工具，按F12打开：

 

### 面板介绍
**Elements（元素面板）**

查找网页源代码HTML中的任一元素，手动修改任一元素的属性和样式且能实时在浏览器里面得到反馈，主要用于DOM和CSS调试。

在使用react或vue等javaScirpt库生成DOM结构时，可在该面板查看DOM结构是否成功渲染。
移动端遇到兼容性问题时，也可以通过该面板查看DOM结构，排查错误。

**Console（控制台面板）**

记录开发者开发过程中的日志信息，且可以作为与JS进行交互的命令行Shell。

**Sources(源代码面板)**

断点调试Javascript

**NetWork(网络面板)**

从发起网页页面请求Request后，分析HTTP请求后得到的各个请求资源信息（包括状态、资源类型、大小、所用时间等），可以根据这个进行网络性能优化。
是与后端联调时重点要查看的面板。

**Performance**

资源录制，常用于性能统计调试

**Memory**

工具主要是用来检测CPU占用程度，堆栈申请的内存。

**Application(资源面板)**

记录网站加载的所有资源信息，包括存储数据（Local Storage、Session Storage、IndexedDB、Web SQL、Cookies）、缓存数据、字体、图片、脚本、样式表等。

**Security(安全面板)**

判断当前网页是否安全。

**Audits**
对当前网页进行网络利用情况、网页性能方面的诊断，并给出一些优化建议。比如列出所有没有用到的CSS文件等。

在联调时Network，Console，Application面板用得最多。

## 与后端联调接口
### 一、XHR面板

chrome开发者工具是个很齐全的前端调试工具，涉及的内容较多，可以调试DOM、CSS兼容，打断点调试Javascript，还可以根据各类输出优化性能。这一次，我们只讲前端与后端数据联调部分，所以我们重点关注network面板。

前后端数据交互，多是XHR（JS的一套HTTP调用接口）类型的请求，打开XHR面板，点击任意一条xhr资源name可以看到该资源的详细信息：

#### 包括如下tab信息：

Headers 该资源的HTTP头信息。

Preview 根据你所选择的资源类型（JSON、图片、文本）显示相应的预览。

Response 显示HTTP的Response信息。

Cookies 显示资源HTTP的Request和Response过程中的Cookies信息。

Timing 显示资源在整个请求生命周期过程中各部分花费的时间。

### 二、页签说明
#### Headers页签

**General**

Request URL ：Client请求地址，后端给你的url地址，对应ajax请求中的url。

Request Method：请求类型 

get、post、put、delete等，对应ajax请求中的type。

Status Code：响应状态码 

200、404、503等，后端返回的响应代码，可根据响应码判断是否成功建立连接

Remote Address：域名对应的真实ip:port

Response header

**Content-Type**：响应内容的格式/类型text/html;charset=UTF-8标识返回的内容是文本类型，html格式。后端返回的文件类型

**Content-Encoding**：web服务器支持的返回内容压缩编码类型 gzip
此类选项，多在nginx中配置，前后端联调时较少关注，主要用于压缩代码的传输格式，性能优化手段中常用。

Request header

Accept：客户端/发送端能够接收的数据类型 text/html,application/xhtml+xml,application/xml；

Accept-Encoding：浏览器可以支持的- web服务器返回内容压缩编码类型 gzip, deflate

Content-Type: 请求内容的格式/类型 application/x-www-form-urlencoded application/json
对应fetch中的content-type，多数用于联调第一个接口，范式定义了，后续更改较少。
Cookie: 客户端缓存的Cookie，在请求时发送给服务端验证身份
Host: 请求的服务器域名
Referer: 当前请求的来源
Form data(post)/Query String Parameter(get)

Form data(post)：Request Method为post请求类型时显示的post表单数据。

Query String Parameter(get)：Request Method为get请求类型时向服务端传递的请求参数。
你发送给后端的参数，都在此选项中查看，如有rap文档，可与rap文档对比查询参数。


Preview页签

Preview页签展示请求响应后的预览，以json形式解析后端返回的数据。


Response页签
Response页签显示响应的具体内容，查看后端返回给你的数据，一般是先看response标签页查看返回的数据是否符合要求，如果符合要求再查看Preview标签解析数据（有机会补足后端返回错误的失败案例）。



Cookie页签
Cookie页签以key-value形式展示客户端所有的Cookie信息，查看Cookie是否正确。



### 三、服务请求被代理
前后端联调时，第一个遇到的大老虎即是跨域，我们先来看看为什么会有跨域这个问题出现：

> 跨域，指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制。

在参与出未校园项目和来米项目时，发现页面加载URL的IP和向后端发送请求的IP是一样的，这个就痛苦了，我怎么知道连接的后端到底是哪个IP？



 仔细查看webpack代码配置，才发现请求被代理了:



vue-cli和create-react-app为了解决跨域问题，采用了http-proxy-middleware中间件。设置proxyTable代理当前地址向后端发送请求，我们先来看看实现原理： 

> 浏览器禁止跨域，服务端并不禁止跨域，所以浏览器可以发送请求给自己的服务端，然后由自己的服务端再转发给要跨域的服务端。

这也就是为什么我们看到的发送给后端服务的请求IP与页面加载的url中的IP一致。proxyTarget属性的值，就是你实际请求的地址，这里写的就是程序员哥哥给你的ip，你也可以在启动的时候console.log输出这个地址。



### 四、常见错误分析

1. 常见的跨域报错： Access-Control-Allow-Origin

前后端联调，查看到status code 报400，打开console控制台，看到Access-Control-Allow-Origin这样的提示信息，这就是跨域问题引起的报错，这就要看你的团队中的跨域解决方案了，如果是后端配置跨域头，那这就需要去找后端解决。也可以参见前面介绍的http-proxy-middleware中间件，设置proxyTable代理。



> 跨域知识扩展：
> 当发生跨域请求时，fetch或ajax会先发送一个options请求，来确认服务器是否允许接受请求。服务器同意后，返回status code 值为200，才会发送真正的请求，第二次请求中才会看到发送请求的参数。

2. 504 Gateway Timeout

504 Gateway Time-out的字面意思可以理解为网页请求超时，这是我们在浏览网站网页时发出的请求没有响应，后端服务的问题，请找联调接口的人员处理。

3. 500 Server Error



服务端错误，请找对接的后端程序员处理。

## 第三节 移动端对接
移动端联调主要是三类问题：

- 第一类问题，后端返回的数据是否符合要求
- 第二类问题，JSBridge传参或回调错误
- 第三类问题，兼容性问题

三类问题中，除了第一类查看服务端返回的数据是否符合要求可以在pc机上调试，另外两种都需要真机调试，如何连接真机调试呢？

**非常重要：真机调试的APP一定要用debug包，找对接的APP开发人员打debug包给你，同时你部署到测试服务器的JS、CSS不要开启压缩模式。**

### 一、真机调试

#### Android 真机调试

桌面端的前端开发调试非常简单，因为有 Firebug、Chrome DevTools 等工具，直接右击打开就可以看到元素的 CSS，并且可以查看各种资源、修改运行调错 JS 等。而移动端浏览器显然没法带有这些功能，我们可以用数据线连接设备，然后用电脑上的 Chrome DevTools 来调试移动设备上的页面。

**第一步：打开移动端开发者模式**

在安卓4.0以后，为了防止手机使用者误操作，系统默认开发者模式都是关闭的。设置→关于手机→连击7次版本号，之后便会在手机设置中出现开发者模式。
进入设置→开发者选项→打开开发者选项，打开USB调试，通过数据线连接手机和电脑，部分机型连接电脑时，会提示安装驱动程序，安装驱动程序。在手机上打开任意一个网页，只要是webview形式的网页都是可以的

**第二步：在电脑上调试页面**

在电脑chrome浏览器地址栏输入：chrome://inspect/#devices 会看到如下情况(如果没有显示设备信息，则表示没有连接好，可以拔插手机或关闭模式重开一下，但是部分华为的机型能看到设备名，无法看到webview中打开的网页，原因不明。据测试最好用的调试机是小米)


找到准备调试的页面，确保手机上该页面也是处于激活状态，点击蓝色的 inspect 链接，弹出一个新的窗口：


**注意 ：第一次使用chrome://inspect/#devices时，打开的页面是空白，其实是chrome需要下载一些工具才能支持，这些工具国内网是无法访问的，需要翻墙。但是只有第一次才需要，以后有了缓存就不要了，除非你的缓存被清掉。**

 接下来就可以像在PC机一样调试页面了。
 
#### IOS的真机调试
首先你得有台苹果电脑，没有苹果电脑，那你可以跳过这一节了。

iPhone 等一系列苹果设备对前端还是相当友好的，性能够好，Safari 浏览器也是不错，型号固定统一，问题也比较好解决，苹果公司也提供了一系列开发者工具，但有些手势操作测试以及最真实的用户体验测试还是需要真机。Safari 调试真机上的网页也是非常简单的。

首先需要在 iPhone 等设备上设置一下 Safari 浏览器，开启调试功能。

具体步骤：设置→Safari→高级→打开”web检查器“。
使用数据线连接电脑，Safari默认是隐藏开发选项的，在第一次使用的时候，需要在 MAC上打开Safari的开发菜单：顶部菜单栏“Safari”→偏好设置→高级→打开”在菜单栏中显示“开发”菜单
在iOS设备上的Safari浏览器中打开要调试的页面
切换到MAC的Safari，在顶部菜单栏选择“开发”→找到你的iOS设备名称→右边二级菜单选择需要调试的对应标签页，即可开始远程调试
在设备上用 Safari 浏览器打开需要调试的页面，之后在桌面版的 Safari 开发选项中即可看到进行调试，跟用chrome devTools差不多。


#### 其它调试工具
Fiddlerr抓包

Fiddler是一个抓包工具，可以将网络传输发送与接受的数据包进行截获、重发、编辑、转存等操作，也可以用来检测网络安全。个人认为更适合非前端人员抓包APP数据，而前端需要调试的内容更多，不太建议采用这种模式调试，更详细的抓包教程：https://www.cnblogs.com/miantest/p/7289694.html


### 二、JSBridge联调接口
JSBridge联调接口，基本集中在检查入参和回参是否符合要求。建议在调用JSBridge前打console输出调用参数，拿到JSBridge中的返回，输出回参，即可查出入参和回参是否正确，再找对应的app开发人员配合解决。

建议调用JSBridge的方法写成Promise形式，统一封装，如调用扫码接口scanCode写成统一的调用方式，可根据不同的平台，如公司自研APP、公众号、UC浏览器，根据运行的平台调用对应的JSBridge，这样页面适配性更高，即可实现一套H5页面实现多端底层接口调用，迁移成本也很小。

调用底层JSBridge实现思路大致如下：
```js
// 调用方法
support.scanCode().then(function(result){
    // 执行扫码后回调
});


//底层调用方法
class support{
    constructor(){
        // 初始化运行环境，判断当前平台
        // 异步引入平台JSBridge,如微信公众号
        let environmentName = JRE();// 判断当前平台
        switch (environmentName) {
            case ENVIRONMENT.WEIXIN_CLIENT:
                //异步加载微信JSBridge
                ...
                weixinClient.init(weixinUrl).then(function(wxResult){
                    this.bridgeName = 'weixin';
                    this.bridge = wx;               
                }).catch(function(error){
                    console.log('微信公众号调取失败',error)
                });
                break;
           case ENVIRONMENT.APP_CLIENT:
                //异步加载APPJSBridge
                ...
           default:
                //默认加载高德地图
                ...
         }       
    }
    
    function scanCode( ){
        return new Promise(function(resolve, reject){
            switch (this.bridgeName) {
                case ENVIRONMENT.WEIXIN_CLIENT:              
                     resolve(weixinClient.scanCode());
                case ENVIRONMENT.APP_CLIENT:
                     ...
                default:
                     resolve({
                         errorMessage:'更多内容请下载APP'
                     });
                } 
        })
    }    
}
    
class weixinClient{
    ...
    function scanCode() {// 调用微信扫码接口
        return new Promise(function(resolve, reject) {            
            this.bridge.scanQRCode({
                needResult: 1,
                success: function(res) {
                    resolve(res);
                }
            });            
        });
    }
}
```
其它思路：上述思路能解决多平台的问题，虽然底层代码是异步调用的，但打包出来依然有各平台的判断代码遗留，如果能在打包代码时，根据环境来引入对应代码，连判断都不需要就更好了，以后有机会实践看看是否可行。

### 三、兼容问题

移动端常见兼容问题：
- 安卓和IOS机型部分事件处理机制不一样
- 低端机型不支持ES6和fetch等语句

1. 安卓与ios常见的不同事件处理机制

- IOS中click事件延迟300ms响应
> 最简单的处理方案：判断当前运行设备，如果是IOS用touchend替代click，然后preventDefault来阻止默认行为click


- IOS中input键盘事件keyup、keydown、keypress支持不是很好
> 解决方案：判断当前运行设备，如果是IOS用onInput事件处理操作

- IOS中的DIV的click事件无效 
> 最简便的处理方案：给目标元素加一条样式规则 cursor: pointer

2. 低端机型不支持ES6和promise等语句

- 页面在多数移动端中能正常打开，但在部分低端机，如iphone6s、iphone6中页面没有正确渲染，出现白屏的情况，这个时候连接真机调试，能看到iOS Safari 报错
```js
SyntaxError: Unexpected keyword 'const'. Const declarations are not supported in strict mode.
```
这就是低端机型不支持es6。
> 解决方案：安装以下插件
>
> babel-preset-es2015, babel-polyfill, es6-promise



### 四、如何判断问题由webview引发
我前面戏称这是一篇甩锅教程，其实解决问题的思路都是先查己方问题，确定不是己方问题，再排查是由哪一方引起的。

在工作中，有些时候APP端没有处理好webview，也会引起H5页面bug。H5页面要运行在多平台，如公众号、安卓、IOS等，如何确定和归纳问题是属于webview引起的，应该由和你配合的APP开发人员去处理呢，排查思路如下：

首先，这类bug不是通用性的，如果一个bug在所有平台中都暴露，这一定是你的问题。这类bug是在特定的容器中暴露，在其它容器中正常。
- 假设A页面，经测在chrome打开正常
- 排查在安卓和IOS中是否正确渲染
- 如安卓正确，IOS不正常，打开IOS的浏览器（非APP中的）排除页面是否能正确打开，如查可以再要看APP中的页面。
- 采用排除法，确定只有在IOS中运行的APP中有问题，基本就是IOS端APP的webview问题。但是，有时候你还是需要百度一下这样有可能会造成的原因，这样才能更好的去找对接的APP开发人员沟通。


结语：

这个系列写到这，基本结束了，有些部分因手上没有实际的项目可供参考就没有整理进来，会在以后的工作中慢慢完善进来。基本上都是一些工作中的总结，如有错漏，欢迎交流指点。

