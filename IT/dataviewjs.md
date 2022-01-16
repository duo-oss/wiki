# dateviewjs
[github地址](https://github.com/blacksmithgu/obsidian-dataview)
[query文档](https://blacksmithgu.github.io/obsidian-dataview/query/queries/)
[dataview字段](https://blacksmithgu.github.io/obsidian-dataview/data-annotation/)
#### Implicit Fields

Dataview automatically adds a large amount of metadata to each page:

-   `file.name`: The file title (a string).
-   `file.folder`: The path of the folder this file belongs to.
-   `file.path`: The full file path (a string).
-   `file.link`: A link to the file (a link).
-   `file.size`: The size (in bytes) of the file (a number).
-   `file.ctime`: The date that the file was created (a date + time).
-   `file.cday`: The date that the file was created (just a date).
-   `file.mtime`: The date that the file was last modified (a date + time).
-   `file.mday`: The date that the file was last modified (just a date).
-   `file.tags`: An array of all unique tags in the note. Subtags are broken down by each level, so `#Tag/1/A` will be stored in the array as `[#Tag, #Tag/1, #Tag/1/A]`.
-   `file.etags`: An array of all explicit tags in the note; unlike `file.tags`, does not include subtags.
-   `file.inlinks`: An array of all incoming links to this file.
-   `file.outlinks`: An array of all outgoing links from this file.
-   `file.aliases`: An array of all aliases for the note.
-   `file.tasks`: An array of all tasks (I.e., `- [ ] blah blah blah`) in this file.


#### task  Implicit Fields

As with pages, Dataview adds a number of implicit fields to each task:

-   Tasks inherit _all fields_ from their parent page - so if you have a `rating` field in your page, you can also access it on your task.
-   `completed`: Whether or not this _specific_ task has been completed.
-   `fullyCompleted`: Whether or not this task and **all** of its subtasks are completed.
-   `text`: The text of this task.
-   `line`: The line this task shows up on.
-   `path`: The full path of the file this task is in.
-   `section`: A link to the section this task is contained in.
-   `link`: A link to the closest linkable block near this task; useful for making links which go to the task.
-   `subtasks`: Any subtasks of this task.
-   `real`: If true, this is a real task; otherwise, it is a list element above/below a task.
-   `completion`: The date a task was completed. If not annotated, will default to file modified time.
-   `due`: The date a task is due, if it has one.
-   `created`: The date a task was created. If not annotated, defaults to file creation time.
-   `annotated`: True if the task has any custom annotations, and false otherwise.


# Sources

A dataview "source" is something that identifies a set of files, tasks, or other data object. Sources are indexed internally by Dataview, so they are fast to query. Dataview currently supports three source types:

1.  **Tags**: Sources of the form `#tag`.
2.  **Folders**: Sources of the form `"folder"`.
3.  **Specific Files**: You can select from a specific file by specifying it's full path: `"folder/File"`.
4.  If you have both a file and a folder with the exact same path, Dataview will prefer the folder. You can force it to read from the file by specifying markdown: `folder/File.md`.
5.  **Links**: You can either select links TO a file, or all links FROM a file.
6.  To obtain all pages which link TO `[[note]]`, use `[[note]]`.
7.  To obtain all pages which link FROM `[[note]]` (i.e., all the links in that file), use `outgoing([[note]])`.

You can compose these filters in order to get more advanced sources using `and` and `or`.

-   For example, `#tag and "folder"` will return all pages in `folder` and with `#tag`.
-   Querying from `#food and !#fastfood` will only return pages that contain `#food` but does not contain `#fastfood`.
-   `[[Food]] or [[Exercise]]` will give any pages which link to `[[Food]]` OR `[[Exercise]]`.
---
## 数据视图 JS

dataview [JavaScript API](https://blacksmithgu.github.io/obsidian-dataview/api/intro)为您提供了 JavaScript 的全部功能，并提供了用于拉取 Dataview 数据和执行查询的 DSL，允许您创建任意复杂的查询和视图。与查询语言类似，您可以通过带`dataviewjs`注释的代码块创建 Dataview JS 块：

<pre>```dataviewjs
let pages = dv.pages("#books and -#books/finished").where(b => b.rating >= 7);
for (let group of pages.groupBy(b => b.genre)) {
   dv.header(group.key);
   dv.list(group.rows.file.name);
}

```</pre>   


在 JS dataview 块中，您可以通过`dv`变量访问完整的 dataview API 。有关您可以使用它做什么的说明，请参阅[API 文档](https://blacksmithgu.github.io/obsidian-dataview/api/code-reference)或[API 示例](https://blacksmithgu.github.io/obsidian-dataview/api/code-examples)。

`LIST FROM <source>` 
```
LIST FROM #games/mobas OR #games/crpg 
```

除了每个匹配文件之外，您还可以通过在 之后添加一个表达式来呈现单个计算值`LIST`：
`LIST WITHOUT ID<expression> FROM <source>`

```
LIST WITHOUT ID file.path FROM "4. Archive"
```




## Examples

*table*
Show all games in the game folder, sorted by rating, with some metadata:
<pre>
```dataview
table time-played, length, rating
from "games"
sort rating desc
```
</pre>

```dataview
table time-played, ctime, rating
from "工具"
sort rating desc
```

---
*list*
<pre>
```dataview
list from "工具"
```
</pre>
```dataview
list from "工具"
sort rating desc
```
---

<pre></pre>
```dataview
list from #算法 
```

---
*列出所有任务*

```dataview
task 
```
---
*列出 文件中包含某标签 的任务*
```dataview
task from #阅读  
```
---
*展示所有文件 的任务*
```dataviewjs
dv.taskList(dv.pages().file.tasks.where(t =>t));
```

*展示 ==没有完成== 的任务*

```dataviewjs
    dv.taskList(dv.pages().file.tasks.where(t => !t.completed));
	
```

*展示 ==已经完成== 的任务*

```dataviewjs
    dv.taskList(dv.pages().file.tasks.where(t => t.completed));
```

---
dateview 收集字段的两种方式
Examples of both are shown below:
```
// 在文章的头部
---
alias: "document"
last-reviewed: 2021-08-17
thoughts:
  rating: 8
  reviewable: false
---

# Markdown Page
// 可以在文章中

Basic Field:: Value
**Bold Field**:: Nice!

```


---

在 插件配置打开 行内 query 选项
文章中 使用 `=this.file.name`
We are on page `= this.file.name`

	We are on page dataviewjs
	
---
#算法 ee
#算法 44
This page was last modified at

`= this.file.mtime`

`= this.file.ctime`

---

```dataviewjs
for (let group of dv.pages().groupBy(p => p.folder)) { dv.header(3, group.key); dv.table(["Name", "folder", "Rating"], group.rows .sort(k => k.rating, 'desc') .map(k => [k.file.link, k.file.folder, k.file.path])) }
```

---
%%```dataviewjs
dv.list(dv.pages())```%%


%%
```dataviewjs
dv.list(dv.pages().file.link)
```
%%

---
%%
```dataviewjs
dv.list(dv.pages().file.link)
```
%%

%%
特定文件架下的 文件
```dataviewjs
dv.list(dv.pages().file.link)
```
%%

# file list
```dataviewjs
dv.list(dv.pages().file)
```


# name list 

```dataviewjs
dv.list(dv.pages().file.filter(t=>t.folder).name)
```
---


# link list 

```dataviewjs
dv.list(dv.pages().file.link)
```

# tags list 

```dataviewjs
 dv.list(dv.pages("#算法").file.link)
```

# 某个 文件夹 list 
```dataviewjs
 dv.list(dv.pages("#算法").file.link)
```

# 数组
```dataviewjs
  dv.list([1,3,4])
```

```dataviewjs
  dv.list([{a:1}.a,{b:8}.b,{c:66}.c])
```

```dataviewjs
  dv.list([{a:1,a2:1},{b:8,a2:1},{c:66}])
```

```dataviewjs
  dv.list([{a:1,a2:1},{b:8,a2:1},{c:66}].map(t=>t.a))
```

```dataviewjs
  dv.list([{a:1,a2:1},{b:8,a2:1},{c:66}].filter(t=>t.a))
```
## 可以使用 Object
```dataviewjs
  dv.list([{a:1,a2:1},{b:8,a2:1},{c:66}].map(t=>Object.keys(t)))
```
```dataviewjs
  dv.list([{a:1,a2:1,c3:5}].map(t=>Object.values(t)))
```



# 某 文件夹 下的文件
```dataviewjs
dv.list(dv.pages('"IT"').file.link)
```

# 某文件的 属性名
```dataviewjs
dv.list( Object.keys(dv.page('技术清单').file))
```
# 某文件的 属性值

```dataviewjs
dv.list( Object.values(dv.page('技术清单').file))
```

# 某文件的  link 的值
```dataviewjs
dv.list(Object.keys(dv.page('技术清单').file.link))
```
# 某文件的  link 的值  
```dataviewjs
dv.list([`dv.page('技术清单').file.link.path`,dv.page('技术清单').file.link.path])
```

```dataviewjs
dv.list([`dv.page('技术清单').file`,dv.page('技术清单').file])
```

```dataviewjs
dv.list([`dv.page('技术清单').file.outlinks`,dv.page('技术清单').file.etags])
```

```dataviewjs
dv.list([`dv.page('技术清单').file.tasks`,dv.page('技术清单').file.tasks])
```

```dataviewjs
dv.list([`dv.page('技术清单').file.tasks`,dv.page('技术清单').file.tasks.map(t=>t.text)])
```

# 创建一条文本
```dataviewjs
dv.el("b", "This is some bold text");
```

# where
```dataviewjs
dv.list(dv.pages("#算法").where(p => p.file.folder=='IT').file.link)
```

# dv.taskList

```dataviewjs
dv.taskList(dv.pages().file.tasks)
```

# dv.taskList 已经完成的任务
```dataviewjs
dv.taskList(dv.pages().file.tasks.filter(t=>t.completed))
```

# dv.taskList 已经完成的任务
```dataviewjs
dv.taskList(dv.pages().file.tasks.where(t=>!t.completed))
```

# dv.table
```dataviewjs
dv.table(["File", "Name", "folder", "path"], dv.pages("#算法") .where(b=>b.file.name!=='dataviewjs') .map(b =>{
const f=b.file
return [f.link,f.name,f.folder,f.path]
} ))
```

```dataviewjs
let pages = dv.pages("#算法");
`for (let group of pages.groupBy(b => b.file.folder)) {
   dv.header(1,group.key);
   dv.list(group.rows.file.name);
   dv.span(group.rows.file.link);
}`

```