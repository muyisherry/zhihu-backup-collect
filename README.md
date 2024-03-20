# 知乎备份剪藏

在这个互联网没有记忆的时代，帮你保存知乎上珍贵的内容，方便日后查阅。

* 复制知乎文章/回答/想法为 Markdown
* 下载文章/回答/想法为 zip（包含图片与文本，以及赞数时间等信息）
* 剪藏文章/回答/想法为图片
* 可选添加保存备注
* 可选保存当前页评论

注：此项目**非**爬蟲，仅用于用户**日常保存喜欢的内容**。请尊重内容作者权利，切勿用于抄袭与盈利。

此项目基于[github.com/Howardzhangdqs/zhihu-copy-as-markdown](https://github.com/Howardzhangdqs/zhihu-copy-as-markdown)（MIT）开发而来，感谢原作者的探索。原作者实现了 Markdown 相关和 zip 下载，我进行优化并适配各种场景和内容类型，添加存图、备注和评论支持。

## Usage

安装油猴脚本：https://greasyfork.org/zh-CN/scripts/486538-%E7%9F%A5%E4%B9%8E%E5%A4%87%E4%BB%BD%E5%89%AA%E8%97%8F（未进行全面测试，可能存在bug）

鼠标移到知乎内容上，会出现保存按钮，点击即可保存（到下载目录）。已支持的页面有关注页，个人主页，回答页，问题页，文章页，想法页，收藏夹页，推荐页、搜索结果页；已支持的内容有文章/回答/想法。具体功能解释：

* 复制 Markdown：复制到剪贴板，语法见[Markdown Reference](https://commonmark.org/help/)
* 下载 zip：将内容的图片、Markdown 文本、信息（赞数、时间等）、当前页评论（如果启用）保存为 zip，文件名格式`标题_作者_日期_备注.zip`
* 剪藏图片：将当前内容（和评论）截为 PNG 图片，会自动隐藏你的头像以保护隐私。**请先滚动到底确保所有图片都加载**，否则图片会是空白
* 备注：备注会保存在文件名末尾，最长60字符，空格会转义为“-”，不能包含` \ / : * ? " < > |`。
* 保存评论：执行以上操作时包含**当前**显示的评论，只能保存内容下方的（弹出式窗口的评论不能），还未解析为 Markdown，凑合用。

可能的问题：

* 能不能保存更多评论？不能
* 如何保存PDF？右键-->打印-->打印为PDF
* 能否批量保存某答主/问题？不能，请找爬蟲
* 已知问题：保存图片时部分样式（点赞栏等）轻微异常
* 已知问题：未适配视频页和部分视频
* ~~已知问题：收起又展开内容后保存报错，控制台显示`false false false`或`未知内容`，请不要收起又展开~~

其他推荐：**SingleFile**，浏览器扩展，扩展商店自寻，可以将网页的全部或部分（比如个人主页的一串回答、弹出窗口中的一串评论）保存为`html`单文件。

## Dev

安装依赖

```bash
pnpm i
```

测试

`0.7.11+`已启用更方便的测试：

1. 允许脚本管理器 Tampermonkey 访问文件网址 右键插件图标-插件管理页面-访问文件网址 或者参照官方 [faq](https://tampermonkey.net/faq.php?ext=dhdg#Q204)
2. 在脚本管理器中安装`scripts/dev.js`，并且修改其`@require `为正确的路径。它会调用本地的`dist/bundle.js`。
3. `pnpm dev`
4. 刷新目标网页

打包

```bash
pnpm build
```

`dist/tampermonkey-script.js` 即为脚本，复制到油猴即可使用。

## 原理

1. 获取页面中所有的富文本框 `RichText` 为 `DOM`
2. 将 `DOM` 使用 `./src/lexer.ts` 转换为 `Lex`
3. 将 `Lex` 使用 `./src/parser.ts` 转换为 `Markdown`
4. 根据每个 `DOM` 获取标题等信息

## TODO

- [ ] 下载文章时同时包含头图
- [ ] TOC解析
- [ ] Markdown纯文本转义
- [X] 解析当前页评论为Markdown
- [X] 为Markdown添加frontmatter
- [ ] 考虑移除info.json

## Changelog

* 24.3.20（0.8.9）:
    - 保存失败时自动尝试补救
* 24.3.20（0.8.8）:
    - 修复无法保存匿名用户的内容
    - 增加保存失败原因提示
* 24.3.4（0.8.7）:
    - 更方便的测试
    - 解析评论为Markdown
    - 评论图片本地化
    - 完善解析评论修复bug
    - 修复zip内文件日期错误问题
    - 修复无法下载视频问题
    - 适配推荐页、搜索结果页
    - info中添加ip属地（如果有）
    - 修复想法无法保存图片
* 24.2.29（0.7.10）:
    - 备注改为最长60字
    - 修复个人页无法保存想法问题
    - 修复保存zip处理评论可能出错问题
* 24.2.4（0.7.7）:
    - 为Markdown添加frontmatter
    - 修正下载md内的图片路径为本地路径
    - 对于有目录的内容，减轻按钮与目录的重叠
* 24.1.19（0.7.4）:
    - 截图适配专栏
* 24.1.13（0.7.x）:
    - 粗略解析评论并添加到zip
    - 修复大量bug
    - 准备发布
* 24.1.13（0.6.x）:
    - 适配想法中的复杂情形
* 24.1.11（0.5.x）:
    - 添加截图功能
    - 初步适配想法
* 24.1.2（0.4.x）:
    - 初步重制
* 23.12.29:
    - 立项
