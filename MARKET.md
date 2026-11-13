# 外部智能体接入（@eduwork/dsh-external-agents）

> 发现并连接本机的 Codex、Cursor Agent 等外部智能体（ACP/stdio）。
>
> Discover and connect local external agents such as Codex and Cursor Agent over ACP/stdio.

## 功能

在设置里检测、启用、停用外部智能体，也可手动添加支持 stdio 的 ACP 智能体；配套的官方 Codex/ACP Provider 必须与宿主 DSH 版本一致。

## 安装

```bash
# 在 DSH 中把本目录作为本地 bundle 安装（绝对路径）
```

安装后由 DSH 插件系统加载，条目 id 为 `dsh-external-agents`。

## 依赖

- `@deepseek-ai/dsh-subagent-codex`
- `@deepseek-ai/dsh-subagent-acp`
- `zod`
- `react`
- `@deepseek-ai/dsh-tool-subagent`
- `@deepseek-ai/dsh-storage-domain`
- `@deepseek-ai/dsh-typert-protocol`

## 兼容性

- `engines.dsh`: `>=0.2.1 <0.3.0`
- 与 EduWork 发行版同源，来源于 ECNU/EduWork 仓库。

## 注意

源码包未包含构建产物 lib/，发布前需在包目录执行 pnpm install && pnpm build（详见 BUILD.md）。

## 许可

MulanPSL-2.0 AND MIT（与上游 EduWork 一致）。

## 入口修正


- 移除悬空声明：files: docs/**/*（目录不存在）
