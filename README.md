# bl_sbx
## itunesstored & bookassetd Sandbox Escape

This repository contains a proof-of-concept demonstrating how maliciously crafted `downloads.28.sqlitedb` and `BLDatabaseManager.sqlite` databases can escape the sandbox of **itunesstored** and **bookassetd** on iOS. By abusing their download mechanisms, the POC enables writing arbitrary `mobile`-owned files to restricted locations in `/private/var/`, including MobileGestalt cache files—allowing device modifications such as spoofing the device type.

### Key Points
- Compatible with iOS **26.2b1 and below** (tested on iPhone 12, iOS 26.0.1).
- **Stage 1 (itunesstored):** Delivers a crafted `BLDatabaseManager.sqlite` to a writable container.
- **Stage 2 (bookassetd):** Downloads attacker-controlled EPUB payloads to arbitrary file paths.
- Writable paths include:
  - `/private/var/containers/Shared/SystemGroup/.../Library/Caches/`
  - `/private/var/mobile/Library/FairPlay/`
  - `/private/var/mobile/Media/`
- Demonstrates modifying `com.apple.MobileGestalt.plist` to validate successful exploitation.

### Outcome
iOS fails to block crafted download tasks, allowing unauthorized file writes unless the target path requires `root` ownership (or the fileowner is not `mobile`).

**Check the blogpost for more information**

### Disclaimer
This project is for **educational purposes only**.  
Do **not** use it for illegal activities.  
Apple may patch this behavior at any time.


# 📖 Usage Instructions

> ⚠️ **IMPORTANT:** Run this tool with **Administrator (Windows)** or
> privileges.\
> The exploit requires creating a secure USB/Network tunnel, which the
> OS blocks without elevated rights.

------------------------------------------------------------------------

## 🔧 Step 1: Preparation

-   Connect your **iPhone** to your PC via **USB**\
    *(Tap "Trust" on your iPhone if prompted)*\
-   Ensure you have your **modded `com.apple.MobileGestalt.plist`**\
    *(must match your exact device model)*

------------------------------------------------------------------------

## 🚀 Step 2: Launch the Tool

Open **Command Prompt (CMD)** or **PowerShell** as Administrator,
navigate to the tool folder, then run:

``` bash
python launcher.py
```

------------------------------------------------------------------------

## 🛠 Step 3: Configure

-   **UDID** → The tool should auto-detect.
    -   If it doesn't, click **SCAN ↻**.
-   **Local Plist** → Click **BROWSE** and select your modded `.plist`.
-   **Target Path** → Click **AUTO FILL** (yellow button).
    -   The correct `/private/var/...` path will be inserted
        automatically.
-   Click **RUN EXPLOIT** (red button).

------------------------------------------------------------------------

## 📲 Step 4: Trigger the Exploit

Watch the **SYSTEM LOG**.

When you see:

> **Please open Books app and download a book to continue.**

Do the following:

1.  Unlock your iPhone\
2.  Open the **Books** app\
3.  Download **any** book

Then wait until the tool reports:

> ✅ **SUCCESS**

Your device will automatically **respring/reboot**.
>It is recommended to **reboot using 3uTools**.
