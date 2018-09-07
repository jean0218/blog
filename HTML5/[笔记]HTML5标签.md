全局属性
    是指对任何元素都使用的属性。

    contenetEditable
   允许用户编辑元素中的内容，该元素必须是可以获得鼠标焦点的元素。是一个布尔值属性，true或flase。
   隐藏的inherit(继承)状态，属性为true时，允许编辑。false不允许编辑 。未指时，由inherit状态决定 。
    isContentEditable属性，当元素可编辑时，该 属性为true,当元素不可编辑时，属性为false.
   编辑完内容的，如想保存，只能把该元素的innerHTML发送到服务器端保存。无其它API来保存编辑后元素中的内容。

    designMode
    指定整个页面是否可编辑 。 只能在Javascript脚本里被编辑修改。有on和off两个值 。为on时可编辑，off时不可编辑 。
    使用JS指定属性的方法如下：document.designMode="on"
    
    hidden
    所有元素都支持。布尔值属性。true和false。

    speelcheck
    针对input和textarea提供的新属性，对用户输入的文本进行拼写和语法检查。布尔值属性。
    如果元素的readOnly和disabled值为true，则不执行检查。


HTML5的结构 
    article
    代表文档、页面或应用程序中独立 的、完整的，可以独自被外部引用的内容。
    一般有自己的标题，放在header里面
<article>
    <header>
        <h1>我是大标题</h1>
    </header>
    <p>............</p>
</article>
<article> 标签规定独立的自包含内容。
一篇文章应有其自身的意义，应该有可能独立于站点的其余部分对其进行分发。
<article> 元素的潜在来源：
论坛帖子
报纸文章
博客条目
用户评论
   
section
    对网站或页面上的内容进行分块。通常由内容及标题组成。section元素中的内容可以单独存储到数据库中或输出到word文档中。
    不推荐没有标题的内容使用section。
<section>
  <h1>PRC</h1>
  <p>The People's Republic of China was born in 1949...</p>
</section>
<article> 标签定义外部的内容。
外部内容可以是来自一个外部的新闻提供者的一篇新的文章，或者来自 blog 的文本，或者是来自论坛的文本。亦或是来自其他外部源内容。

    一段独立 的、完整的内容，使用该元素。对文章分段的工作也是使用section。
    
    article与section的区别
    artice强调独立性，section强调分段或分块。
    
aside为语义化标签
通常用来描述与文档主体内容不相关的内容
比如，博客文章用atricle标签，而博客旁边的文章信息栏(作者头像、博文分类、作者等级等于博客正文内容无关的)用aside
 
  注意：不要将section用作设置样式的页面容器，那是div的工作
              如果 artice、aside或nav更符合使用条件 ，不要使用 setion.
              不要为没有标题的内容区块使用setion

    nav
    用于以下场合：传统导航条，网站上不同层级的导航条。
                            侧边栏导航
                            页内导航 
                            翻页操作
    
    aside
     表示当前页面或文章的附属信息部分。使用方法：
              被包含在article中作为主要内容的附属信息。
              在article之外，作为页面或站点全局的附属信息部分。如侧边栏，内容可以是友情链接，博客中其他文章的列表、广告信息等。    




表单与文件
    
    文件API
    FileList对象与file对象
    filelist 表示用户选择的文件列表，有两个属性，name 表示文件名，不包括路径 。lastModifiedDate表示文件最后修改的日期。
    
图形元素(The Figure Element )
文字裹在p标签里，与img标签各行其道，很难让人联想到这就是标题。HTML5通过采用<figure>元素对此进行了改正。当合<figcaption>元素组合使用时，我们就可以语义化地联想到这就是图片相对应的标题
<figure>
    <img src="path/to/image" alt="About image" />
    <figcaption>
        <p>This is an image of something interesting. </p>
    </figcaption>
</figure>

IE和HTML5(Internet Explorer and HTML5)
不幸的是，讨厌的IE浏览器需要动点小手术才能理解新的HTML5元素。
所有元素，默认的，都有个inline的display
为了确保所有新的HTML5元素能以block水平的元素正确地渲染，有必要对其做如下定义：
header, footer, article, section, nav, menu, hgroup {  
    display: block;  
} 


必要的属性(Required Attribute )
表单允许新的必要属性，用来指定是否需要特殊的input。这取决于你的代码偏好，你可以以下面两种方式之一申明此属性。
<input type="text" name="someInput" required>  

或者，使用更结构化的方法：
<input type="text" name="someInput" required="required"> 
此属性还可以用在CSS中，例如下面这个有些傻里傻气的CSS文字改变的例子：
CSS代码：
.data_custom { display:inline-block; position: relative; }
.data_custom:hover { color: transparent; }
.data_custom:hover:after {
    content: attr(data-hover-response);
    color: black;
    position: absolute;
    left: 0;
}

