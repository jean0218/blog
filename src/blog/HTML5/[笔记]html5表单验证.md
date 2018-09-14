## 新增种类
- url 要求用户必须输入URL格式。
- email 要求用户必须输入email格式。
- date 输入日期时弹出日期选择器。
- time 专门用来输入时间的。
- datetime 专门用来输入UTC日期和时间的。
- datetime-local 专门用来输入本地日期和时间的。
- month 专门用来输入月份的。
- week 专门用来输入周号的。
- number 只允许输入一段范围内数值的文本框。min~max与step属性一次增减多少。
- range 与Number类似 区别为显示为一个可拖动的横线。
- search 用于做搜索框。没有特殊样式。
- tel 专用于电话。没有特殊验证。
- color 颜色选择器。
- file 可以指定multiple属性实现多文件上传。

## 新增属性
- form属性
 HTML4中表单元素必须放在表单中才可以提交，HTML5中可以指定表单元素的form属性为form的ID，从而实现将表单元素放在任意位置。
- formaction属性 如上所述，表单元素可以action到各个页面中，不再是单一action。
- formmethod属性 如上所述，表单元素可以任意设置method属性为post或get。
- placeholder属性 用作type=text 或 textarea 中未输入文字时的提示文字。
- autofocus属性 用作页面打开时，默认获得焦点。
- list属性 input type=text 设置list属性的值为datalist元素的id。再将datalist元素设置为隐藏，实现类似百度搜索效果。
- autocomplete属性 配合list属性使用，设置autocompelte属性时有三种值""为默认，on时可显式指定，off隐式。
- HTML5表单验证
- required属性 除了隐藏元素大多可以指定required属性 提示必填内容。
- pattern属性 允许写入正则表达式。
- min~max属性 限制了输入的范围。
- step属性 增加或减少的间隔，比如一次为5个数值。
- checkValidity方法 每个表单元素都包含一个checkValidity方法用来将验证结果返回。!email.checkValidity()不通过。
- HTML5表单取消验证
- novalidate属性 全局失效 在Form上设置novalidate属性时为关闭表单验证。反之则为开启验证。novalidate="novalidate"
- formnovalidate 单一失效 在表单元素中单一设置formnovalidate时则单独关闭某个元素验证。
- HTML5自定义错误信息
- setCustomValidity方法用来自定义错误信息。配合checkValidity方法使用。
- if (!mail.checkValidity()) { mail.setCustomValidity("邮箱不正确哦~");