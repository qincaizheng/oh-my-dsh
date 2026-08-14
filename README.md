# oh-my-dsh — DSH 整合包

DeepSeek Harness（DSH）一键整合包：14 组精选插件 + 开箱配置，覆盖 Web UI 全家桶、编码工具链、权限 auto 档、`/btw` 侧问与第三方插件兼容修复层。

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
| 修复层 | dsh-upstream-fixes（修复 auto-approval scoped 名与 sidechain 客户端深路径 import 两个启动崩溃点，**必装**） |

## 快速开始（把下面的提示词交给 AI）

本仓库的权威安装手册是根目录的 [INSTALL.md](./INSTALL.md)。把下面这段提示词复制给你使用的任意 AI / Agent，它会读取手册并完成整套安装：

```text
请阅读本仓库根目录的 INSTALL.md（DSH 整合包安装指南），并严格按照其中的清单与步骤执行：

1. 检查前置条件（dsh web 可运行、Node ≥ 20、pnpm ≥ 10、网络可达 GitHub 与 npm）；
2. 按 §1 清单与 §2 步骤安装全部插件（npm 发布包与源码包两类，源码包先克隆再 link 挂载）；
3. 应用 §3 的配置追加（~/.dsh/profiles/web/cordis.patch.yml：better-sidebar 挂载行 + auto/auto-review 权限预设）；
4. 执行 §4 的三个通用坑修复（断链改指全局 DSH 安装、裸 schemastery 别名、未发布私有依赖处理）；
5. 执行 §5 的启动前验证（组合树无 entry not found、各插件宿主模块可 import、upstream-fixes 别名就位）；
6. 安装完成后重启 dsh web 并硬刷新浏览器，按 §8 验收清单逐项确认。

任何一步失败：先对照 §6「为什么 dsh-upstream-fixes 必装」与 §9「已知风险」判断原因，再决定修复或回滚（§10），最后把结果与建议报告给用户。不要跳过验证步骤，不要省略 upstream-fixes 的别名修复。
```

## 目录结构

```text
.
├── README.md     # 本文件：整合包说明 + 交给 AI 的安装提示词
└── INSTALL.md    # 权威安装手册：清单 / 步骤 / 配置 / 修复 / 验证 / 回滚
```

## 注意事项

- 本整合包按 DSH 0.1.0-rc.6 实测整理；部分老插件（vision-toolkit / agent-teams / toolkit 系 rc.1 时代源码）已按 rc.6 重建/链接，**升级 DSH 后需重跑 INSTALL.md §4 的修复与构建**。
- 第三方插件兼容情报参考：<https://github.com/AdamPlatin123/awesome-dsh-plugins>（每日兼容矩阵）、<https://github.com/0xsline/awesome-deepseek-harness>（人工精选目录）。
- `dsh-upstream-fixes` 是本整合包的修复层插件，修复两个真实崩溃点；未安装时重启 dsh web 会直接报错，详见 INSTALL.md §6。
