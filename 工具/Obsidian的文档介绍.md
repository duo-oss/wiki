[Obsidian] API
[eede]


最新[Obsidian](https://obsidian.md/) API 的类型定义。
[API Types](https://github1s.com/obsidianmd/obsidian-api/blob/c148c24b30dc68d9835820b4169b95d3a038a0f6/obsidian.d.ts)
**警告：** Obsidian API 仍处于早期 alpha 阶段，随时可能更改！

>defrfr
>夫人夫人
>夫人夫人
### [](https://github.com/obsidianmd/obsidian-api/blob/HEAD/README.md#documentation)文档

所有 API 文档都位于该文件中`obsidian.d.ts`。这将包括类型、==属性==、方法和解释所有内容的注释。

有关如何创建 Obsidian 插件的完整示例，请使用[https://github.com/obsidianmd/obsidian-sample-plugin 上](https://github.com/obsidianmd/obsidian-sample-plugin)的模板[](https://github.com/obsidianmd/obsidian-sample-plugin)

### [](https://github.com/obsidianmd/obsidian-api/blob/HEAD/README.md#plugin-structure)插件结构

`manifest.json`

-   `id` 您的插件的 ID。

-   `name` 插件的显示名称。
-   `author` 插件作者的名字。
-   `version` 你的插件的版本。
-   `minAppVersion` 您的插件所需的最低黑曜石版本。
-   `description` 插件的详细描述。
-   `authorUrl` （可选）您自己网站的 URL。
-   `isDesktopOnly` 你的插件是使用 NodeJS 还是 Electron API。

`main.js`

-   这是插件的主要入口点。
-   使用导入任何 Obsidian API `require('obsidian')`
-   使用`require('fs')`或导入 NodeJS 或 Electron API`require('electron')`
-   必须导出扩展的默认类 `Plugin`
-   必须使用 Rollup、Webpack 或其他 javascript 捆绑器将所有外部依赖项捆绑到此文件中。

### [](https://github.com/obsidianmd/obsidian-api/blob/HEAD/README.md#app-architecture)应用架构

##### [](https://github.com/obsidianmd/obsidian-api/blob/HEAD/README.md#the-app-is-organized-into-a-few-major-modules)该应用程序分为几个主要模块：

-   `App`，拥有其他一切的全局对象。您可以通过`this.app`插件内部访问它。该`App`接口为以下接口提供访问器。
-   `Vault`，该界面可让您与 Vault 中的文件和文件夹进行交互。
-   `Workspace`，该界面可让您与屏幕上的窗格进行交互。
-   `MetadataCache`, 包含每个 markdown 文件的缓存元数据的接口，包括标题、链接、嵌入、标签和块。

##### [](https://github.com/obsidianmd/obsidian-api/blob/HEAD/README.md#additionally-by-inheriting-plugin-you-can)此外，通过继承`Plugin`，您可以：

-   使用`this.addRibbonIcon`.添加功能区图标。
-   使用`this.addStatusBarItem`.添加状态栏（底部）元素。
-   添加一个全局命令，可选择使用默认热键，使用`this.addCommand`.
-   使用添加插件设置选项卡`this.addSettingTab`。
-   使用 注册一种新的视图`this.registerView`。
-   使用`this.loadData`和保存和加载插件数据`this.saveData`。

##### [注册事件](https://github.com/obsidianmd/obsidian-api/blob/HEAD/README.md#registering-events)

要从任何事件接口注册事件，例如`App`和`Workspace`，请使用`this.registerEvent`，它会在您的插件卸载时自动分离您的事件处理程序：

```ts
this.registerEvent(app.on('event-name', callback));
```

如果您为插件卸载后仍保留在页面上的元素注册 DOM 事件，例如`window`或`document`事件，请使用`this.registerDomEvent`：

```js
this.registerDomEvent(element, 'click', callback);
```

如果您使用`setInterval`，请使用`this.registerInterval`：

```js
this.registerInterval(setInterval(callback, 1000));
```

# 非人防
## 帆rfrrgfr


