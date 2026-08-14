# 插件清单（唯一权威）

> 整合包的完整插件列表。每个条目给出：插件、**包名（profile 依赖键）**、来源仓库、安装方式、挂载备注。
> 安装前先按 [INSTALL.md](./INSTALL.md) §1 与本机已装插件对比，**只安装缺失项，已安装的跳过**。

## A. dsh-web-ui 全家桶（同一仓库子包，link 安装）

仓库：<https://github.com/zhu1090093659/dsh-web-ui>（clone 到 `~/.dsh/plugins/dsh-web-ui`，`pnpm install` 后逐个 `dsh plugin add <子包路径>`）

| 插件 | 包名（依赖键） | 来源子包 | 挂载 |
|---|---|---|---|
| dsh-ssh | `@deepseek-ai/dsh-ssh` | packages/dsh-ssh | bundle |
| dsh-task-board | `@deepseek-ai/dsh-client-ui-task-board` | packages/dsh-task-board | bundle |
| dsh-aionui-panel | `@deepseek-ai/dsh-client-ui-aionui-panel` | packages/dsh-aionui-panel | bundle |
| dsh-git-graph | `@deepseek-ai/dsh-client-ui-git-graph` | packages/dsh-git-graph | bundle |
| dsh-live-stats | `@deepseek-ai/dsh-live-stats` | packages/dsh-live-stats | bundle |
| dsh-pet | `@deepseek-ai/dsh-pet` | packages/dsh-pet | bundle |
| dsh-remote-web-ui | `@deepseek-ai/dsh-remote-web-ui` | packages/dsh-remote-web-ui | bundle |
| dsh-web-ui-settings | `@deepseek-ai/dsh-client-ui-web-ui-settings` | packages/dsh-web-ui-settings | bundle |
| skin-center | `@deepseek-ai/dsh-client-ui-skin-center` | packages/skins/skin-center | bundle |

## B. npm 注册表发布

| 插件 | 包名（依赖键） | 安装方式 | 挂载 |
|---|---|---|---|
| dsh-web-review | `@canglongcl/dsh-web-review@^0.1.0` | npm | bundle |
| dsh-openpencil | `@zseven-w/dsh-openpencil@0.1.0-rc.1` | npm | bundle |
| dsh-auto-approval | `dsh-auto-approval@^0.1.0`（**裸名**） | npm | bundle（scoped 挂载名由 upstream-fixes 别名修复） |
| dsh-client-ui-auto-approval | `dsh-client-ui-auto-approval@^0.1.0`（**裸名**） | npm | 纯 client 插件（无 bundle，随修复层注册） |

## C. 源码 link（未发布 npm）

| 插件 | 包名（依赖键） | 来源仓库 | 安装方式 | 挂载 |
|---|---|---|---|---|
| dsh-better-sidebar | `dsh-better-sidebar` | <https://github.com/omdsh-dev/DSH-better-sidebar> | link + pnpm 构建（prepare 自动） | **需手动 insert 行** |
| dsh-at-file | `dsh-at-file` | <https://github.com/omdsh-dev/dsh-at-file> | link（lib 已提交） | bundle |
| dsh-agent-teams | `dsh-agent-teams` | <https://github.com/NanmiCoder/dsh-agent-teams> | link（依赖走 INSTALL.md §4.3 软链法） | bundle |
| dsh-git-identity | `@loserfox/git-identity` | <https://github.com/LoserFox/dsh-git-identity> | link（零依赖零构建） | bundle |
| plugin-console | `@dsh-external/plugin-console` | <https://github.com/vlln/plugin-registry>（子包 packages/plugin/console） | link + 补 yaml 依赖 | bundle |
| dsh-toolkit | `@deepseek-ai/dsh-toolkit` | <https://github.com/omdsh-dev/dsh-toolkit> | link + 构建（10 子包，清 lockfile 后 npm install） | bundle |
| dsh-sidechain | `@dsh-external/dsh-sidechain` | <https://github.com/Buyi-wsgzg/dsh-sidechain> | link + pnpm 构建 + 裸 schemastery 别名 | bundle（客户端依赖 upstream-fixes shim） |
| dsh-upstream-fixes | `@dsh-external/dsh-upstream-fixes` | <https://github.com/qincaizheng/dsh-upstream-fixes> | git clone + link + **手动跑 install-aliases.mjs** | bundle（**必装修复层**，见 INSTALL.md §6） |
| dsh-mcp-manager | `dsh-mcp-manager` | <https://github.com/hyqhyq3/dsh-mcp-manager> | link（lib 已提交零依赖） | bundle |
| dsh-paste-input | `@dsh-community/dsh-paste-input` | <https://github.com/lhh010/dsh-paste-input> | link（lib 已提交零依赖） | 纯插件无 bundle，**需手动 insert 行** |
| dsh-annotation | `@omdsh-dev/dsh-annotation` | <https://github.com/dsh-external/dsh-annotation>（现组织 omdsh-dev） | link（预构建零依赖） | bundle（**勿**再手动 insert） |

## 统计

- 共 24 个包 / 16 个功能组。
- 「挂载」为 bundle 的条目：`dsh plugin add` 后自动进 `dsh.profile.bundles`，无需其他操作。
- 「挂载」为手动 insert 的条目（dsh-better-sidebar、dsh-paste-input）：必须在 profile `cordis.patch.yml` 写对应 insert 行（见 INSTALL.md §3），且**只写一次**。
