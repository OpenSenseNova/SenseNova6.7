# 🍎 macOS 安装指南 — Hermes Agent / OpenClaw

本文档介绍在 macOS 上通过 [Agent Pack](https://github.com/SenseTime-FVG/agent_pack) 安装 **Hermes Agent** 与 **OpenClaw** 的完整步骤。

---

## 1. 前置工具安装

> 只需进行一次。

1. 在 App 中搜索 `terminal` 或 **终端**，打开终端。

   <p align="center">
     <img src="../assets/macos1.png" alt="打开终端" width="70%">
   </p>

2. 打开终端之后，依次复制粘贴并执行以下命令：

   ```bash
   xcode-select --install
   ```

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

   根据提示操作，按下回车键继续。

   <p align="center">
     <img src="../assets/macos2.png" alt="执行 xcode-select 与 Homebrew 安装命令" width="70%">
   </p>

3. 安装完成：接下来进入 **安装 Agent Pack** 阶段。

   <p align="center">
     <img src="../assets/macos3.png" alt="前置工具安装完成" width="70%">
   </p>

---

## 2. 安装 Agent Pack

1. 下载安装器：[AgentPack-1.0.10-macos-universal.pkg](https://github.com/SenseTime-FVG/agent_pack/releases/download/v1.0.10/AgentPack-1.0.10-macos-universal.pkg)，打开进行安装。

   > ⚠️ **如果提示安全隐私问题**：前往 **系统设置 → 隐私与安全性 → 安全性**，会看到对应应用的 "**打开 / 仍要打开**" 按钮。该按钮通常只会在你刚尝试打开后的约 1 小时内出现。输入登录密码后即可放行，之后它会被记为例外。

2. 进入安装界面。

   <p align="center">
     <img src="../assets/macos4.png" alt="进入安装界面" width="70%">
   </p>

   每一步都点击 **继续**，中间可能会需要输入用户密码。

3. 进入安装步骤的时候会提示需要安装的 Agent，**建议一次只选择一个**，示例选择使用 **OpenClaw**。

   <p align="center">
     <img src="../assets/macos5.png" alt="选择安装的 Agent" width="70%">
   </p>

4. 之后提示选择 LLM 供应商：选择 **Custom Endpoint**，然后填入：

   ```
   https://token.sensenova.cn/v1
   ```

   <p align="center">
     <img src="../assets/macos6.png" alt="选择 Custom Endpoint 并填入 Base URL" width="70%">
   </p>

5. 填入模型 ID：`sensenova-6.7-flash-lite`

   <p align="center">
     <img src="../assets/macos7.png" alt="填入模型 ID" width="70%">
   </p>

6. 填入 API Key：`sk-*********`

   <p align="center">
     <img src="../assets/macos8.png" alt="填入 API Key" width="70%">
   </p>

7. 验证配置（失败也没有关系，有可能是系统缺少验证组件导致）。

   <p align="center">
     <img src="../assets/macos9.png" alt="验证配置" width="48%">
     &nbsp;&nbsp;
     <img src="../assets/macos10.png" alt="验证结果" width="48%">
   </p>

8. 点击 **开始安装**。

   <p align="center">
     <img src="../assets/macos11.png" alt="开始安装" width="70%">
   </p>

9. 允许 **控制终端**。

   <p align="center">
     <img src="../assets/macos12.png" alt="允许控制终端" width="70%">
   </p>

10. 之后会弹出终端，等待安装完成即可。

    <p align="center">
      <img src="../assets/macos13.png" alt="终端执行安装脚本" width="70%">
    </p>

11. 之后会自动弹出 OpenClaw 的 dashboard 页面。

    <p align="center">
      <img src="../assets/macos14.png" alt="OpenClaw dashboard 页面" width="70%">
    </p>

---

## 下一步

安装完成后，请返回主 README 查看 [开始使用](../README.md#-开始使用)、[常见问题](../README.md#-常见问题) 与 [反馈与支持](../README.md#-反馈与支持) 章节。
