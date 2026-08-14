# DSH 整合包 · 安装指南（给执行安装的模型/Agent 看）

> 目标读者：负责部署本整合包的 AI 模型。本文是唯一权威清单：从哪装、怎么装、装什么、怎么验证。
> 目标环境：已装 DSH（`dsh web` 可跑），Node ≥ 20，pnpm ≥ 10，web profile 位于 `~/.dsh/profiles/web/`。

## 0. 总原则

1. 所有安装统一走 `dsh plugin --profile web add <包名|本地路径>`（底层转发 pnpm，装完自动把带 `dsh.bundle` 声明的包 reconcile 进 `dsh.profile.bundles` 层栈）。
2. 纯 cordis 插件（无 `dsh.bundle`，如 dsh-better-sidebar）必须额外在 profile 的 `cordis.patch.yml` 写 insert 挂载行。
3. **装完全部插件后必须重启 `dsh web` 并硬刷新浏览器**（Cmd/Ctrl+Shift+R）才生效。
4. 源码插件常见三个坑，通用修复见 §4。

## 1. 安装清单（当前整合包 = 14 组）

### A. npm 注册表发布（优先，一条命令）

| 插件 | 包名 | 用途 |
|---|---|---|
| dsh-web-review | `@canglongcl/dsh-web-review@^0.1.0` | 网页预览 + 元素批注，AI 按可视化反馈改前端源码 |
| dsh-openpencil | `@zseven-w/dsh-openpencil@0.1.0-rc.1` | 设计稿预览与编辑 |
| dsh-auto-approval | `dsh-auto-approval@^0.1.0`（**裸名**） | 权限 auto 档：L0 规则 + L1 LLM 两态分类器 |
| dsh-client-ui-auto-approval | `dsh-client-ui-auto-approval@^0.1.0`（**裸名**） | composer 旁 AA 状态芯片 |

### B. 源码安装（未发布 npm）

| 插件 | 仓库 | 备注 |
|---|---|---|
| dsh-better-sidebar | <https://github.com/omdsh-dev/DSH-better-sidebar> | 需 pnpm 构建；纯 cordis 插件，需 insert 行 |
| dsh-at-file | <https://github.com/omdsh-dev/dsh-at-file> | lib 已提交，无需构建 |
| dsh-agent-teams | <https://github.com/NanmiCoder/dsh-agent-teams> | lib 已提交 |
| dsh-vision-toolkit | <https://github.com/Anionex/dsh-vision-toolkit> | lib 已提交；需补 saxes + schemastery 别名 |
| dsh-git-identity | <https://github.com/LoserFox/dsh-git-identity> | 零依赖零构建 |
| plugin-console | <https://github.com/vlln/plugin-registry>（子包 `packages/plugin/console`） | 需补 yaml 依赖 |
| dsh-toolkit | <https://github.com/omdsh-dev/dsh-toolkit> | 需构建（10 个子工具 + meta） |
| dsh-sidechain | <https://github.com/Buyi-wsgzg/dsh-sidechain> | 需构建；提供 `/btw` 一次性侧问与 `/side` 侧会话 |
| dsh-upstream-fixes | <https://github.com/qincaizheng/dsh-upstream-fixes> | **必装的修复层**（见 §6，缺它会崩）；git clone + link 安装 + 手动跑别名脚本 |
| dsh-mcp-manager | <https://github.com/hyqhyq3/dsh-mcp-manager> | lib 已提交零依赖；bundle 插件，add 即挂载（设置 → MCP 页） |
| dsh-paste-input | <https://github.com/lhh010/dsh-paste-input> | lib 已提交零依赖；纯插件无 bundle，需手动 insert 行（见 §3） |

### C. dsh-web-ui 全家桶

仓库：<https://github.com/zhu1090093659/dsh-web-ui>（clone 到 `~/.dsh/plugins/dsh-web-ui`，先 `pnpm install` 并按需 `pnpm build`）

| 子包（逐个 add 本地路径） | 作用 |
|---|---|
| dsh-ssh | 远程 SSH 运维（侧边栏） |
| dsh-task-board | 任务看板（cron 定时任务） |
| dsh-aionui-panel | 右侧预览/文件树/SCM 面板 |
| dsh-git-graph | git 提交图 |
| dsh-live-stats | token/TPS 实时统计 |
| dsh-pet | 桌面鲸鱼宠物 |
| dsh-remote-web-ui | 远程 Web UI |
| dsh-web-ui-settings | Web UI 插件组设置卡 |
| skins/skin-center | 皮肤中心（皮肤本身可选，见 §7） |

## 2. 安装步骤

### 2.1 npm 插件

```bash
dsh plugin --profile web add @canglongcl/dsh-web-review
dsh plugin --profile web add @zseven-w/dsh-openpencil
dsh plugin --profile web add dsh-auto-approval
dsh plugin --profile web add dsh-client-ui-auto-approval
```

### 2.2 dsh-web-ui 全家桶

```bash
git clone https://github.com/zhu1090093659/dsh-web-ui.git ~/.dsh/plugins/dsh-web-ui
cd ~/.dsh/plugins/dsh-web-ui && pnpm install
# 逐个挂载（聚合包 web-ui-all 亦可，此处按当前整合包逐包挂载）
for p in dsh-ssh dsh-task-board dsh-aionui-panel dsh-git-graph dsh-live-stats dsh-pet dsh-remote-web-ui dsh-web-ui-settings; do
  dsh plugin --profile web add ~/.dsh/plugins/dsh-web-ui/packages/$p
done
dsh plugin --profile web add ~/.dsh/plugins/dsh-web-ui/packages/skins/skin-center
```

### 2.3 源码插件（通用流程）

```bash
SRC=~/.dsh/plugins
git clone --depth 1 https://github.com/omdsh-dev/DSH-better-sidebar.git $SRC/DSH-better-sidebar
git clone --depth 1 https://github.com/omdsh-dev/dsh-at-file.git       $SRC/dsh-at-file
git clone --depth 1 https://github.com/NanmiCoder/dsh-agent-teams.git  $SRC/dsh-agent-teams
git clone --depth 1 https://github.com/Anionex/dsh-vision-toolkit.git  $SRC/dsh-vision-toolkit
git clone --depth 1 https://github.com/LoserFox/dsh-git-identity.git   $SRC/dsh-git-identity
git clone --depth 1 https://github.com/vlln/plugin-registry.git        $SRC/plugin-registry
git clone --depth 1 https://github.com/omdsh-dev/dsh-toolkit.git       $SRC/dsh-toolkit
git clone --depth 1 https://github.com/Buyi-wsgzg/dsh-sidechain.git    $SRC/dsh-sidechain
git clone --depth 1 https://github.com/qincaizheng/dsh-upstream-fixes.git $SRC/dsh-upstream-fixes
git clone --depth 1 https://github.com/hyqhyq3/dsh-mcp-manager.git     $SRC/dsh-mcp-manager
git clone --depth 1 https://github.com/lhh010/dsh-paste-input.git     $SRC/dsh-paste-input
```

按需装依赖 + 构建：

```bash
# better-sidebar（pnpm install 会跑 prepare 自动构建）
(cd $SRC/DSH-better-sidebar && pnpm install --no-frozen-lockfile)
# sidechain（构建 + 裸 schemastery 别名，见 §4）
(cd $SRC/dsh-sidechain && pnpm install --no-frozen-lockfile && pnpm build)
# toolkit（清掉上游 lockfile 再装，避免 404；再构建全部子包）
(cd $SRC/dsh-toolkit && rm -f package-lock.json && npm install && bash scripts/build-all.sh)
# vision-toolkit（lib 已提交，只补运行时依赖）
(cd $SRC/dsh-vision-toolkit && npm install --no-save --legacy-peer-deps)
# plugin-console（子包；补 yaml 依赖）
(cd $SRC/plugin-registry/packages/plugin/console && pnpm install --config.auto-install-peers=false)
# agent-teams（pnpm 用 --no-frozen-lockfile；其 peer 指向未发布私有包，不要整体装，只做 §4 断链修复）
(cd $SRC/dsh-agent-teams && pnpm install --no-frozen-lockfile || true)
```

挂载：

```bash
dsh plugin --profile web add $SRC/DSH-better-sidebar
dsh plugin --profile web add $SRC/dsh-at-file
dsh plugin --profile web add $SRC/dsh-agent-teams
dsh plugin --profile web add $SRC/dsh-vision-toolkit
dsh plugin --profile web add $SRC/dsh-git-identity
dsh plugin --profile web add $SRC/plugin-registry/packages/plugin/console
dsh plugin --profile web add $SRC/dsh-toolkit
dsh plugin --profile web add $SRC/dsh-sidechain
dsh plugin --profile web add $SRC/dsh-upstream-fixes
dsh plugin --profile web add $SRC/dsh-mcp-manager
dsh plugin --profile web add $SRC/dsh-paste-input

# link: 安装不跑 postinstall —— 手动执行别名修复（同一命令可反复执行作修复）
node $SRC/dsh-upstream-fixes/scripts/install-aliases.mjs
```

## 3. 配置文件追加（一次性）

向 `~/.dsh/profiles/web/cordis.patch.yml` 追加（保留已有的 webserver 行）：

```yaml
# better-sidebar 挂载行（纯 cordis 插件必须）
- insert:
    - id: better-sidebar
      name: 'dsh-better-sidebar'

# dsh-paste-input 挂载行（纯插件无 bundle，同样必须）
- insert:
    - id: dsh-paste-input
      name: '@dsh-community/dsh-paste-input'

# 权限预设：补 auto / auto-review 两档
- id: permission
  config:
    presets:
      read-only:
        sandbox: read-only
        approval: ask
        name: Read-only
        description: Read everything; every mutation requires approval.
      workspace-write:
        sandbox: workspace-write
        approval: ask
        name: Workspace write
        description: Write inside the workspace and permitted temporary directories; wider retries require approval.
      auto:
        sandbox: workspace-write
        approval: never
        name: Auto
        description: Fully autonomous inside the workspace - no prompts, out-of-workspace writes are rejected. Pairs with the auto-approval plugin for classified allow/deny guardrails.
      auto-review:
        sandbox: workspace-write
        approval: ask
        name: Auto review
        description: The auto-approval classifier decides ordinary tool calls; escalation attempts still ask you. Every decision is audited in the session log.
      danger-full-access:
        sandbox: danger-full-access
        approval: never
        name: Full access
        description: Full file access without approval prompts.
```

## 4. 源码插件的三个通用坑（必做修复）

### 4.1 断链：link 指向作者本机 DSH checkout

很多仓库的 lockfile 把 `@deepseek-ai/*` 写成 `link:../dsh/...` 或 `link:../../../.dsh/source/...`，换机器后全是断链。统一改指本机全局 DSH 安装：

```bash
#!/bin/bash
# 用法: fix-dsh-links.sh <插件仓库目录>
repo="$1"
GLOBAL=$(npm root -g)/@deepseek-ai/dsh/node_modules/@deepseek-ai
for link in "$repo"/node_modules/@deepseek-ai/*; do
  [ -L "$link" ] || continue
  name=$(basename "$link")
  [ -e "$link" ] && continue
  [ -e "$GLOBAL/$name" ] && rm "$link" && ln -s "$GLOBAL/$name" "$link"
done
```

### 4.2 裸 `schemastery` 别名（vision-toolkit / sidechain）

上游源码 import 的是不带 scope 的 `schemastery`：

```bash
GLOBAL=$(npm root -g)/@deepseek-ai/dsh/node_modules/@deepseek-ai
ln -sfn "$GLOBAL/schemastery" "$SRC/dsh-vision-toolkit/node_modules/schemastery"
ln -sfn "$GLOBAL/schemastery" "$SRC/dsh-sidechain/node_modules/schemastery"
```

### 4.3 未发布的私有依赖（agent-teams）

`dsh-agent-teams` 的 peer 里含未发布包（`@deepseek-ai/dsh-compact` 等），整体 `pnpm install` 必 404。处理：不追求完整安装，只把它的 `@deepseek-ai/*` peer 软链到全局 DSH 安装（同 §4.1 的 GLOBAL 路径，逐个 `ln -s`），宿主运行时按名解析即可。

## 5. 装完验证（重启前）

```bash
# 1) 组合树无 "entry not found"（仅皮肤警告可忽略）
dsh web --dump-config 2>&1 | grep -c 'not found'

# 2) 每个新插件宿主模块能 import
cd ~/.dsh/profiles/web
for pkg in @loserfox/git-identity dsh-at-file dsh-agent-teams @dsh-external/dsh-vision-toolkit \
           @canglongcl/dsh-web-review @zseven-w/dsh-openpencil @dsh-external/plugin-console \
           @deepseek-ai/dsh-toolkit dsh-better-sidebar dsh-auto-approval \
           @dsh-external/dsh-sidechain @dsh-external/dsh-upstream-fixes; do
  node --input-type=module -e "import('$pkg').then(()=>console.log('OK $pkg')).catch(e=>{console.error('FAIL $pkg', e.message); process.exit(1)})" || echo "== $pkg 加载失败 =="
done

# 3) upstream-fixes 的 scoped 别名已就位（缺了重启必崩，见 §6）
ls ~/.dsh/profiles/node_modules/@deepseek-ai/dsh-auto-approval \
   ~/.dsh/profiles/node_modules/@deepseek-ai/dsh-client-ui-auto-approval

# 4)（可选）冒烟启动：不占 3080，用补丁换端口试跑
cat > /tmp/test-port.yml <<'EOF'
- id: webserver
  config:
    host: 127.0.0.1
    port: 3099
EOF
dsh web --patch /tmp/test-port.yml   # 起来后 curl http://127.0.0.1:3099/ 看启动清单
```

## 6. 为什么 dsh-upstream-fixes 必装（两个真实崩溃点）

1. **dsh-auto-approval 的 scoped 名 bug**：它的 `cordis.patch.yml` 挂载名写的是 `@deepseek-ai/dsh-auto-approval`（带 scope），但 npm 上发布的是裸名 `dsh-auto-approval`。没有修复层时，重启 `dsh web` 直接 `ERR_MODULE_NOT_FOUND` 崩掉。
2. **dsh-sidechain 的深路径 import bug**：它的 client bundle require `@deepseek-ai/dsh-client-runtime/src/...`（不在客户端模块表里），没有修复层时 Web UI 报 `missed the module table` 加载失败。

修复层做的事：`scripts/install-aliases.mjs` 在 `$DSH_HOME/profiles/node_modules` 建 scoped→裸名软链（pnpm 不管理该目录，profile 重装不丢）+ 以 `immediately` 优先级注册 client shim。**安装方式为 git clone + 本地路径 link：pnpm 对 link: 依赖不跑 postinstall，所以必须手动执行 `node <克隆目录>/scripts/install-aliases.mjs`；该命令幂等，链接失效时重跑即可。**（若改从 registry 安装，postinstall 会自动跑。）

## 7. 可选：皮肤

皮肤包（qq98/ths/xp/blue-fantasy/dragon-heir/minecraft/whale-song）默认不装。需要时：

```bash
~/.dsh/plugins/dsh-web-ui/scripts/dsh-skin install <皮肤名>   # = dsh plugin add
~/.dsh/plugins/dsh-web-ui/scripts/dsh-skin use <皮肤名>       # 互斥启用，热重载
```

## 8. 激活与验收

```bash
# 重启 dsh web（Ctrl+C 后重新运行），浏览器硬刷新
```

验收点：侧边栏出现 SSH/任务看板/新工作台（better-sidebar）；输入框 `@文件` 提及生效；`/btw 问题`、`/side 问题`、`/side list` 可用；设置→权限出现 5 档（含 Auto / Auto review）+ auto-approval 设置区；composer 旁出现 AA 芯片；网页预览 tab（web-review）可打开。

## 9. 已知风险

- 老插件（vision-toolkit / agent-teams / toolkit 系 rc.1 时代源码）已按本机 DSH rc.6 重建/链接；**升级 DSH 后需重跑 §4 的断链修复与构建**。
- 两个目录做兼容情报：<https://github.com/AdamPlatin123/awesome-dsh-plugins>（每日兼容矩阵）、<https://github.com/0xsline/awesome-deepseek-harness>。

## 10. 卸载/回滚

```bash
dsh plugin --profile web remove <包名>          # npm/裸名插件
dsh plugin --profile web remove <本地路径对应的包名>  # link 插件，包名见 profile package.json
# better-sidebar 还要删 cordis.patch.yml 里的 insert 段
# upstream-fixes 卸载：dsh plugin --profile web remove @dsh-external/dsh-upstream-fixes
# 再删 clone 目录与它建的 scoped 软链（~/.dsh/profiles/node_modules/@deepseek-ai/dsh-{auto-approval,client-ui-auto-approval}）
```
