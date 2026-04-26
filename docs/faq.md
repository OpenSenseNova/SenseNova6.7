# ❓ 常见问题 — Hermes Agent / OpenClaw

🌐 [English](faq_EN.md) | **中文**

本文档收录 Agent Pack（Hermes Agent / OpenClaw）安装与使用过程中的常见问题。

---

## Q1：安装过程中窗口突然关了 / 卡住了？

别慌，每个平台都留了完整日志：

- **Windows**：`C:\Users\<你的用户名>\AppData\Local\AgentPack\logs\`
- **macOS**：`/private/tmp/agent-pack-postinstall.log`
- **Linux**：终端里有 `Full log:` 后面带路径

把日志发给我们，95% 能定位到问题。

---

## Q2：Windows 装完后输入 `hermes` 提示 "命令未找到"？

重开一个 PowerShell 窗口再试。安装器给系统 PATH 加了条目，但旧窗口不会自动刷新，开新的就好。

---

## Q3：国内网络很慢 / 连不上 GitHub？

安装器会自动检测国内网络并切换到国内镜像（ghproxy、TUNA、npmmirror、阿里云 PyPI），不用手动配置。如果自动检测没成功，在运行安装器之前手动设置环境变量：

- **Windows PowerShell**：

  ```powershell
  $env:AGENTPACK_CN = "1"
  ```

  再双击安装器。

- **macOS / Linux 终端**：

  ```bash
  export AGENTPACK_CN=1
  ```

  再跑安装器。

---

## Q4：想换 API Key / 换模型怎么办？

- **Hermes**：编辑 `~/.hermes/.env`（Windows 上是 `\\wsl$\Ubuntu\home\<用户名>\.hermes\.env`），修改其中的 `OPENROUTER_API_KEY=...`
- **OpenClaw**：在终端运行：

  ```bash
  openclaw config set agents.defaults.model openrouter/<新的模型名>
  ```

或者干脆再跑一次安装器 —— 它是幂等的，重新装一遍会覆盖旧配置，不会搞坏别的。

---

## Q5：想彻底卸载？

- **Windows**：控制面板 → 程序 → 找到 "Agent Pack" → 卸载
- **macOS / Linux**：

  ```bash
  rm -rf ~/.agent-pack ~/.hermes ~/.openclaw
  # 如果想保留会话历史，把这三个目录先备份一下再删
  ```

---

## Q6：OpenClaw 网页打不开 / 端口被占了？

OpenClaw 默认用端口 `18789`。如果被其他程序占了，网关会自动选另一个端口 —— 看 OpenClaw 启动时打印的 `Then open:` 后面那个地址，别硬记 `18789`。

手动停 / 启网关：

```bash
openclaw gateway stop
openclaw gateway
```

---

## Q7：我没 API Key 能先试试吗？

可以，安装时 API Key 那一栏留空就行。装完后再去注册 / 申请，然后编辑 `~/.hermes/.env`（或 `~/.openclaw/.env`）补上即可。

---

[← 返回主 README](../README.md)
