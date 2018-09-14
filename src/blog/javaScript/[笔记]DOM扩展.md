Selecte API 支持的浏览器IE8+,Firefox3.5+,Safari3.1+,Chorme,Opera10+
querySelector()    接收一个CSS选择符，返回 与该模式匹配的第一个元素
                             通过Document类型调用querySelector()方法时，会在文档 元素的范围内查找匹配的内容。通过element类型调用querySelector()方法时，只会在该元素后代元素的范围内查找匹配的元素。

querySelectorAll()    接收一个CSS选择符，返回 一个NodeList的实例
                             调用的类型包括Document,DocumentFragment,Element
                             要取得NodeList中的每一个元素，可以使用item()，或方括号语法

matchesSelector()   接收一个参数css选择符，如果调用元素与该选择符匹配，返回true, 否则，返回false(支持的浏览器IE9+,Firefox3.6+,Safari5+)，最好是编写一个包装函数。

元素遍历
      对于元素间的空格，IE9及之前版本不会返回文本节点，而其他所有浏览器都会返回文本节点。
childElementCount    返回子元素（不包括文本节点和注释）的个数
firstElementChild         指向第一个子元素：firstChild的元素版
lastElement                  指向最后一个子元素；lastChild的元素版
previousElemetnSibling    指向前一个同辈无毒 previousSibling的元素版
nextElementSibling    指向后一个同辈 元素；nextSibling的元素版
支持Element    Traversal规范的浏览器有IE9+,Firefox3.5+,Safari4+,Chrome和Opera10+

HTML5
    class可以用来表示元素的语义

getElementsByClassName    接收一个参数 ，即一个包含一或多个类名的字符串，返回带有指定类的所有元素的NodeList
       可以通过document对象及所有HTML元素调用该方法
        IE9+,Firefox3+,Safari3.1+,Chrome和Opera9.5+
className属性，添加、删除和替换类名
//删除user类

//取得类名字符串并拆分成数组
var classNames = div.className.split(/\s+/);
//找到要删除的类名
var pos = -1,i,len; 
for(i=0,len=classNames.length;i<len;i++){
    if(classNames[i] =="user"){
        pos=i;
        break;
    }
}
//删除类名
classNames.splice(i,1);
//把剩下的类名拼成字符串并重新设置
div.clasName =classNames.join("");


classList属性
    要取得 每个元素可以使用item()方法，或方括号语法，还包括如下方法
   add(value):将给定的字符串值添加到列表中。如果值已经存在，就不添加 了
   contains(value):表示列表中是否存在给定的值，如果存在则返回true,否则返回false.
   remove(value):从列表中删除给定的字符串。
   toggle(value):如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加 它。
支持的浏览器Firefox3.6和Chrome.

焦点管理
document.activeElement属性，这个属性始终引用DOM中当前获得了焦点的元素。元素获得焦点的方式有页面加载、用户输入和在代码中调用focus()方法
默认情况下，文档刚刚加载完成时，document.activeElement中保存的是document.body元素的引用。文档加载期间，document.activeElement的值为null.

document.hasFocus()，用于确定文档是否获得了焦点
document.readyState 实现指示文档已经完成的指示器 值loading正在加载文档 ，complete已经加载完成文档 
document.compatMode    告诉开发人员浏览器采用了哪种渲染模式
document.head    引用head无素
                var head = document.head || document.getElementByTagName("head")[0];

自定义数据属性
HTML5规定可以为元素添加非标准的属性，但要添加前缀data-
dataset    属性访问自定义属性的值 。
                映射在html中，data-myname,JS中，就是dataset.myname
支持的浏览器Firefox6+和chorme

插入标记
innerHTML
    限制：在大多数浏览器中，通过 innerHTML插入<script>元素并不会执行其中的脚 本。
    不支持innerHTML的元素：col,colgroup,frameset,head,html,style,table,tobdy,thead,tfoot,tr,此外，在IE8及更早版本中，<title>元素也没有innerHTML属性。
    无论什么时候，只要使用innerHTML从外部插入HTML，都应该首先以可靠的方式处理HTML。IE8为止提供了windo.toStaticHTML()方法，这个方法接收一个参数，即一个HTML字符串，返回 一个经过无害处理后的版本，从源HTML中删除所有脚本节点和事件处理程序属性。

outerHTML
    支持的浏览器IE4+,Safari4+,Chrome,Opera8+,Firefox7+.
insertAdjacentHTML()方法
    接收两个参数：插入位置和要插入的HTML文本。第一个参数必须是下列值之一：
        beforebegin，在当前元素之前插入一个紧邻的同辈元素
        afterbegin，在当前元素之下插入一个新的子元素或在第一个子元素之前再插入新的子元素
        beforeend，在当前元素之下播放一个新的子元素或在最后一个子元素之后再插入新的子元素
        afterend，在当前元素之后插入一个紧邻的同辈元素。
内存与性能问题
    在使用innerHTML,outerHTML和inserAdjacentHTML()方法时，最好先手工删除要被替换的元素的所有事件处理程序 和javascript对象履性。

scrollIntoView()    通过滚动浏览器窗口或某个容器元素，调用元素就可以出现在礼品中。如果给这个方法传入true作为参数，或者不传入任何参数 ，那么窗口滚动之后会让调用 元素的顶部与视口顶部尽可能齐平。如果传入false作为参数 ，调用元素会尽可能全部出现在视口中。

contains()方法
    不通过 在DOM文档 树中查找即可获得这个信息，调用contains()方法的应该是祖先节点 ，也就是搜索 开始 的节点，接收一个参数，即要检测的后代节点。如果 被检测的节点是后代节点，返回true，否则返回flase
    支持的浏览器：IE,Firefox9+,Safari,Opera和Chrome
compareDocumentPosition()    确定节点间的关系，返回 珍上表示该关系的位掩码
        1    无关（给定的节点不在当前文档 中）
        2    居前（给定的节点在DOM树中位于参考 节点之前）
        4    居后（给定的节点在DOM树中位于参考节点之后）
        8    包含（给定的节点是参才节点的祖先）
        16  被包含（给定的节点是参考节点的后代）

支持的浏览器：IE9+,Firefox,Safari,Opera9.5+,和Chrome 
