# DSH 整合包 · 安装指南（给执行安装的模型/Agent 看）

> 目标读者：负责部署本整合包的 AI 模型。本文是安装手册（从哪装、怎么装、怎么验证）；**插件清单的唯一权威是 [PLUGINS.md](./PLUGINS.md)**。
> 目标环境：已装 DSH（`dsh web` 可跑），Node ≥ 20，pnpm ≥ 10，web profile 位于 `~/.dsh/profiles/web/`。

## 0. 总原则

1. 所有安装统一走 `dsh plugin --profile web add <包名|本地路径>`（底层转发 pnpm，装完自动把带 `dsh.bundle` 声明的包 reconcile 进 `dsh.profile.bundles` 层栈）。
2. 纯 cordis 插件（无 `dsh.bundle`，如 dsh-better-sidebar）必须额外在 profile 的 `cordis.patch.yml` 写 insert 挂载行。
3. **装完全部插件后必须重启 `dsh web` 并硬刷新浏览器**（Cmd/Ctrl+Shift+R）才生效。
4. 源码插件常见三个坑，通用修复见 §4。
5. **幂等安装**：每次执行先按 §1 对比本机已装插件，**只安装缺失项**；已安装的条目一律跳过，不重装、不重复写配置。
6. **禁用清单**：每次安装/更新后按 §2.4 处理 [DISABLED.md](./DISABLED.md)——清单内已安装的条目写禁用行，并在最终汇报里告知用户重新启用入口。

## 1. 插件清单与本机对比（先做，每次必做）

完整插件清单（唯一权威）在 [PLUGINS.md](./PLUGINS.md)：每个条目含「包名（profile 依赖键）」「来源」「安装方式」「挂载」四列。

**每次执行都先与本机已装插件对比，只安装缺失项：**

```bash
# 1) 本机已装插件集合（profile 依赖键，与 PLUGINS.md 的「包名」列一一对应）
node -e "console.log(Object.keys(require(process.env.HOME + '/.dsh/profiles/web/package.json').dependencies ?? {}).sort().join('\n'))"

# 2) 逐行对照 PLUGINS.md：
#    - 已安装的条目 → 跳过（不重装、不改配置、不重写 insert 行）
#    - 缺失的条目   → 记下它的「安装方式」，跳到 §2 执行对应命令
# 3) 需要手动 insert 挂载行的插件（PLUGINS.md「挂载」列标出：dsh-better-sidebar / dsh-paste-input）
#    还要确认挂载行是否已存在，计数为 0 才追加：
grep -c 'id: better-sidebar' ~/.dsh/profiles/web/cordis.patch.yml
grep -c 'id: dsh-paste-input' ~/.dsh/profiles/web/cordis.patch.yml
# 重复 insert 会导致启动报错，务必只写一次。
```

## 2. 安装步骤

> 本节命令按 §1 的对比结果**只对缺失条目执行**；全部已装的条目直接跳过本节。

### 2.1 npm 插件

```bash
dsh plugin --profile web add @canglongcl/dsh-web-review
dsh plugin --profile web add @zseven-w/dsh-openpencil
dsh plugin --profile web add @dsh-plugin/dsh-thought-buddy
dsh plugin --profile web add @dsh-plugin/dsh-auxiliary
dsh plugin --profile web add dsh-notify
```

### 2.2 dsh-web-ui 全家桶（聚合包）

推荐 npm 一键安装（上游已发布到 `@linxin666` scope，一个依赖键装齐 9 个功能插件 + 皮肤 + compat shim）：

```bash
dsh plugin --profile web add @linxin666/dsh-web-ui-all
```

> 首次安装若提示 `ERR_PNPM_IGNORED_BUILDS`（pnpm 拒绝依赖构建脚本），按提示把 `cloudflared` / `cpu-features` / `ssh2` 加入 profile 的 `pnpm-workspace.yaml` 的 `allowBuilds` 后重跑即可。

仓库源码安装（改代码调试用，需 Node ≥ 22 + pnpm）：

```bash
SRC=~/.dsh/plugins
git clone https://github.com/zhu1090093659/dsh-web-ui.git $SRC/dsh-web-ui
(cd $SRC/dsh-web-ui && pnpm install --no-frozen-lockfile && pnpm -r build)
node $SRC/dsh-web-ui/scripts/link-profile.mjs   # 把子包链接进 profile 命名空间
dsh plugin --profile web add link:$SRC/dsh-web-ui/packages/dsh-web-ui-all
```

### 2.3 源码插件（通用流程）

```bash
SRC=~/.dsh/plugins
git clone --depth 1 https://github.com/omdsh-dev/DSH-better-sidebar.git $SRC/DSH-better-sidebar
git clone --depth 1 https://github.com/omdsh-dev/dsh-at-file.git       $SRC/dsh-at-file
git clone --depth 1 https://github.com/NanmiCoder/dsh-agent-teams.git  $SRC/dsh-agent-teams
git clone --depth 1 https://github.com/LoserFox/dsh-git-identity.git   $SRC/dsh-git-identity
git clone --depth 1 https://github.com/vlln/plugin-registry.git        $SRC/plugin-registry
git clone --depth 1 https://github.com/omdsh-dev/dsh-toolkit.git       $SRC/dsh-toolkit
git clone --depth 1 https://github.com/Buyi-wsgzg/dsh-sidechain.git    $SRC/dsh-sidechain
git clone --depth 1 https://github.com/ZhuRuoLing/dsh-command-approve-for-me.git $SRC/dsh-command-approve-for-me
git clone --depth 1 https://github.com/ZhuRuoLing/dsh-client-plugin-approve-for-me.git $SRC/dsh-client-plugin-approve-for-me
git clone --depth 1 https://github.com/qincaizheng/dsh-upstream-fixes.git $SRC/dsh-upstream-fixes
git clone --depth 1 https://github.com/hyqhyq3/dsh-mcp-manager.git     $SRC/dsh-mcp-manager
git clone --depth 1 https://github.com/lhh010/dsh-paste-input.git     $SRC/dsh-paste-input
git clone --depth 1 https://github.com/dsh-external/dsh-annotation.git  $SRC/dsh-annotation
git clone --depth 1 https://github.com/mafeis/dsh-net-proxy.git        $SRC/dsh-net-proxy
```

按需装依赖 + 构建：

```bash
# better-sidebar（pnpm install 会跑 prepare 自动构建）
(cd $SRC/DSH-better-sidebar && pnpm install --no-frozen-lockfile)
# sidechain（构建 + 裸 schemastery 别名，见 §4）
(cd $SRC/dsh-sidechain && pnpm install --no-frozen-lockfile && pnpm build)
# toolkit（清掉上游 lockfile 再装，避免 404；再构建全部子包）
(cd $SRC/dsh-toolkit && rm -f package-lock.json && npm install && bash scripts/build-all.sh)
# plugin-console（子包；补 yaml 依赖）
(cd $SRC/plugin-registry/packages/plugin/console && pnpm install --config.auto-install-peers=false)
# agent-teams（pnpm 用 --no-frozen-lockfile；其 peer 指向未发布私有包，不要整体装，只做 §4 断链修复）
(cd $SRC/dsh-agent-teams && pnpm install --no-frozen-lockfile || true)
# approve-for-me 主插件（lib 已提交零构建；peer 全部软链全局 rc.6，见 §4.3）
bash <整合包工作区>/scripts/link-dsh-peers.sh $SRC/dsh-command-approve-for-me
# approve-for-me 前端插件（host 半部仅依赖 zod）
mkdir -p $SRC/dsh-client-plugin-approve-for-me/node_modules
[ -e $SRC/dsh-client-plugin-approve-for-me/node_modules/zod ] || ln -s $(npm root -g)/@deepseek-ai/dsh/node_modules/zod $SRC/dsh-client-plugin-approve-for-me/node_modules/zod
# dsh-net-proxy（lib 已提交零构建；peer 软链全局 rc.6，同 §4.3）
bash <整合包工作区>/scripts/link-dsh-peers.sh $SRC/dsh-net-proxy
```

挂载：

```bash
dsh plugin --profile web add $SRC/DSH-better-sidebar
dsh plugin --profile web add $SRC/dsh-at-file
dsh plugin --profile web add $SRC/dsh-agent-teams
dsh plugin --profile web add $SRC/dsh-git-identity
dsh plugin --profile web add $SRC/plugin-registry/packages/plugin/console
dsh plugin --profile web add $SRC/dsh-toolkit
dsh plugin --profile web add $SRC/dsh-sidechain
dsh plugin --profile web add $SRC/dsh-upstream-fixes
dsh plugin --profile web add $SRC/dsh-mcp-manager
dsh plugin --profile web add $SRC/dsh-paste-input
dsh plugin --profile web add $SRC/dsh-annotation
dsh plugin --profile web add $SRC/dsh-command-approve-for-me
dsh plugin --profile web add $SRC/dsh-client-plugin-approve-for-me
dsh plugin --profile web add $SRC/dsh-net-proxy

# link: 安装不跑 postinstall —— 手动执行别名修复（同一命令可反复执行作修复）
node $SRC/dsh-upstream-fixes/scripts/install-aliases.mjs
```

### 2.4 禁用清单处理（每次安装/更新后执行）

先读 [DISABLED.md](./DISABLED.md) 的禁用清单。对清单里每条（把表格「插件 id」列逐个代入；当前清单：`ui-dsh-aionui-panel`、`live-stats`、`dsh-sidechain`）：检查本机组合树里是否存在对应 id（存在且未禁用才需要处理；插件已被卸载的不存在，跳过）：

```bash
for id in ui-dsh-aionui-panel live-stats dsh-sidechain; do
  echo "== $id =="
  dsh web --dump-config 2>&1 | grep "id: $id"
done
```

存在 → 向 `~/.dsh/profiles/web/cordis.patch.yml` **末尾**追加该条目的禁用行。幂等：追加前先确认该 id 尚无 `disabled: true` 行（`grep -A1 'id: <条目id>' ~/.dsh/profiles/web/cordis.patch.yml` 输出含 `disabled: true` 即已处理，跳过）：

```yaml
# 禁用清单（DISABLED.md）：默认禁用，重新启用入口见该文件
- id: ui-dsh-aionui-panel
  disabled: true

- id: live-stats
  disabled: true

- id: dsh-sidechain
  disabled: true
```

全部条目处理完后，**最终汇报必须包含**：被默认禁用了哪些插件、每个插件的重新启用入口（见 DISABLED.md「重新启用入口」列）。重新启用 = 删除对应禁用行后重启 `dsh web`。

## 3. 配置文件追加（一次性、幂等）

> 追加前先按 §1 第 3 步 grep 检查：挂载行计数为 0 才追加；权限预设若已存在同名条目也不要重复追加。

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

# dsh-auxiliary 自 0.4.2 起内置 bundle patch，随 `dsh plugin add` 自动挂载，
# 无需手动 insert 行（0.4.2 之前版本才需要；如已按旧手册写过 insert 行，升级后请删除该行以免重复）

# approve-for-me 挂载行（主插件：审批 answerer + 审查预设 + 沙箱「替我同意」选项）
- insert:
    - id: approve-for-me
      name: 'dsh-plugin-approve-for-me'
      config:
        mode: review

# approve-for-me-ui 挂载行（前端：会话流里的审查状态行）
- insert:
    - id: approve-for-me-ui
      name: 'dsh-client-plugin-approve-for-me'

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
        description: Fully autonomous inside the workspace - no prompts, out-of-workspace writes are rejected. For classifier-based guardrails use the approve-for-me presets.
      auto-review:
        sandbox: workspace-write
        approval: ask
        name: Auto review
        description: Legacy preset kept for compatibility; approval behavior now comes from the approve-for-me presets (or native prompts when review mode is not active).
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

### 4.2 裸 `schemastery` 别名（sidechain）

上游源码 import 的是不带 scope 的 `schemastery`：

```bash
GLOBAL=$(npm root -g)/@deepseek-ai/dsh/node_modules/@deepseek-ai
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
for pkg in @loserfox/git-identity dsh-at-file dsh-agent-teams \
           @canglongcl/dsh-web-review @zseven-w/dsh-openpencil @dsh-external/plugin-console \
           @deepseek-ai/dsh-toolkit dsh-better-sidebar dsh-plugin-approve-for-me \
           dsh-client-plugin-approve-for-me @dsh-external/dsh-sidechain @dsh-external/dsh-upstream-fixes \
           @dsh-plugin/dsh-thought-buddy @dsh-plugin/dsh-auxiliary dsh-notify dsh-net-proxy; do
  node --input-type=module -e "import('$pkg').then(()=>console.log('OK $pkg')).catch(e=>{console.error('FAIL $pkg', e.message); process.exit(1)})" || echo "== $pkg 加载失败 =="
done

# 3)（可选）冒烟启动：不占 3080，用补丁换端口试跑
cat > /tmp/test-port.yml <<'EOF'
- id: webserver
  config:
    host: 127.0.0.1
    port: 3099
EOF
dsh web --patch /tmp/test-port.yml   # 起来后 curl http://127.0.0.1:3099/ 看启动清单
```

## 6. dsh-upstream-fixes（修复层，当前为惰性保险）

修复层最初修复两个真实崩溃点，现已随插件替换/禁用而失效，但保留安装以兼容历史配置：

1. **旧 dsh-auto-approval 的 scoped 名 bug**（已移除）：它曾在 `cordis.patch.yml` 里用带 scope 的挂载名，缺修复层时重启即 `ERR_MODULE_NOT_FOUND`。该插件已被 approve-for-me 替代，此问题不再存在。
2. **dsh-sidechain 的深路径 import bug**（默认禁用）：它的 client bundle require `@deepseek-ai/dsh-client-runtime/src/...`（不在客户端模块表里），没有修复层时 Web UI 报 `missed the module table` 加载失败。**只有重新启用 sidechain（见 DISABLED.md）时才需要修复层**。

修复层做的事：`scripts/install-aliases.mjs` 在 `$DSH_HOME/profiles/node_modules` 建 scoped→裸名软链（pnpm 不管理该目录，profile 重装不丢）+ 以 `immediately` 优先级注册 client shim。**安装方式为 git clone + 本地路径 link：pnpm 对 link: 依赖不跑 postinstall，所以必须手动执行 `node <克隆目录>/scripts/install-aliases.mjs`；该命令幂等。**（若改从 registry 安装，postinstall 会自动跑。）

## 7. 可选：皮肤

皮肤随聚合包内置（dsh-skins：qq98/ths/xp/blue-fantasy/dragon-heir/minecraft/whale-song/miku/trading），无需单独安装，在 GUI「皮肤中心」（skin-center）内切换即可。

旧版 `dsh-skin` CLI 已废弃。若全局 `~/.dsh/cordis.patch.yml` 残留旧 CLI 生成的皮肤管理块（`# --- dsh-skin managed ---` 段），可整块删除——它只会产生 "entry not found" 警告；皮肤切换由聚合包的皮肤中心接管。

## 8. 激活与验收

```bash
# 重启 dsh web（Ctrl+C 后重新运行），浏览器硬刷新
```

验收点：侧边栏出现 SSH/任务看板/新工作台（better-sidebar）；输入框 `@文件` 提及生效；`/btw 问题`、`/side 问题`、`/side list` 可用；设置→权限出现 5 档；会话流出现 approve-for-me 审查状态行；沙箱权限下拉出现「替我同意 / Approve For Me」与「Approve For Me - Strict Mode」（可用 `/permission approve-for-me` 切换）；网页预览 tab（web-review）可打开；模型思考时出现 thought-buddy 小表情（「Deep diving...」前）；auxiliary 的 `inspect_image` 工具可用。

> approve-for-me 的 `mode: review` 只有会话选中 approve-for-me / strict-review 预设后才调用审查模型；未选中时审批仍走原生弹窗。

> dsh-sidechain 默认禁用（DISABLED.md）：安装后 `/btw`、`/side` 不出现属预期，重新启用入口见 DISABLED.md。

## 9. 已知风险

- 老插件（agent-teams / toolkit 系 rc.1 时代源码）已按本机 DSH rc.6 重建/链接；**升级 DSH 后需重跑 §4 的断链修复与构建**。
- **dsh-auxiliary 的设置命名空间 `dsh-auxiliary` 不在 rc.6 的 `WEB_SETTINGS_NAMESPACES` 白名单**：设置页「辅助模型」表单不可用，配置直接写 `~/.dsh/settings.yaml` 的 `dsh-auxiliary:` 段（键名与示例见插件 README）。
- dsh-web-ui 全家桶已适配当前 DSH 壳（compat shim 内置）；**升级 DSH 后若侧边栏入口不显示，先更新全家桶仓库（`git pull` + `pnpm -r build` + `scripts/link-profile.mjs`）再重启 dsh web**。
- 两个目录做兼容情报：<https://github.com/AdamPlatin123/awesome-dsh-plugins>（每日兼容矩阵）、<https://github.com/0xsline/awesome-deepseek-harness>。

## 10. 卸载/回滚

```bash
dsh plugin --profile web remove <包名>          # npm/裸名插件
dsh plugin --profile web remove <本地路径对应的包名>  # link 插件，包名见 profile package.json
# better-sidebar 还要删 cordis.patch.yml 里的 insert 段
# upstream-fixes 卸载：dsh plugin --profile web remove @dsh-external/dsh-upstream-fixes
# 再删 clone 目录与它建的 scoped 软链（~/.dsh/profiles/node_modules/@deepseek-ai/dsh-{auto-approval,client-ui-auto-approval}）
# approve-for-me 回滚：remove 两个 link 包 + 删 cordis.patch.yml 里 approve-for-me / approve-for-me-ui 的 insert 段 + 删 clone 目录
# 若要恢复旧 auto-approval：npm 包重新 add 两个包，并重新执行 upstream-fixes install-aliases.mjs（见 §6）
```