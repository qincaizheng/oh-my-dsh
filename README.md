# oh-my-dsh — DSH Integration Pack

[中文](./README.zh.md) | English

One-command integration pack for DeepSeek Harness (DSH): 14 curated plugin groups plus out-of-the-box configuration — the Web UI suite, a coding toolchain, the permission "auto" tier, `/btw` side-asks, and a compatibility fix layer for third-party plugins.

## What's inside

| Category | Plugins |
|---|---|
| Web UI suite | ssh · task-board · aionui-panel · git-graph · live-stats · pet · remote-web-ui · web-ui-settings · skin-center |
| Coding workbench | dsh-better-sidebar (files/terminal/Git sidebar) · dsh-at-file (@file mentions) · dsh-web-review (web preview & annotations) · dsh-openpencil (design preview) · dsh-toolkit (10 tools) · dsh-git-identity (commit identity) |
| Vision | dsh-vision-toolkit (screenshot OCR / UI restoration) |
| Multi-agent | dsh-agent-teams |
| Permission extensions | dsh-auto-approval (two-state auto classifier) + auto / auto-review presets |
| Side-asks | dsh-sidechain (`/btw` one-shot side question, `/side` persistent side thread) |
| Plugin management | plugin-console (browser panel for profile plugin state) |
| Fix layer | dsh-upstream-fixes (repairs two boot-breaking bugs in dsh-auto-approval and dsh-sidechain — **required**) |

## Quick start

The authoritative installation manual is [INSTALL.md](./INSTALL.md). Hand the prompt below to any AI / Agent and it will read the manual and perform the full install:

```text
Read https://github.com/qincaizheng/oh-my-dsh/INSTALL.md and treat it as the single authoritative installation manual. Install and configure the full integration pack on this machine's DeepSeek Harness (dsh web) environment: install every listed plugin, apply the config additions, run the repairs and verifications from the manual, restart dsh web — do not skip any plugin or verification step — and report the result back to me.
```

## Repository layout

```text
.
├── README.md      # English intro (default) + install prompt for AI agents
├── README.zh.md   # Chinese version of README.md
└── INSTALL.md     # Authoritative manual: checklist / steps / config / repairs / verification / rollback
```

## Notes

- Verified against DSH 0.1.0-rc.6. Some older plugins (vision-toolkit / agent-teams / toolkit originate from the rc.1 era) were rebuilt/relinked against rc.6 — **after upgrading DSH, re-run the repairs and rebuilds in INSTALL.md §4**.
- Third-party compatibility intelligence: <https://github.com/AdamPlatin123/awesome-dsh-plugins> (daily compatibility matrix), <https://github.com/0xsline/awesome-deepseek-harness> (hand-curated directory).
- `dsh-upstream-fixes` is this pack's fix layer and repairs two real crash points; without it, restarting `dsh web` fails — see INSTALL.md §6.
