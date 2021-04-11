### commit message 格式

当使用`git commit`进入编辑界面时，要提交的message应按照以下格式进行书写

```
<type>(<scope>): <subject>
//空行
<body>
//空行
<footer>
```

- 标题行: 必需, 描述主要修改类型和内容

- 主题内容: 描述为什么修改, 做了什么样的修改, 以及开发的思路等等

- 页脚注释

此格式是目前最流行的写法。更多信息可参考[Angular规范](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)以及[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

#### header

Header 部分只有一行，包括三个字段：`type`（必需）、`scope`（可选）和`subject`（必需）。

1. type

type只允许7个类别的标识

```
feat：新功能（feature）
fix：修补bug
docs：文档（documentation）
style：格式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
test：增加测试
chore：构建过程或辅助工具的变动
```

2. scope

scope 用于说明 commit 影响的范围，比如数据层、控制层、视图层等等。

- 以动词开头，使用第一人称一般现在时。且以小写开头。

3. subject

subject 是 commit 目的的简短描述。

> 参考
>
> [Commit message 和 Change log 编写指南](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
>
> [优雅的提交你的 Git Commit Message](https://juejin.im/post/5afc5242f265da0b7f44bee4)

