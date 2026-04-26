# 🪟 Windows Install Guide — Hermes Agent / OpenClaw

🌐 **English** | [中文](install-windows.md)

This guide walks through installing **Hermes Agent** and **OpenClaw** on Windows via [Agent Pack](https://github.com/SenseTime-FVG/agent_pack).

---

## 1. Install WSL2 (one-time)

WSL2 is the "Linux container" Microsoft ships with Windows — Agent Pack relies on it under the hood.

1. Press the `Win` key and type `cmd`.
2. Right-click the search result and choose **"Run as administrator"** (this matters).
3. Paste into the black window:

   ```powershell
   wsl --install
   ```

4. Press Enter and wait — it takes a few minutes.

   👉 This step automatically:
   - Enables the WSL feature
   - Installs the Virtual Machine Platform
   - Installs the Linux kernel
   - Installs Ubuntu by default

5. Reboot.
6. After reboot, Windows pops up a window asking you to set an Ubuntu username and password — you can skip it.

---

## 2. Install Agent Pack

1. Download the installer: [AgentPack-1.0.10-windows-x64.exe](https://github.com/SenseTime-FVG/agent_pack/releases/download/v1.0.10/AgentPack-1.0.10-windows-x64.exe).
2. Double-click to open the installer.
3. Pick **Hermes** or **OpenClaw** (one at a time).

   <p align="center">
     <img src="../assets/install_page.PNG" alt="Pick which product to install" width="70%">
   </p>

4. Fill in the LLM configuration.

   > ⚠️ `verify` may fail because the system is missing some components. That's fine — continue to the next step.

   <p align="center">
     <img src="../assets/install2.PNG" alt="Enter the LLM configuration" width="70%">
   </p>

5. Click **Next** to start the installation.

---

## 3. Run the install

1. A `cmd` window pops up automatically and starts running the installer script.

   <p align="center">
     <img src="../assets/install3.PNG" alt="cmd opens automatically and runs the installer" width="70%">
   </p>

2. Installation complete.

   <p align="center">
     <img src="../assets/install4.png" alt="Install finished" width="70%">
   </p>

---

## Next steps

Once installation finishes, head back to the main README to see the [Getting Started](../README_EN.md#-getting-started), [FAQ](../README_EN.md#-faq-), and [Feedback & Support](../README_EN.md#-feedback--support) sections.
