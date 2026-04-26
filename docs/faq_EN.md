# ❓ FAQ — Hermes Agent / OpenClaw

🌐 **English** | [中文](faq.md)

This document collects frequently asked questions about installing and using Agent Pack (Hermes Agent / OpenClaw).

---

## Q1: The window suddenly closed / froze during installation. Now what?

Don't worry — every platform writes a full log:

- **Windows**: `C:\Users\<your-username>\AppData\Local\AgentPack\logs\`
- **macOS**: `/private/tmp/agent-pack-postinstall.log`
- **Linux**: the terminal prints a `Full log:` line with the path

Send us the log and we can usually pinpoint the issue 95% of the time.

---

## Q2: After installing on Windows, typing `hermes` says "command not found".

Open a fresh PowerShell window and try again. The installer adds entries to your system PATH, but existing terminal windows don't pick them up automatically — a new window will.

---

## Q3: My China network is slow / can't reach GitHub.

The installer auto-detects China-region networks and switches to domestic mirrors (ghproxy, TUNA, npmmirror, Aliyun PyPI) — no manual config needed. If auto-detection fails, set the environment variable manually before running the installer:

- **Windows PowerShell**:

  ```powershell
  $env:AGENTPACK_CN = "1"
  ```

  Then launch the installer.

- **macOS / Linux terminal**:

  ```bash
  export AGENTPACK_CN=1
  ```

  Then launch the installer.

---

## Q4: How do I change the API key or model?

- **Hermes**: edit `~/.hermes/.env` (on Windows that's `\\wsl$\Ubuntu\home\<username>\.hermes\.env`) and update `OPENROUTER_API_KEY=...`.
- **OpenClaw**: in the terminal, run:

  ```bash
  openclaw config set agents.defaults.model openrouter/<new-model-name>
  ```

Or simply re-run the installer — it's idempotent. Reinstalling overrides the old config without breaking anything else.

---

## Q5: How do I uninstall completely?

- **Windows**: Control Panel → Programs → find "Agent Pack" → Uninstall.
- **macOS / Linux**:

  ```bash
  rm -rf ~/.agent-pack ~/.hermes ~/.openclaw
  # If you want to keep the chat history, back these directories up before deleting.
  ```

---

## Q6: OpenClaw web UI won't open / port is in use.

OpenClaw uses port `18789` by default. If it's taken, the gateway picks another port automatically — read the URL printed after `Then open:` at startup; don't hard-code `18789`.

To stop / start the gateway manually:

```bash
openclaw gateway stop
openclaw gateway
```

---

## Q7: Can I try it without an API key?

Yes — leave the API key field blank during installation. Once installed, register or apply for a key, then edit `~/.hermes/.env` (or `~/.openclaw/.env`) and fill it in.

---

[← Back to main README](../README_EN.md)
