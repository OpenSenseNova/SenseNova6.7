# 🍎 macOS 安装指南 — Hermes Agent / OpenClaw

本文档介绍在 macOS 上通过 [Agent Pack](https://github.com/SenseTime-FVG/agent_pack) 安装 **Hermes Agent** 与 **OpenClaw** 的完整步骤。

---

## 1. 先装两个前置工具（只需要一次）

1. 按 `⌘ Command` + `空格`，输入 **终端**，回车打开终端
2. 粘贴这条命令，回车：

   ```bash
   xcode-select --install
   ```

3. 接着粘贴这条命令（装 Homebrew，是 macOS 的软件管理器）：

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

4. 装完后，Apple Silicon（M1/M2/M3）机器还要再跑一次：

   ```bash
   echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
   eval "$(/opt/homebrew/bin/brew shellenv)"
   ```

   > Intel Mac 跳过这一步。

---

## 下一步

安装完成后，请返回主 README 查看 [开始使用](../README.md#-开始使用)、[常见问题](../README.md#-常见问题) 与 [反馈与支持](../README.md#-反馈与支持) 章节。
