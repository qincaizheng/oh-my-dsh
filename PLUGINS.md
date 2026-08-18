# 插件清单（唯一权威）

> 整合包的完整插件列表。每个条目给出：插件、**包名（profile 依赖键）**、来源仓库、安装方式、挂载备注。
> 安装前先按 [INSTALL.md](./INSTALL.md) §1 与本机已装插件对比，**只安装缺失项，已安装的跳过**。
> 默认禁用清单见 [DISABLED.md](./DISABLED.md)：安装/更新后按 INSTALL.md §2.4 自动禁用对应条目，并告知用户重新启用入口。

## A. dsh-web-ui 全家桶（聚合包，npm 或仓库 link）

仓库：<https://github.com/zhu1090093659/dsh-web-ui>。上游已把全家桶发布为 npm 聚合包（`@linxin666` scope）：一个依赖键装齐 9 个功能插件 + 皮肤全家桶（dsh-skins，皮肤资产内置）+ compat shim（兼容旧壳 `data-pane`/slot 挂载点）。

| 插件 | 包名（依赖键） | 来源 | 安装方式 | 挂载 |
|---|---|---|---|---|
| dsh-web-ui-all（全家桶聚合） | `@linxin666/dsh-web-ui-all` | packages/dsh-web-ui-all | npm（推荐）或仓库 clone + `pnpm -r build` + `scripts/link-profile.mjs` + link | bundle（内部自动 insert 各功能插件行） |

聚合包内含的功能插件（各为一条 insert 行，**无需**再单独安装，功能组不随安装方式变化）：

| 插件 | 聚合内 insert id | 说明 |
|---|---|---|
| dsh-ssh | `ssh` | 远程 SSH 运维面板（配置存 `~/.dsh/dsh-ssh.json`） |
| dsh-task-board | `ui-task-board` | 任务看板 |
| dsh-aionui-panel | `ui-dsh-aionui-panel` | 右侧预览 / 文件树 / SCM 面板（**默认禁用**，见 DISABLED.md） |
| dsh-git-graph | `ui-git-graph` | Git 图谱 |
| dsh-live-stats | `live-stats` | 实时 token 统计（**默认禁用**，见 DISABLED.md） |
| dsh-pet | `pet` | 鲸鱼娘宠物 |
| dsh-remote-web-ui | `remote-web-ui` | 移动端远程 |
| dsh-web-ui-settings | `ui-web-ui-settings` | 插件设置中心 |
| skin-center | `ui-skin-center` | 皮肤中心（皮肤经 dsh-skins 内置，中心内切换） |
| —（compat 桥接） | `ui-web-ui-compat` | 随聚合包内置的兼容 shim |

## B. npm 注册表发布

| 插件 | 包名（依赖键） | 安装方式 | 挂载 |
|---|---|---|---|
| dsh-web-review | `@canglongcl/dsh-web-review@^0.1.0` | npm | bundle |
| dsh-openpencil | `@zseven-w/dsh-openpencil@0.1.0-rc.1` | npm | bundle |
| dsh-thought-buddy | `@dsh-plugin/dsh-thought-buddy@^0.1.1` | npm | bundle（思考时的小表情，localStorage 配置） |
| dsh-auxiliary | `@dsh-plugin/dsh-auxiliary@^0.4.2` | npm | bundle（0.4.2 起内置 bundle patch，**无需**手动 insert 行）；设置页表单 rc.6 不可用，走 settings.yaml（键 `dsh-auxiliary`） |
| dsh-notify | `dsh-notify@^0.1.2` | npm | bundle（会话完成 Windows toast + 音效通知，仅后台触发，音量可调） |

## C. 源码 link（未发布 npm）

| 插件 | 包名（依赖键） | 来源仓库 | 安装方式 | 挂载 |
|---|---|---|---|---|
| dsh-better-sidebar | `dsh-better-sidebar` | <https://github.com/omdsh-dev/DSH-better-sidebar> | link + pnpm 构建（prepare 自动） | **需手动 insert 行** |
| dsh-at-file | `dsh-at-file` | <https://github.com/omdsh-dev/dsh-at-file> | link（lib 已提交） | bundle |
| dsh-agent-teams | `dsh-agent-teams` | <https://github.com/NanmiCoder/dsh-agent-teams> | link（依赖走 INSTALL.md §4.3 软链法） | bundle |
| dsh-git-identity | `@loserfox/git-identity` | <https://github.com/LoserFox/dsh-git-identity> | link（零依赖零构建） | bundle |
| plugin-console | `@dsh-external/plugin-console` | <https://github.com/vlln/plugin-registry>（子包 packages/plugin/console） | link + 补 yaml 依赖 | bundle |
| dsh-toolkit | `@deepseek-ai/dsh-toolkit` | <https://github.com/omdsh-dev/dsh-toolkit> | link + 构建（10 子包，清 lockfile 后 npm install） | bundle |
| dsh-sidechain | `@dsh-external/dsh-sidechain` | <https://github.com/Buyi-wsgzg/dsh-sidechain> | link + pnpm 构建 + 裸 schemastery 别名 | bundle（客户端依赖 upstream-fixes shim；**默认禁用**，见 DISABLED.md） |
| dsh-upstream-fixes | `@dsh-external/dsh-upstream-fixes` | <https://github.com/qincaizheng/dsh-upstream-fixes> | git clone + link + **手动跑 install-aliases.mjs** | bundle（**必装修复层**，见 INSTALL.md §6） |
| dsh-mcp-manager | `dsh-mcp-manager` | <https://github.com/hyqhyq3/dsh-mcp-manager> | link（lib 已提交零依赖） | bundle |
| dsh-paste-input | `@dsh-community/dsh-paste-input` | <https://github.com/lhh010/dsh-paste-input> | link（lib 已提交零依赖） | 纯插件无 bundle，**需手动 insert 行** |
| dsh-annotation | `@omdsh-dev/dsh-annotation` | <https://github.com/dsh-external/dsh-annotation>（现组织 omdsh-dev） | link（预构建零依赖） | bundle（**勿**再手动 insert） |
| approve-for-me（主插件） | `dsh-plugin-approve-for-me` | <https://github.com/ZhuRuoLing/dsh-command-approve-for-me> | link（lib 已提交；peer 走 INSTALL.md §4.3 软链法） | 无 bundle，**需手动 insert 行**（INSTALL.md §3） |
| approve-for-me-ui（前端状态行） | `dsh-client-plugin-approve-for-me` | <https://github.com/ZhuRuoLing/dsh-client-plugin-approve-for-me> | link（lib 已提交；host 仅依赖 zod，软链全局 zod） | 无 bundle，**需手动 insert 行**（INSTALL.md §3） |
| dsh-net-proxy | `dsh-net-proxy` | <https://github.com/mafeis/dsh-net-proxy> | link（lib 已提交；peer 走 INSTALL.md §4.3 软链法） | bundle（agent 网络请求走 HTTP/SOCKS5 代理，配置存 `~/.dsh/net-proxy.json`，设置页即时生效） |

## 统计

- 共 20 个依赖键包 / 20 个功能组（dsh-web-ui 全家桶合并为 1 个聚合依赖键，其 9 个功能插件仍各算 1 个功能组）。
- 「挂载」为 bundle 的条目：`dsh plugin add` 后自动进 `dsh.profile.bundles`，无需其他操作。
- 「挂载」为手动 insert 的条目（dsh-better-sidebar、dsh-paste-input）：必须在 profile `cordis.patch.yml` 写对应 insert 行（见 INSTALL.md §3），且**只写一次**。
