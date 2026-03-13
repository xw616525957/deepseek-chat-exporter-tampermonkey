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

- [`user.js`](/Users/xuwei/Documents/code/ai/chatgpt-exporter/user.js)：修复后的 Tampermonkey 用户脚本
- [`DeepSeek-复选框修复说明.md`](/Users/xuwei/Documents/code/ai/chatgpt-exporter/DeepSeek-%E5%A4%8D%E9%80%89%E6%A1%86%E4%BF%AE%E5%A4%8D%E8%AF%B4%E6%98%8E.md)：修复分析与实现说明

## 使用方式

1. 安装浏览器扩展 Tampermonkey
2. 新建一个用户脚本
3. 将 [`user.js`](/Users/xuwei/Documents/code/ai/chatgpt-exporter/user.js) 内容粘贴进去并保存
4. 打开 DeepSeek 聊天页面刷新后使用

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
