# Windows Login Screen Bypass & Application Execution

> ⚠️ **Educational Purpose Only**  
> This repository documents a known Windows security vulnerability for educational and defensive purposes. The goal is to highlight potential weaknesses so operating system security can be improved. Use responsibly and legally.

🟢 **Status:** Tested and confirmed working as of **August 24, 2026**.

---

## Overview

This repository demonstrates a technique to bypass the Windows login screen and execute applications without authenticating. It covers:

- Accessing a restricted command prompt from the lock screen
- Elevating that prompt to a full terminal
- Launching standard user applications (including multi-process apps) while the machine remains locked

---

## How It Works

The bypass works by replacing a legitimate System32 executable with `cmd.exe`. When the replaced binary is triggered from the login screen, Windows launches an elevated Command Prompt. From there you can spawn additional processes and run applications.

---

## Stealth Method: Using `narrator.exe` (Recommended)

The classic method replaces `sethc.exe` (Sticky Keys) and is triggered by pressing **Shift** five times. This behavior is heavily monitored by modern EDR solutions and Windows Defender.

**Better approach:**

1. Rename `narrator.exe` → `narrator.exe.bak`
2. Copy `cmd.exe` and rename the copy to `narrator.exe`

**Why this is better:**  
You trigger it by opening the Accessibility / Ease of Access menu on the login screen and selecting **Narrator**. No repeated Shift key presses are required, making it significantly stealthier.

---

## Step-by-Step Guide

### Step 1: Gain Access to System32

You need write access to `C:\Windows\System32`. Two common methods:

#### Method A — Bootable USB

1. Create a bootable USB (Windows Installation Media or a Linux distro).
2. Boot the target machine from the USB.
3. Navigate to the Windows partition → `Windows\System32`.

#### Method B — Windows Recovery Environment (No USB)

> **Note:** This method may require the local Administrator password or BitLocker recovery key.

1. On the login screen, hold **Shift** and click **Power → Restart**.
2. The system boots into the Windows Recovery Environment (WinRE).
3. Go to **Troubleshoot → Advanced options → Command Prompt**.
4. If prompted, select the Administrator account and enter the password.
5. Determine the correct drive letter (it is often not `C:` in recovery):
   ```cmd
   dir C:\Windows
   dir D:\Windows
   dir E:\Windows
   ```
6. Navigate to the correct System32 folder, for example:
   ```cmd
   cd /d D:\Windows\System32
   ```

---

### Step 2: Replace the Target Executable

Once inside System32:

**Recommended (Narrator method):**
```cmd
ren narrator.exe narrator.exe.bak
copy cmd.exe narrator.exe
```

**Classic (Sticky Keys method):**
```cmd
ren sethc.exe sethc.exe.bak
copy cmd.exe sethc.exe
```

Then exit the recovery environment or reboot normally.

---

### Step 3: Trigger the Bypass

1. Boot to the normal Windows login screen.
2. Trigger the modified binary:
   - **Narrator method:** Click the Accessibility icon (or press `Win + Ctrl + Enter`) → select **Narrator**.
   - **Sticky Keys method:** Press the **Shift** key five times.
3. A Command Prompt window appears.

---

### Step 4: Elevate the Command Prompt

The initial prompt can have restrictions. Launch a new instance:

```cmd
cmd
```

You now have a fully functional elevated Command Prompt.

---

### Step 5: Launch Applications

Example — start Google Chrome:

```cmd
cd "C:\Program Files\Google\Chrome\Application"
chrome.exe
```

The application runs on the lock screen. Session data (tabs, cache, etc.) is preserved and will be available after a normal login.

---

## Key Features

- Works via external bootable media **or** Windows Recovery Environment
- No authentication required
- Supports modern multi-process applications
- Full access to system tools (`compmgmt.msc`, `regedit`, etc.)
- Application data and session state persist after login
- Narrator method avoids common Sticky Keys detection heuristics

---

## Additional Notes

- Always confirm the exact path of the application you want to launch — installation locations can vary.
- The primary mitigation against this class of attack is full-disk encryption (BitLocker or equivalent). Physical or recovery access is required for the technique to work.

---

## Contribution

Ideas for improvements, alternative methods, or defensive mitigations are welcome. Please open a discussion or reach out.

If you reference or republish this technique, please credit the original author.

---

## Disclaimer

This material is provided strictly for **educational and defensive** purposes. Understanding how security controls can be bypassed is necessary to improve them.

**Use at your own risk.** Performing these actions on systems you do not own or do not have explicit authorization to test is illegal. Always stay within the law.

---

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.
