<div class="header-banner purple">
<div class="header-label purple">为 Dialogic 做贡献</div>
</div>

我们公开地在 [GitHub](https://github.com/dialogic-godot) 上开发 Dialogic 插件。
主要的 [dialogic](https://github.com/dialogic-godot/dialogic) 仓库包含插件的全部代码、图像与逻辑，而本文档则存放在 [documentation](https://github.com/dialogic-godot/documentation) 仓库中。你还会找到其他仓库，例如 [Godot 3 版本的 Dialogic](https://github.com/dialogic-godot/dialogic-1)、[测试项目](https://github.com/dialogic-godot/test-project)、[视觉小说模板](https://github.com/dialogic-godot/visual-novel-template)，以及各类扩展等等。

## 如何贡献

为 Dialogic 做贡献有不同方式。你可能是一名开发者、设计师、写作者，或者是一位想帮助改进项目的用户。以下是一些你可以贡献的方式：
- [报告 bug](#报告-bug)
- [创建拉取请求](#创建拉取请求)
- [回答问题](#回答问题)
- [捐赠](#捐赠)


### 报告 bug
在制作自己的项目时，你可能会遇到 bug，或想到一个能让 Dialogic 社区受益的新功能。如果你认为这项改进或 bug 修复对大多数用户有价值，那它就值得作为一个 issue 提出。Dialogic 与 Godot 的设计目标是灵活且可扩展，因此最好对具体的游戏需求保持中立。在创建新 issue 之前，请考虑以下步骤，以帮助我们改进 Dialogic：

- **搜索相似 issue：** 使用 GitHub 上的[搜索功能](https://github.com/dialogic-godot/dialogic/issues)输入与你所发现问题相关的关键词。这有助于你查看是否已有人报告过。
- **查看已有 issue：** 浏览[现有 issue 列表](https://github.com/dialogic-godot/dialogic/issues)，确认你的问题是否已被记录。如果已有记录，请考虑参与讨论或主动提供帮助，而不是新建一个。

如果你的问题并不存在，请点击「New issue」按钮或访问[此页面](https://github.com/dialogic-godot/dialogic/issues/new/choose)来创建它。记得详细描述问题，如果是 bug 请包含复现步骤、你使用的 Godot 与 Dialogic 版本，或者解释新功能为何有益。如果是 bug，附上一个能复现该问题的最小代码示例会非常有帮助，例如一个小型的 Godot+Dialogic 项目来演示该问题。这有助于维护者快速定位问题的原因并修复它。


### 创建拉取请求

如果你已经找到某个已有 issue，并希望贡献修复或增强，请按以下步骤创建拉取请求：

1. **Fork 仓库：** 点击仓库页面右上角的「Fork」按钮。这会在你的 GitHub 账户中创建该仓库的副本。
2. **克隆仓库：** 点击「Code」按钮并复制 URL。然后在终端中运行：
    ```bash
    git clone <URL>
    ```
3. **创建新分支：** 在你的本机上进入仓库目录，并为你的改动创建一个新分支：
    ```bash
    git checkout -b <branch-name>
    ```
4. **进行修改：** 编辑仓库中的文件，实现你的修复或增强。
5. **提交修改：** 暂存并提交你的改动，附上描述清楚的提交信息：
    ```bash
    git add .
    git commit -m "Your message here"
    ```
6. **推送修改：** 将你的改动推送到你 fork 的仓库：
    ```bash
    git push origin <branch-name>
    ```
7. **创建拉取请求：** 前往 GitHub 上的仓库页面，点击「New pull request」按钮。选择正确的分支，并详细描述你的改动，引用你所处理的 issue。

如果你觉得使用终端有困难，也可以使用 [GitHub Desktop](https://desktop.github.com/) 应用或其他图形化 git 客户端来管理你的改动。这些工具提供了更易用的界面来处理 git，让你能更专注于贡献内容，而不是命令行操作。

提交拉取请求后，请留意项目维护者的评论或反馈。如果需要修改，请根据反馈做出必要的调整，并将更新推送到你的分支；你的拉取请求会自动更新。与审查者礼貌交流，感谢他们的反馈，如有需要可以请求澄清。记住，我们都是志愿者，一起为所有人改进这个项目，因此保持积极协作的氛围很重要。


### 回答问题

回答问题是为 Dialogic 社区做贡献的另一种宝贵方式。你可以通过参与我们的 [GitHub Discussions 页面](https://github.com/dialogic-godot/dialogic/discussions)和 [Discord 服务器](https://discord.com/invite/DjcDgDaTMe)来提供帮助，用户经常在那里询问插件用法、故障排查或最佳实践建议。


### 捐赠

如果你喜欢 Dialogic 并希望支持它的开发，可以通过 [Patreon](https://www.patreon.com/jowanspooner) 或 [Ko-fi](https://ko-fi.com/jowan_spooner)向项目捐赠。非常感谢你的支持，这有助于让项目持续发展！
