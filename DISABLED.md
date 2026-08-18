# 禁用清单（安装 / 更新后自动处理）

> 整合包的默认禁用模块清单。执行安装或更新的 Agent **必须**按 [INSTALL.md](./INSTALL.md) §2.4 处理本清单：
> 检测到清单内插件已安装（组合树中存在对应 id）→ 在 profile `cordis.patch.yml` 写禁用行 → **最终汇报中告知用户各禁用插件的重新启用入口**。
> 新增禁用项：在表格加一行即可。恢复启用：删除对应禁用行（或改 `disabled: false`）后重启 `dsh --profile oh-my-dsh` 并硬刷新浏览器。
> **聚合包 0.1.20 起 insert id 统一改为 `web-ui-` 前缀**（下表为新 id）；若 profile 残留旧无前缀 id 的禁用行（`ui-dsh-aionui-panel` / `live-stats` / `pet`），删除并改写下表新 id，旧行只会产生 "entry not found" 警告且禁用不生效。

| 插件 id | 插件 | 所属包 | 重新启用入口 |
|---|---|---|---|
| `web-ui-dsh-aionui-panel` | dsh-aionui-panel（右侧面板：文件树 + 预览） | `@linxin666/dsh-web-ui-all`（聚合包内；旧 id `ui-dsh-aionui-panel`） | 删除 `~/.dsh/profiles/oh-my-dsh/cordis.patch.yml` 末尾的 `- id: web-ui-dsh-aionui-panel` + `disabled: true` 两行 → 重启 `dsh --profile oh-my-dsh` → 浏览器硬刷新 |
| `web-ui-live-stats` | dsh-live-stats（输入框下方实时 token / TPS 统计） | `@linxin666/dsh-web-ui-all`（聚合包内；旧 id `live-stats`） | 删除 `~/.dsh/profiles/oh-my-dsh/cordis.patch.yml` 末尾的 `- id: web-ui-live-stats` + `disabled: true` 两行 → 重启 `dsh --profile oh-my-dsh` → 浏览器硬刷新 |
| `dsh-sidechain` | dsh-sidechain（侧链面板：`/side` 与 `/btw` 子代理） | `@dsh-external/dsh-sidechain`（源码 link，见 PLUGINS.md C 组） | 若已被禁用：删除 `~/.dsh/profiles/oh-my-dsh/cordis.patch.yml` 末尾的 `- id: dsh-sidechain` + `disabled: true` 两行 → 重启 `dsh --profile oh-my-dsh`；若已卸载：按 PLUGINS.md C 组重新安装后再重启 |
| `web-ui-describe-image` | dsh-tool-describe-image（图像理解工具） | `@linxin666/dsh-web-ui-all`（聚合包 0.1.20 新增） | 与 dsh-auxiliary 的 `describe_image` 工具重名冲突（`tool "describe_image" is already registered`），保留 dsh-auxiliary。重新启用 = 删除对应禁用两行 → 重启 `dsh --profile oh-my-dsh`（需先卸载/停用 dsh-auxiliary 的同名工具） |
| `better-sidebar` | 独立安装的 dsh-better-sidebar 的 bundle 入口 | `dsh-better-sidebar`（独立 npm/link 安装时） | 聚合包 0.1.20 起已内置挂载（`web-ui-better-sidebar`），独立入口禁用以免重复注册 `/sidebar/api`。无独立安装则跳过本条；想换用独立入口：删除本禁用行并改为禁用 `web-ui-better-sidebar` → 重启 `dsh --profile oh-my-dsh` |
