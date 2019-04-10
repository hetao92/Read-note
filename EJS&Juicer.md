##EJS 常用标签
`<% %>`流程控制标签
`<%= %>`输出标签 将转义后的值输出至模板
`<%- %>`输出标签 输入未被转义的值至模板（HTML会被浏览器解析）
`<%# %>`注释标签
`<%%`输出字面上的`<%`
`%` 对标记进行转义
`<%_` 删除所有前面的空格
`-%>`去掉尾随的换行符
`_%>` 删除所有尾随空格

##EJS成员函数
`Render(str,data,[option])`:直接渲染字符串并生成html
str：需要解析的字符串模板
data：数据
option：配置选项

`Compile(str,[option])`:编译字符串得到模板函数
str：需要解析的字符串模板
option：配置选项

Option [object]:
`Cache` [boolean]:是否缓存
`Filename` [string]:缓存名称，用作缓存的 key
`Context` [object]:执行上下文函数
`compileDebug` [boolean]:标示是否编译 debug，若为 true，则显示堆栈信息
`Client` [boolean]:指示是否在客户端执行，若为 true，则返回函数
`Delimiter` [string]:指定分隔符。默认`%`
`Debug` [boolean]:标示是否设置为 debug 状态，若 `true` 则会在控制台显示生成函数源码
`_with` [boolean]:是否使用`with(){}`函数，若为 false，数据将会保存在 locals 变量中



###包含文件
`<%- include(path) %>`
因为文件内有原始输出标签，所以为避免双重转义，用`<%-`

###过滤器
`<%=:data | filter%>`注意有`:`和`|`
filter：
`first` 返回数组的第一个元素
`last`  返回数组的最后一个元素
`capitalize` 返回首字母大写的字符串
`downcase`  返回字符串的小写
`sort`  排序
`sort_by:'prop'`    按照指定的 prop 属性进行升序排序
`size`  返回长度 length 属性
`plu:n` 加 n，转换为 Number 进行运算
`minus:n`   减去 n
`times:n`    乘以 n
`divided_by:n`   除以 n
`join:'val'`     数组以 val 为分隔符合并为新字符串
`truncate:n`    截取前 n 个字符，超过长度则返回一个副本
`truncate_words:n`  取得字符串中前 n 个 word
`replace:pattern,substitution`字符串替换，substitution不提供将删除匹配的子串
`prepend:val`   如果操作数为数组则进行合并，为字符串则在前面添加 val
`append:val`     如果操作数为数组则进行合并，为字符串则添加 val 在后面
`map:'prop'`    返回对象数组中属性为 prop 的值组成的数组
`reverse`   翻转数组或字符串
`get:'prop'`    取得属性为 prop 的值
`json`  转化为 json 格式字符串