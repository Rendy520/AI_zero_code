
# 什么是模板方法模式？AI 零代码应用生成项目中，你如何使用模板方法模式处理文件保存逻辑？

模板方法模式是一种行为设计模式，它在一个抽象父类中定义了一个操作的标准流程或骨架，并将一些具体的、可变的实现步骤延迟到子类中去完成。 这样，子类可以在不改变算法整体结构的情况下，重新定义该算法的某些特定步骤。

在我们的项目中，这个模式被用来优化代码文件的保存流程。 无论是生成单文件 HTML 应用还是多文件应用，其核心保存流程是相似的：**验证输入 -> 创建唯一目录 -> 保存文件**。但具体 保存文件 这一步的实现是不同的：HTML 模式只保存一个 `index.html`，而多文件模式需要分别保存 `index.html`、`style.css` 和 `script.js`。

1）定义抽象模板：我们创建了一个抽象类 `CodeFileSaverTemplate<T>`。它定义了一个 `final` 的 `saveCode` 方法，这就是模板方法。这个方法固定了文件保存的整体流程，保证所有子类的保存行为都遵循统一的规范。

2）实现具体子类：

-   `HtmlCodeFileSaverTemplate`：继承自抽象模板，实现了 `saveFiles` 方法，逻辑是在指定目录下写入一个 `index.html` 文件。 它还重写了 `validateInput` 方法，增加了对 HTML 代码非空的校验。
-   `MultiFileCodeFileSaverTemplate`：同样继承自抽象模板，但它的 `saveFiles` 方法会分别写入 `index.html`、`style.css` 和 `script.js` 三个文件。

3）通过执行器调用：与策略模式类似，我们使用 `CodeFileSaverExecutor` 来根据代码生成类型，选择并调用相应的模板子类来执行保存操作，对上层屏蔽具体实现的差异。

架构如下图所示：

![](https://pic.code-nav.cn/mianshiya/question_picture/markdown/mwowSu5c_1755832948165-40af3eca-3cb2-4d03-a6fd-8f107fb919ed_mianshiya.png)

它把文件保存流程中的不变部分和可变部分分离开来，代码结构清晰，复用性强。当需要支持新的文件保存逻辑时，只需创建一个新的子类并实现其特定步骤即可。
