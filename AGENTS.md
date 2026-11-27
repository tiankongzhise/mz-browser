# MZ Browser 项目协作规则

> ⚠️ **AI 署名约定**（AI Signature Policy）：用户决定保留历史署名。
> 记录于 2026-11-26，触发场景：chore/fork-build-workflow 首次推送 + 历史检出 65 条署名（均已存在于远端 main）。
> 约定：允许保留署名推送。
> 修改约定请直接编辑本行。

> ⚠️ **贡献者身份约定**（Author Identity Policy）：用户决定 push 前必须统一作者。
> 记录于 2026-11-26，触发场景：chore/fork-build-workflow 首次推送 + 检出 0 个名单外作者。
> 约定：push 前必须统一作者。
> 允许身份：notebook <tiankongzhise@163.com>
> 修改约定请直接编辑本行。

## 适用范围

本文件约束当前本地工作区中的 AI/Agent 行为。它属于个人开发工作流约束，**不得提交到准备 PR 给上游 `mouzhai163/mz-browser` 的功能分支**。

## 仓库分层

- `upstream`：`https://github.com/mouzhai163/mz-browser.git`，代表原项目干净主干。
- `origin`：`git@github-tk:tiankongzhise/mz-browser.git`，代表个人仓库。
- 准备提交给上游的功能分支必须从 `upstream/main` 或其干净祖先创建，只包含上游应收到的产品代码、测试和通用文档。
- 个人工作流文件、个人云端验证 workflow、本文件等 fork-only 内容只能进入个人仓库的专用分支或个人 `main`，不得混入上游功能分支。

## 本地构建约束

- **不要在本地执行 `cargo build --release`、`tauri build` 或完整 Windows 安装包构建。**
- 不在本地生成或保留庞大的 Rust release 构建中间产物；`src-tauri/target/` 已由 Git ignore，必要时应清理而不是提交。
- 本地可以做源码编辑、ESLint/Prettier/TypeScript 等前端轻量检查。
- **本地默认禁止执行 `cargo check`、`cargo test`、`cargo build`、`tauri build` 等会产生 Rust 编译中间体的命令；Rust 编译、检查和测试全部交给 GitHub Actions。** 只有用户明确针对当次任务授权时才能例外。
- 需要测试 exe/安装包时：只提交源码并 `git push` 到 GitHub，由云端 Windows runner 构建，再从 Actions Artifact 或 GitHub Release 下载产物验证。

## 验证约定

- 前端：`pnpm typecheck`、`pnpm test`、`pnpm lint`、`node build.cjs` 可作为云端验证；本地运行前应确认依赖已安装且成本可接受。
- 测试云端 exe/安装包前，先检查是否仍有旧 Artifact 或旧版本实例运行（重点看进程路径是否位于历史 `mz-browser-artifact-*` 目录）。若有，先结束旧实例再测试；应用单实例机制会让后启动实例直接退出，避免把这个问题误判成新产物无法启动。
- Rust：云端 CI 使用 `cargo check --locked`、`cargo test --locked --lib`、`cargo test --locked -p runtime`、`cargo test --locked -p business`。
- 不要为了通过本地或云端检查而关闭安全策略、提交密钥、绕过 branch protection 或修改供应链保护配置。

## Git 与 PR 约定

- 不主动 push，除非用户明确要求。
- push 前确认待推送分支不包含 fork-only 文件、个人规则、临时文件或敏感信息。
- 上游 PR 只能使用干净功能分支；个人验证可以使用个人仓库中的专用分支、PR 或手动 workflow。
