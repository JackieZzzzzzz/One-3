# One-3：一主三子（手动开启）

Codex 主控按需协调规划师、DSH 执行者和独立审查者。默认关闭，仅用户主动调用时对指定任务启用；不固定主控或子角色的模型。

## 使用

在技能列表选择“一主三子（手动开启）”，或输入：

```text
$one-main-three 请完成以下任务：……
```

主控根据目标、风险和已有方案选择角色，不要求三个角色全部启动。指定任务完成、取消或开启新会话后关闭；中途可说“关闭一主三子”。普通聊天、讨论技能和任务复杂都不构成开启。元数据设置 `allow_implicit_invocation: false`。

## 在其他机器安装

先安装并配置 Codex。下面将仓库直接放入本机用户技能目录，**不要克隆进业务项目**。如果目标目录已有同名技能，先备份并核对，再决定如何迁移；不要直接覆盖。

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/JackieZzzzzzz/One-3.git "${CODEX_HOME:-$HOME/.codex}/skills/one-main-three"
```

技能未出现在列表时重启 Codex。也可下载 GitHub 的版本 ZIP，解压后将含 `SKILL.md` 与 `agents/` 的文件夹命名为 `one-main-three`，放到上述用户技能目录。

## 更新和固定版本

安装目录直接作为本机维护源，不在各业务项目另存技能副本。通过 GitHub 同步到其他机器：

```bash
git -C "${CODEX_HOME:-$HOME/.codex}/skills/one-main-three" status --short
git -C "${CODEX_HOME:-$HOME/.codex}/skills/one-main-three" pull --ff-only
```

本机有未提交修改时先处理，不能盲目覆盖或强推。维护后提交并推送到本仓库；已有会话不会因文件更新自动重新加载，新调用再核对版本。

需要固定版本可在干净的安装目录执行 `git fetch --tags` 后检出对应 tag；这是 detached HEAD，回到 `main` 后再正常更新。首次发布为 `v0.1.0`。

## 项目接入前提

本仓库只发布技能，不包含账号认证、DSH 安装、研究数据或业务代码。使用协作的目标项目需已经具备：

- `AGENTS.md`、`STATUS.md` 和 `docs/agent_collaboration.md`。
- `scripts/delegate_dsh.py`、`scripts/delegate_codex.py` 及其运行环境。
- `.codex/agents/planner.toml`、`reviewer.toml` 角色定义。
- 可用的 Git、Python 3.11+、Codex 与 DSH；分别配置好模型路由、认证和网络。

缺少接口时，技能会说明前提，不自动移植旧项目或发起模型请求。项目中的执行器和角色定义是接入文件，不是第二份技能。只安装技能不代表任意项目已经接通 DSH；完整接入包需单独迁移。

## 主控的介入边界

主控派发前只做足以明确约束、接口和验收的调查；具体实现调查交给执行者。正常执行中等待，遇到失败、超时、阻塞、用户询问或明确异常才深入检查。交付后先核对摘要、差异和关键验证；未变且无新疑点的材料与检查复用。

执行者保留原始证据并报告限制。主控仍负责验收；独立上下文不等于操作系统安全隔离，模型意见不等于测试通过或研究有效性。
