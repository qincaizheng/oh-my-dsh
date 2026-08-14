# oh-my-dsh — DSH 整合包

[English](./README.md) | 中文

DeepSeek Harness（DSH）一键整合包：17 组精选插件 + 开箱配置，覆盖 Web UI 全家桶、编码工具链、权限 auto 档、`/btw` 侧问与第三方插件兼容修复层。

## 包含内容

| 类别 | 插件 |
|---|---|
| Web UI 全家桶 | ssh · task-board · aionui-panel · git-graph · live-stats · pet · remote-web-ui · web-ui-settings · skin-center |
| 编码工作台 | dsh-better-sidebar（文件/终端/Git 侧栏）· dsh-at-file（@文件提及）· dsh-web-review（网页预览批注）· dsh-openpencil（设计稿）· dsh-toolkit（10 个工具）· dsh-git-identity（提交身份） |
| 视觉与浏览器 | dsh-vision-toolkit（截图 OCR/UI 还原） |
| 多智能体 | dsh-agent-teams |
| 权限扩展 | dsh-auto-approval（auto 档两态分类器）+ 预设 auto / auto-review |
| 侧问 | dsh-sidechain（`/btw` 一次性侧问、`/side` 持续侧会话） |
| 插件管理 | plugin-console（浏览器面板管理插件安装态） |
| MCP | dsh-mcp-manager（设置 → MCP 页：stdio/SSE 服务器管理，OAuth PKCE + 动态客户端注册） |
| 输入增强 | dsh-paste-input（Ctrl+V 粘贴 / 拖拽 / 选择文件，发送时复制进会话工作区） |
| 文本批注 | dsh-annotation（选中回复文本 → 加批注 → 发送，模型按编号逐一回应并显示悬停标签） |
| 修复层 | dsh-upstream-fixes（修复 auto-approval scoped 名与 sidechain 客户端深路径 import 两个启动崩溃点，**必装**） |

## 快速开始

本仓库的权威安装手册是 [INSTALL.md](./INSTALL.md)。把下面这段提示词交给任意 AI / Agent 即可完成安装：

```text
请阅读 https://github.com/qincaizheng/oh-my-dsh/INSTALL.md，把它作为唯一权威的安装手册，在本机 DeepSeek Harness（dsh web）环境按手册完成整合包的全部安装与配置：装完所有插件、应用配置追加、执行手册中的修复与验证、重启 dsh web，不要跳过任何插件或验证步骤，最后向我汇报安装结果。
```

## 目录结构

```text
.
├── README.md      # 英文说明（默认）+ 交给 AI 的安装提示词
├── README.zh.md   # 中文说明
└── INSTALL.md     # 权威安装手册：清单 / 步骤 / 配置 / 修复 / 验证 / 回滚
```

## 注意事项

- 本整合包按 DSH 0.1.0-rc.6 实测整理；部分老插件（vision-toolkit / agent-teams / toolkit 系 rc.1 时代源码）已按 rc.6 重建/链接，**升级 DSH 后需重跑 INSTALL.md §4 的修复与构建**。
- 第三方插件兼容情报参考：<https://github.com/AdamPlatin123/awesome-dsh-plugins>（每日兼容矩阵）、<https://github.com/0xsline/awesome-deepseek-harness>（人工精选目录）。
- `dsh-upstream-fixes` 是本整合包的修复层插件，修复两个真实崩溃点；未安装时重启 dsh web 会直接报错，详见 INSTALL.md §6。
