# chatgpt-exporter-userscript-fix

基于原始 Tampermonkey 油猴脚本的兼容性修复版，主要解决 DeepSeek 网页新版 DOM 变化后，聊天记录中只有部分消息可勾选导出的问题。

## 项目作用

这个项目用于维护一个可直接安装的油猴脚本：

- 为 DeepSeek 聊天消息添加多选框
- 支持批量导出聊天内容
- 支持导出 Markdown、Word、PDF、脑图
- 支持生成图文卡片
- 修复 DeepSeek 页面中“用户消息 / 模型回复 / 思考过程”复选框不完整的问题

## 背景

原脚本在 DeepSeek 旧版页面结构下可以工作，但随着页面 DOM 调整，消息识别逻辑出现兼容性问题：

- 有时只有用户消息前面有复选框
- 有时只有模型回复前面有复选框
- 思考过程区域可能无法被选中和导出

本仓库在保留原脚本主要功能的基础上，补充了对新版 DeepSeek 页面结构的兼容处理。

## 当前内容

- [`deepseek-chat-exporter.user.js`](/Users/xuwei/Documents/code/ai/chatgpt-exporter/deepseek-chat-exporter.user.js)：可直接安装的 Tampermonkey 用户脚本
- [`deepseek-chat-exporter.meta.js`](/Users/xuwei/Documents/code/ai/chatgpt-exporter/deepseek-chat-exporter.meta.js)：供 Tampermonkey 检查更新使用的元数据文件
- [`DeepSeek-复选框修复说明.md`](/Users/xuwei/Documents/code/ai/chatgpt-exporter/DeepSeek-%E5%A4%8D%E9%80%89%E6%A1%86%E4%BF%AE%E5%A4%8D%E8%AF%B4%E6%98%8E.md)：修复分析与实现说明

## 使用方式

### 方式一：直接安装 `deepseek-chat-exporter.user.js`

1. 安装浏览器扩展 Tampermonkey
2. 打开仓库中的 `deepseek-chat-exporter.user.js`
3. 点击 GitHub 页面右上角的 `Raw`
4. 浏览器会自动唤起 Tampermonkey 安装页
5. 点击安装即可

如果你已经把仓库发布到了 GitHub，建议将下面这个 Raw 链接放到 README 或仓库说明中，方便其他用户直接安装：

```text
https://raw.githubusercontent.com/xw616525957/deepseek-chat-exporter-tampermonkey/master/deepseek-chat-exporter.user.js
```

用户也可以直接访问这个地址进行安装。

### 方式二：手动安装

1. 安装浏览器扩展 Tampermonkey
2. 新建一个用户脚本
3. 将 `deepseek-chat-exporter.user.js` 文件内容全部复制进去
4. 保存脚本
5. 打开 DeepSeek 聊天页面并刷新

## 如何让其他用户下载

推荐让其他用户通过下面两种方式获取脚本：

1. 进入仓库后打开 `deepseek-chat-exporter.user.js`，点击 `Raw` 安装
2. 直接访问 Raw 链接安装

推荐分享地址：

```text
仓库主页：
https://github.com/xw616525957/deepseek-chat-exporter-tampermonkey

脚本直链：
https://raw.githubusercontent.com/xw616525957/deepseek-chat-exporter-tampermonkey/master/deepseek-chat-exporter.user.js
```

## Tampermonkey 版本管理与自动更新

Tampermonkey 可以利用脚本头部的更新地址自动检查新版本。当前仓库采用标准的 `.user.js + .meta.js` 结构：

- `@version`
- `@downloadURL`
- `@updateURL`

其中：

- `deepseek-chat-exporter.user.js` 用于安装和实际下载脚本
- `deepseek-chat-exporter.meta.js` 用于检查版本更新

脚本头中的地址应保持为：

```javascript
// @downloadURL https://raw.githubusercontent.com/xw616525957/deepseek-chat-exporter-tampermonkey/master/deepseek-chat-exporter.user.js
// @updateURL https://raw.githubusercontent.com/xw616525957/deepseek-chat-exporter-tampermonkey/master/deepseek-chat-exporter.meta.js
```

这样做之后，其他用户只要通过你的仓库安装过一次脚本，后续你每次提升 `@version` 并同步更新 `.user.js` 与 `.meta.js`，Tampermonkey 就可以检测到新版本。

### 用户如何检查更新

其他用户安装脚本后，可以通过 Tampermonkey 使用以下方式更新：

1. 打开 Tampermonkey 控制面板
2. 找到当前脚本
3. 点击“检查更新”
4. 如果远程版本号更高，Tampermonkey 会提示安装新版本

### 维护者如何发布新版本

每次更新建议按下面流程操作：

1. 修改 `deepseek-chat-exporter.user.js`
2. 同步更新 `deepseek-chat-exporter.meta.js` 中的 `@version`
3. 提交并 push 到 GitHub
4. 用户在 Tampermonkey 中执行“检查更新”，或等待自动检测

### 注意

如果 `@downloadURL` 和 `@updateURL` 仍然指向原脚本发布地址，那么用户后续检查更新时，拿到的仍然会是原作者发布的版本，而不是你这个仓库的维护版本。

## 适用场景

- DeepSeek 网页聊天导出
- 批量勾选用户消息和模型消息
- 导出问答内容、回复正文、思考过程

## 仓库定位

这是一个社区维护仓库，用于：

- 跟进 DeepSeek 页面结构变化
- 快速修复油猴脚本兼容性问题
- 为其他用户提供可直接下载和安装的更新版本

如果后续找到了原作者公开仓库，可以再将修复以 Issue、Patch 或 Merge Request 的形式同步回原项目。

## 许可证与说明

原脚本头部声明许可证为 `MIT`。如果你继续公开分发，建议：

- 保留原作者署名信息
- 在变更记录中说明你的修复内容
- 明确这是兼容性维护版本，而不是原作者官方仓库
