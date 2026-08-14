# 禁用清单（安装 / 更新后自动处理）

> 整合包的默认禁用模块清单。执行安装或更新的 Agent **必须**按 [INSTALL.md](./INSTALL.md) §2.4 处理本清单：
> 检测到清单内插件已安装（组合树中存在对应 id）→ 在 profile `cordis.patch.yml` 写禁用行 → **最终汇报中告知用户各禁用插件的重新启用入口**。
> 新增禁用项：在表格加一行即可。恢复启用：删除对应禁用行（或改 `disabled: false`）后重启 `dsh web` 并硬刷新浏览器。

| 插件 id | 插件 | 所属包 | 禁用原因 | 重新启用入口 |
|---|---|---|---|---|
| `ui-dsh-aionui-panel` | dsh-aionui-panel（右侧面板：文件树 + 预览，常驻 shell 网格列、无整体关闭入口） | `@linxin666/dsh-web-ui-all`（聚合包内） | 面板无法整体关闭，默认不启用 | 删除 `~/.dsh/profiles/web/cordis.patch.yml` 末尾的 `- id: ui-dsh-aionui-panel` + `disabled: true` 两行 → 重启 `dsh web` → 浏览器硬刷新 |
| `live-stats` | dsh-live-stats（输入框下方实时 token / TPS 统计） | `@linxin666/dsh-web-ui-all`（聚合包内） | 用户要求默认禁用 | 删除 `~/.dsh/profiles/web/cordis.patch.yml` 末尾的 `- id: live-stats` + `disabled: true` 两行 → 重启 `dsh web` → 浏览器硬刷新 |
