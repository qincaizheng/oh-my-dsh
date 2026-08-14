# oh-my-dsh — DSH Integration Pack

[中文](./README.zh.md) | English

One-command integration pack for DeepSeek Harness (DSH): 16 curated plugin groups plus out-of-the-box configuration — the Web UI suite, a coding toolchain, the permission "auto" tier, `/btw` side-asks, and a compatibility fix layer for third-party plugins.

## What's inside

See [PLUGINS.md](./PLUGINS.md) for the full, authoritative plugin list (package name / source / install method / mount per entry).

## Quick start

The authoritative installation manual is [INSTALL.md](./INSTALL.md). Hand the prompt below to any AI / Agent and it will read the manual and perform the full install:

```text
Read https://github.com/qincaizheng/oh-my-dsh/blob/main/INSTALL.md and treat it as the single authoritative installation manual. Install and configure the full integration pack on this machine's DeepSeek Harness (dsh web) environment: compare PLUGINS.md against this machine's installed plugins first, install only the missing plugins (skip everything already installed), apply the config additions, run the repairs and verifications from the manual, restart dsh web, and report the result back to me.
```

## Repository layout

```text
.
├── README.md      # English intro (default) + install prompt for AI agents
├── README.zh.md   # Chinese version of README.md
├── PLUGINS.md     # Authoritative plugin list: package name / source / install method / mount
└── INSTALL.md     # Installation manual: compare → install missing → config → verify → rollback
```

## Notes

- Verified against DSH 0.1.0-rc.6. Some older plugins (agent-teams / toolkit originate from the rc.1 era) were rebuilt/relinked against rc.6 — **after upgrading DSH, re-run the repairs and rebuilds in INSTALL.md §4**.
- Third-party compatibility intelligence: <https://github.com/AdamPlatin123/awesome-dsh-plugins> (daily compatibility matrix), <https://github.com/0xsline/awesome-deepseek-harness> (hand-curated directory).
- `dsh-upstream-fixes` is this pack's fix layer and repairs two real crash points; without it, restarting `dsh web` fails — see INSTALL.md §6.