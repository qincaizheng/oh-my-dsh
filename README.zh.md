# oh-my-dsh — DSH 整合包

[English](./README.md) | 中文

DeepSeek Harness（DSH）一键整合包：18 组精选插件 + 开箱配置，覆盖 Web UI 全家桶、编码工具链、approve-for-me 自动审批（codex guardian 式审查 + 沙箱「替我同意」选项）与第三方插件兼容修复层。

## 包含内容

完整插件清单（唯一权威）见 [PLUGINS.md](./PLUGINS.md)：每个条目含包名（profile 依赖键）/ 来源 / 安装方式 / 挂载。

## 快速开始

本仓库的权威安装手册是 [INSTALL.md](./INSTALL.md)。把下面这段提示词交给任意 AI / Agent 即可完成安装：

```text
请阅读 https://github.com/qincaizheng/oh-my-dsh/blob/main/INSTALL.md，把它作为唯一权威的安装手册，在本机 DeepSeek Harness（dsh web）环境按手册完成整合包的全部安装与配置：先按手册 §1 对比本机已装插件、只安装缺失的插件（已安装的跳过），再应用配置追加、按手册处理禁用清单（DISABLED.md）并告知我禁用了哪些插件及重新启用入口、执行修复与验证、重启 dsh web，最后向我汇报安装结果。
```

## 目录结构

```text
.
├── README.md      # 英文说明（默认）+ 交给 AI 的安装提示词
├── README.zh.md   # 中文说明
├── PLUGINS.md     # 插件清单（唯一权威）：包名 / 来源 / 安装方式 / 挂载
├── DISABLED.md    # 默认禁用清单（安装/更新时自动处理，附重新启用入口）
└── INSTALL.md     # 安装手册：对比本机 → 装缺失项 → 配置 → 验证 → 回滚
```

## 注意事项

- 本整合包按 DSH 0.1.0-rc.6 实测整理；部分老插件（agent-teams / toolkit 系 rc.1 时代源码）已按 rc.6 重建/链接，**升级 DSH 后需重跑 INSTALL.md §4 的修复与构建**。
- 第三方插件兼容情报参考：<https://github.com/AdamPlatin123/awesome-dsh-plugins>（每日兼容矩阵）、<https://github.com/0xsline/awesome-deepseek-harness>（人工精选目录）。
- `dsh-upstream-fixes` 是本整合包的修复层插件，最初修复旧 auto-approval / sidechain 带来的两个崩溃点；auto-approval 已被 approve-for-me 替代、sidechain 默认禁用后该层已无实际作用，但保留安装以兼容历史配置——详见 INSTALL.md §6。