# DeepSeek 复选框修复说明

## 问题现象

在 DeepSeek 聊天页中，油猴脚本只能给用户提问消息添加复选框，大模型回复和思考过程没有复选框，导致无法批量导出完整对话。

## 根因分析

原脚本的 DeepSeek 复选框逻辑在 `addCheckboxesToMessages_DeepSeek()` 中，依赖以下旧版 DOM 假设：

1. 先扫描 `.ds-flex` 操作栏。
2. 再从操作栏向上回溯父节点和前序兄弟节点。
3. 通过这套固定层级关系推断消息容器。

这套逻辑对旧结构有效，但当前 DeepSeek 页面里，消息容器已经更接近统一的 `.ds-message`/消息块结构，回答区和思考区不再稳定满足“从 `.ds-flex` 反推容器”的路径，所以最终只覆盖了部分用户消息。

## 修复方案

本次修复将 DeepSeek 逻辑从“操作栏驱动”改成“消息容器驱动”：

1. 新增 DeepSeek 消息容器选择器集合，优先识别 `.ds-message`，并兼容旧的 `data-testid`/通用消息容器类名。
2. 通过正文块 `.ds-markdown` 反向定位消息容器，而不是依赖 `.ds-flex` 的固定层级。
3. 给每个真实消息容器直接注入复选框，避免遗漏回复消息。
4. 提取导出内容时，改为合并同一消息容器下的多个 markdown 区块，从而覆盖“回答正文 + 思考过程”这类分段内容。
5. MutationObserver 补充监听 `.ds-message`，保证流式回复新增时也能补上复选框。

## 修改文件

- `user.js`

## 关键修改点

### 1. 新增 DeepSeek 容器/内容选择器

在脚本顶部增加：

- `deepSeekMessageWrapperSelectors`
- `deepSeekContentSelectors`
- `deepSeekMessageWrapperSelector`
- `deepSeekContentSelector`

用于统一描述当前 DeepSeek 页面里的消息容器和正文节点。

### 2. 新增消息容器收集函数

新增：

- `isInsideInjectedUi()`
- `getDeepSeekMessageWrappers()`
- `getDeepSeekContentBlocks()`

作用：

- 过滤脚本自己注入的 UI。
- 直接收集真实消息容器。
- 收集同一条消息下的多个正文块，避免只取到一段内容。

### 3. 重写复选框注入逻辑

`addCheckboxesToMessages_DeepSeek()` 已从旧的 `.ds-flex` 回溯方案改为：

- 遍历 `getDeepSeekMessageWrappers()` 的结果。
- 对每个消息容器直接 `prepend` 复选框。
- 通过 `:scope > .gm-message-checkbox-container` 防止重复注入。

### 4. 补强导出内容提取

`getMarkdownFromMessageWrapper()` 的 DeepSeek 分支现在会：

- 优先提取当前消息下的全部 markdown 内容块。
- 将多段内容拼接后再导出。
- 兼容回答正文和思考过程分段渲染的情况。

## 已完成校验

已执行静态语法校验：

```bash
node --check user.js
```

结果通过，无语法错误。

## 手工验证建议

在 Tampermonkey 中重新加载 `user.js` 后，打开 DeepSeek 对话页，验证以下场景：

1. 用户提问消息左侧有复选框。
2. 模型正式回复左侧有复选框。
3. 展开的思考过程所在消息也有复选框。
4. 点击“全选”后，用户消息和模型消息都会被选中。
5. 导出 Markdown/Word/PDF 时，回复正文和思考过程都能被带出。

## 说明

尝试通过浏览器自动化直接复核你给出的会话页时，当前自动化上下文没有复用你本地浏览器的登录态，DeepSeek 跳转到了登录页。因此本次修复基于：

- 当前脚本静态分析
- 公开可见的 DeepSeek 页面结构线索
- 现有选择器兼容性补强

如果你希望，我下一步可以继续基于你本地已登录浏览器做一次真实页面验证，并按实际 DOM 再做一轮定点微调。
