节点关系（属性）
childNodes[i]  子节点，
                        保存着nodeList对象。NodeList是一类数组对象，用于保存一组有序的节点。
                        如:someNode.childNodes.item(1);
                        someNode.childNodes.length;                 
parentNode  父节点
previousSibling  前一个兄弟节点
nextSibling   后一个兄弟节点
firstChild    第一个子节点
lastChild    最后一子节点

操作节点（方法）
必须是节点，与document.createElement("div");配合使用
appendChild()  插入后成为最后一个子节点
insertBefore()   插放到指定节点前，接收两个参数，要插入的节点和作为参照的节点，如：someNode.insertBefore(newNode,someNode.lastChild)
replaceChild()   替换节点，接收两个参数：要插入的节点和要替换的节点。如someNode.replaceChild(newNode,someNode.lastChild);
removeChild()   移除节点
前四个方法操作的都是某个子节点，操作前必须要取得父节点。
cloneNode()    创建副本，参数为true，复制节点及其整个子节点树，flase,只复制节点本身。复制后的节点属于文档所有，但是没有父节点，需要加入到文档中，才有父节点。
                        cloneNode()方法不会复制javascript属性，如事件处理程序等，IE有BUG,会复制事件处理程序，建议在复制前最好移除事件处理程序。
normalize()    处理文档树中的文本节点

document类型(属性)
document.doucumentElement    document.childNodes[0]    document.firstChild    三者值相机，都指向<html>元素
document.body    指向<body>元素
document.doctype    取得对<!DOTYPE>的引用，因浏览器对该属性的支持不一致，因此该属性的用处很有 限。
documnet.title     指向文档中的title属性，当前页面的标题
document.URL    取得完整的URL，地址栏显示 的URL    只读
documnet.domain    取得域名    可设置    当页面中包含来自其他子域的框架或内嵌框架时，设置该属性就非常方便，可以互相对方包含的JS对象
document.referrer    取得来源页面的URL    只读
    
查找元素（方法）
getElementById()    
                IE7中当name与ID名重名时，会将name当作ID用
getElementByTagName()    返回包含零或多个元素的NodeList.获取到的是个集合，可以用方括号语法或item()方法来访问,如：images.lenght,images[0].src,images.item(0).src
                                              还有namedItem(),使用这个方法可以通过元素的name特性取得集合中的项。如images.namedItem(nameMing),或images[nameMing]
getElementByName()    返回带有给定name特性的所有元素，最常用的是取得单选按钮，返回NodeList,集合

特殊集合
document.anchors    所有带name特性的<a>元素
document.forms        包含文档中所有的<form>元素
document.images      包含文档中所有的<img>元素
document.links          包含文档中所有带href特性的<a>元素

文档写入
document.write()        字符串参数，即要写入到输出流中的文本，原样写入。也可动态的包含外部资源，如javaScript文件等。
                                     document.write("<script type=\"text/javascript\" src=\"file.js\"><\/script")
document.writeln()    在字符串末尾添加 一个换行符
document.open()       打开网页的输出流
document.close()        关闭网页的输出流

Element类型（属性）          用于表现HTML元素，提供了对元素标签名、子节点及特性的访问
nodeName/tagName        元素的标签名，如document.getElementById("aa").nodeName
                                            注意：为了能在XML或HTML中执行，最好在执行时，转换为小写，如： element.tagName.toLowerCase()
    1、HTML元素（IE8之前不能访问）
            id
            title
            lang    元素内容的语言代码，很少使用
            dir       语言的方向，值为ltr,或rtl
            className    类名
            value
    2、
取得特性
        getAttribute()            
        setAttribute()
        removeAttribute()
    3、attributes属性    常用于遍历元素的特性
getNamedItem(name)    返回属性等于 name的节点
removeNamedItem(name)        同removeAttribute() ，不同处，removeNamedItem()   返回表示被 删除特性的Attr节点
setNamedItem(node)    向列表中添加节点，以节点的NodeName属性为索引，不常用
item(pos)    返回位于数字pos位置处的节点
nodeValue     name的值 
                        element.attributes.getNamedItem("id").nodeValue;
                        element.attributes["id"].nodeValue    
                        elementattributes["id"].nodeValue = "someOtherId"
遍历元素的特性
        function    outputAttributes(element){
            var    pairs    = new Array(),
                    attrName,attrValue,i,len;
            for(i=0,len=element.attributes.length;    i<len;    i++){
                attrName = element.attributes[i].nodeName;
                attrValue = element.attributes[i].nodeValue;
               if(element.attributes[i].specified){
                    pairs.push(attrName + "=\"" + attrValue    +"\"");
                } 
            }
            return pairs.join(" ");
        }

创建元素
    document.creatElement()    参数 ，要创建的标签名
    在IE中    document.creatElement("<div id=\"abc\" ></div>")    这种方式有利于避开在IE7及更早版本动态创建元素的某些问题
    注意：不能设置动态创建的<iframe>元素的name特性
              不能通过表单的reset()方法重设动态创建的<input>元素
              动态创建的type特性值为"reset"的<button>元素重设不了表单
              动态创建的一批name相同的单选按钮彼此豪无关系。
            if(client.browser.ie && client.browser.ie<=7){
                document.creatElement("<div id=\"abc\" ></div>")//其它浏览器不支持这种用法
            }
元素的子节点
    for(var i=0,len=element.childNodes.length; i<len;i++){
        if(element.childNodes[i].nodetype == 1){//表示是元素节点
            
        }
}    
     获取标签名的子节点或后代节点
        var ul=document.getElementById("mylist");
        var items = ul.getElementsByTagName("li");

Text类型
        获取text element.nodeValue,element.data

    appendData(text)    将text添加到节点的末尾
    deleteData(offset,count)    从offset指定的位置开始 删除count个字符
    inserData(offset,text)    从offset指定的位置插入text
    replaceData(offset,coun,text)    用text替换从offset指定的位置开始到offset+count为止处的文本。
    substringData(offset,count)    提取从offset指定的位置开始到offset+count为止处的字符串
    length    字符数目

    document.cteateTextCode()    创建新文本节点，接收参数：文本
    normalize()    合并父元素的文本节点
    splitText()       按照指定的位置分割nodeValue的值 ，分割 成两个文本节点，原来的文本节点将包含从开始到指定位置之前的内容，新文本节点将包含剩下的文本。
                            返回 一个新文本节点，该节点与原节点的parentNode相同。


 Comment类型    注释类型
    document.creatComment()    创建 注释节点

documentFragment       
    文档片断不可能 直接添加到文档中，但可以作为一个仓库来使用，即可以在里面保存将来可能 会添加到文档中的节点。
    document.createDocumentFragment();    创建文档片断
    文档片断可以通过appendChild(）或insertBfefore(）添加到文档 中





