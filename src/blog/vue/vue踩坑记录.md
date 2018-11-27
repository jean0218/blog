# vue踩坑记录
### 绑值
绑定值
``` js
{{message}}
```
绑定属性值
``` js
v-bind:title=""
```
注意：如果绑定的值是字符串，记得再加'',如"'vue'"

### 数组生成生表格，每隔七列换行显示
VUE一般使用template来创建HTML，查了下template没有语法可以实现这种情况。查看了element的源码，它先把数组先处理成行，再循环行中的列。这个写法还是觉得复杂了。可以使用javascript来创建html，这时候我们需要使用render函数，采用jsx来实现：

```
render(h) {
        const { list, defaultValue } = this;
        let tdDom = [], thisDom = [];
        return (
            <div class="calendar-item">
                <div class="calendar-item-header">{defaultValue.y}年{defaultValue.m + 1}月</div>
                <table class = "calendar-table">
                    <thead>
                        <tr>
                              <th>日</th>
                              <th>一</th>
                              <th>二</th>
                              <th>三</th>
                              <th>四</th>
                              <th>五</th>
                              <th>六</th>
                          </tr>
                      </thead>
                      <tbody>
                          {
                              list.map(function(item, index){
                                  tdDom.push(<td><span class={item.class} data-value={item.value}>{item.text}</span></td>)
                                  if(index !== 0 && (index + 1) % 7 === 0){
                                      thisDom = tdDom;
                                      tdDom = [];
                                      return(
                                          <tr>
                                              {thisDom}
                                          </tr>
                                      )
                                  }
                              })
                          }
                      </tbody>
                  </table>
            </div>
        )
    }
   ```
  其中无论是props传下来的属性，还是data中自带的属性值，统一通过this获取，不需要像react区分this.props,this.state


坑备注：在写日历组件返回下一月的日历组件时，单元测试通过，但是在vue组件无法更改单元测试的组件，除非用把当前组件深拷贝一份，
这个究竟是vue的bug还是方法上面的bug呢？ 待论证。

### scoped属性
在vue组件中，在style标签上添加scoped属性，以表示它的样式作用于当下的模块，很好的实现了样式私有化的目的，这是一个非常好的机制。但是为什么要慎用呢？在实际业务中我们往往会对公共组件样式做细微的调整，如果添加了scoped属性，那么样式将会变得不易修改。


### 查看dom渲染了几次
在react中养成的习惯，代码写完后，查看dom渲染了几次，是否有多余的渲染。在vue中想找对应的生命周期来实现，结果发现vue中多数用的template渲染模板，很少用render函数，不能在render中输出语句来判断是否渲染了图层，可在mounted和updated中输出查看。

疑问：初次渲染时，为什么mounted和updated都会更新一次，待解决。

### vue-cli编译后，刷新页面出现webpackJsonp is not defind

问题的起因是这样的，有天在前端群里，无意中看到有人咨询vue-cli编译的文件出现webpackJsonp is not defind报错，由于以前在配置react项目引入的JS顺序不正确会出现这样的报错，就随意提了几句，对方还是没解决。
今天又有个朋友遇到了这样的报错，叫我帮他瞧一瞧。想着奇怪了，vue-cli编译的文件经常出现这个报错吗？那我们就来分析一下这个BUG吧。

我先复现一下BUG：

A网站，从首页a.com进入登录页a.com/login,正常，能正确打开页面
刷新login路由就出现报错：

![image](./images01.png)

重新加载页面查看生成的html结构，a.com 页面：(请忽略前面的JS，这项目不是我配置的....)

![image](./images02.png)

加载a.com/login,再刷新，js显示如下：

![image](./images03.png)

仔细查看加载的js，a.com/login 路由下只加载了当前路由的js，公共的JS没有了。


我们来分析一下引起这个问题，该问题出现在生产环境，我们需要更改的文件就是在webpack.prod.config.js。有可能出现在哪些配置项
CommonsChunkPlugin：配置按需加载
HtmlWebpackPlugin：模板生成单页面或多页面

查看了下两者配置：
```js
    new HtmlWebpackPlugin({
      filename: process.env.NODE_ENV === 'testing'
        ? 'index.html'
        : config.build.index,
      template: 'index.html',
      inject: true,
      minify: {
        removeComments: true,
        collapseWhitespace: true,
        removeAttributeQuotes: true
        // more options:
        // https://github.com/kangax/html-minifier#options-quick-reference
      },
      chunks: ['manifest', 'vendor', 'app'],
      // necessary to consistently work with multiple chunks via CommonsChunkPlugin
      chunksSortMode: 'dependency'
    }),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      minChunks (module) {
        // any required modules inside node_modules are extracted to vendor
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          module.resource.indexOf(
            path.join(__dirname, '../node_modules')
          ) === 0
        )
      }
    }),
    // extract webpack runtime and module manifest to its own file in order to
    // prevent vendor hash from being updated whenever app bundle is updated
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest',
      minChunks: Infinity
    }),
    // This instance extracts shared chunks from code splitted chunks and bundles them
    // in a separate chunk, similar to the vendor chunk
    // see: https://webpack.js.org/plugins/commons-chunk-plugin/#extra-async-commons-chunk
    new webpack.optimize.CommonsChunkPlugin({
      name: 'app',
      async: 'vendor-async',
      children: true,
      minChunks: 3
    }),
```
都没什么大问题，都是按的默认配置，再运行一下编译命令：npm run build
这个时候，就发现问题了，说好的SPA呢？为什么这么多的文件夹，每一个都有文件html文件

![image](./images04.png)

这些html文件是哪里来的，找啊找,在文件中找到了这么一段配置文件
```js
new PrerenderSPAPlugin({
      //将渲染的文件放到dist目录下
      staticDir: path.join(__dirname, '../dist'),
      //需要预渲染的路由信息
      routes: ['/',
        '/home',
       ...
      ],
      renderer: new Renderer({
        //在一定时间后再捕获页面信息，使得页面数据信息加载完成
        captureAfterTime: 50000,
        //忽略打包错误
        ignoreJSErrors: true,
        phantomOptions: '--web-security=false',
        maxAttempts: 10
      })
    })
```
百度了一下这个插件，对方想采用prerender-spa-plugin做预渲染，但是又采用了HtmlWebpackPlugin模板生成页面，两者之间的配置冲突了,注释其中一个就可以了。

### 感悟
学习vue的各种用法，再来看react的用法，其实他们的不同，很多来自dom实现的不同。甚至vue中也可以像react一样使用jsx模板语言实现，他们的事件触发后的处理语句写的位置是一样的。

两者都了解后，考虑问题的方式也逐渐开始跳出框架。像移动端的上滑下滑事件，这个无论是在哪个框架中，这个事情都需要由原生来处理。

原来接触vue的时候，觉得这玩艺耦合性低，入侵性也比较强，和react的思想相比，模板化不是很清晰，一度不想深入了解。最近参与了一些项目，让我认识到，无论哪个技术栈，只要能快速解决问题的都是好的。
vue和react两个可以深入了解。
