# Markdown编辑学习总结
November 29, 2017 7:51 PM
_ _ _
[TOC "float:left"]

## 总结内容
###1. Emphasis（强调）

- *斜体强调*
> **用法**：用于两个`*`之间。**注意**：两边各只有一个星号，即` *[content]* `。
**例如**：`* 这个是斜体 *`的显示结果为 ：* 这个是斜体 *。
- **粗体强调**
> **用法**：用一对`**`将内容包含起来。**注意**：两边各有两个星号，即` ** [content] **`。
**例如**：`** 这个是粗体 **`的显示结果为 ：**这个是粗体**。

###2. Links(链接)
- Inline
>**用法**：` [Link](http://example.com) `，中括号之间显示的是* 名称 *，圆括号里面的内容是链接（在文档中不显示）。
**例如**：`[Baidu](http://www.baidu.com/ "BAIDU")`的显示结果为： [Baidu](http://www.baidu.com/)

###3. Code(代码)
- Code span
> **用法**：\`[Codes]\`，用两个 \` 将内容包含起来。
**例如**：\`#include<stdio.h>\`的显示结果为：`#include<stdio.h>`

###4. Image(图片)
- Image
> **用法**：`![Link](http://example.com)`，中括号里面表示显示的*图片标题*，圆括号中表示图片的链接。
**注意**：注意将链接`[Link](http://example.com)`和`![Link](http://example.com)`区别开来，图片的格式中多一个`!`。
**例如**：`![haroopad icon](http://pad.haroopress.com/assets/images/logo-small.png)`的显示结果为：
![haroopad icon](http://pad.haroopress.com/assets/images/logo-small.png)

###5. Headers(标题)
- Headers
> **用法**：
 * 在*大标题头*正下方加入一行`=`，在*小标题头*正下方加入一行`-`。
 * 通过
    \# Header 1
    \## Header 2
    \### Header 3
    ……
    \###### Header 6
    表示，即，通过前面加的`#`的个数，最多叠加到6层。

 > **例如**：下面的例子：
   \# Header 1
    \## Header 2
    \### Header 3
    ……
    \###### Header 6
    显示的结果为：
    # Header 1
    ## Header 2
    ### header 3
    ……
    ###### Header 6

###6. Lists(列表)
- 有序列表
> **用法**：`1. `、`2. `、`3. `等空一个空格然后接后面的内容。
 **例如**：
 1. Red
 2. Blue
 3. Green

 >**注意**：`.`后面要有一个空格和后面的内容隔开
- 无序列表
> **用法**：`*`、`-`或`+`空一格后接上后面的内容
**例如**：
` * Red `的显示结果为:
  * Red
` - Red `的显示结果为:
  - Red
` + Red `的显示结果为:
  + Red

 > 三种用法的结果一样。

###7. Blockquotes(引用)
* 引用
> **用法**：`>`后面接引用的内容。
**例如**：`>This is a **Blockquotes**`的的显示结果为

> This is a **Blockquotes**

###8. Horizontal Rules(分隔线规则)
- 分隔线
> **用法**：使用三个或更多的`-`、`*`或`_`来进行分隔。
**例如**：
	+ `* * *`的显示结果为：
	* * *
	+ `- - -`的显示结果为：
	- - -
	+ `_ _ _`的显示结果为：
	_ _ _
**注意**：分隔线的那一行不能够有除了空格外的其他字符。

###9. Table(GFM)
- 表格（GitHub支持语法）
> **例子**：下列例子
```
| name | age | gender | money |
|------|-----|--------|-------|
| sdfd | sad | fdvfd  | 4554  |
| asdv | 34  | dadsv  | dsa   |
```
的结果为：
| name | age | gender | money |
|------|:---:|:-------|------:|
| sdfd | sad | fdvfd  | 4554  |
| asdv | 34  | dadsv  | dsa   |
**注意**：第二行中的冒号的意义：如果为`:----:`形式，那么该列的数据均居中；如果为`:----`形式，那么该列的数据均靠左显示；如果为`-----:`形式，那么该列数据均靠右显示。

###10. 删除线、Code显示块
- 删除线
> **用法**：` ~~[Content]~~`，用两对`~~`将内容包含。
**例如**：` ~~删除该内容~~`的显示结果为：~~删除该内容~~。
- Code显示块
> **用法**：用两个 \``` 将code内容包含。
**例如**：
>\```
>void main()
>{
>	int i;
>	int y;
>}
>\```
的显示结果为:
```
void main()
{
	int i;
    int y;
}
```

###11. 高亮强调、目录
- 高亮强调
> **用法**：`==[Content]==`，用`==`将内容包含起来。
**例如**：下列例子：
`==高亮显示该句子==`
的显示结果为：
==高亮显示该句子==
- 目录
> **用法**：`[TOC "float:left"]`或者`[toc "float:right"]`。
**注意**：后面双引号中的left和right表示该目录向左对齐还是向右对齐。

###12. Multimarkdown
- 右上角标
> **用法**：用一对`^`将内容包含起来。
**例如**：以下例子：
`右上角标^右上角标^`
的显示结果为：
右上角标^右上角标^
- 右下角标
> **用法**：用一对`~`将内容包含起来。
**例如**：以下例子：
`右下角标~右下角标~`
的结果为：
右下角标~右下角标~
- 下划线
> **用法**：用一对`++`将内容包含起来。
**例如**：以下例子：
`++下划线的句子++`
的显示结果为：
++下划线的句子++

###13. 流程图
- 流程图
> **简单例子**：
```
```mermaid
graph TD;
  A-->B;
  A-->D;
  B-->C;
  D-->C;
``````
的显示结果为：
```mermaid
graph TD;
  A-->B;
  A-->D;
  B-->C;
  D-->C;
```

###14. 任务列表（多用于待办事项或检查表）
- 任务列表
> **简单例子**:
>```
>- [ ] first task
>- [x] second task
>- [ ] third task
>```
>的显示结果为：
> - [ ] first task
> - [x] second task
> - [ ] third task

###15. 展示
- 展示
> **用法**：使用`***`或者`---`将几个页面分开。
**例如**：
> ```
>## slide1 title
>slide1 content
>***
>## slide2 title
>slide2 content
>```
>的显示结果为：
>## slide1 title
>slide1 content
>***
>## slide2 title
>slide2 content

###16. 脚注
- 脚注
> **用法**：在正文中使用`[^Mark]`进行标记，在结尾的时候，使用`[^Same-Mark]:explanatory information`进行脚注。
**例如**：
>```
>This is footnote[^1]
>
>[^1]:explanation information
>```
>的显示结果为：
>This is footnote[1]
>
>[1]:explanation information

