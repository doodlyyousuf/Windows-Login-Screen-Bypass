# Windows Login Screen Bypass & Application Execution

> ⚠️ **Educational Purpose Only**: This repository documents a known Windows security vulnerability for educational and defensive purposes. The goal is to highlight potential weaknesses to help improve operating system security. Use responsibly and legally.

🟢 **Status:** Tested and confirmed working as of today, **August 24, 2026**.

## 📖 Overview

This repository demonstrates an advanced technique to bypass the Windows login screen and execute applications directly without authenticating. It covers methods to access a restricted command prompt, elevate it to a full-fledged terminal, and run standard user applications (including multi-process apps) seamlessly on the lock screen.

## ⚙️ How It Works

The bypass relies on replacing a legitimate System32 executable with the Command Prompt (`cmd.exe`). When triggered from the lock screen, Windows executes the command prompt with elevated privileges, allowing you to spawn a secondary terminal and launch applications.

---

## 🕵️ Stealth Method: The `narrator.exe` Alternative

While the classic method involves replacing `sethc.exe` (Sticky Keys) and pressing the **Shift key 5 times**, this action is heavily monitored by modern Endpoint Detection and Response (EDR) systems and Windows Defender.

> 💡 **Pro Tip**: Instead of `sethc.exe`, rename `narrator.exe` to `narrator.exe.bak` and copy `cmd.exe` as `narrator.exe`. 
> 
> **Why?** It works exactly the same but avoids the 5-shift key press detection. You simply trigger it by clicking the **Accessibility/Ease of Access** menu on the login screen and selecting **Narrator**. This is significantly stealthier.

---

## 🚀 Step-by-Step Execution Guide

### Step 1: Choose Your Entry Method (Get to System32)

You need to access the `C:\Windows\System32` folder to replace the executable. You can do this via a Bootable USB OR the built-in Windows Recovery Environment.

#### Method A: Using a Bootable USB (Classic)
1. Create a bootable USB drive using Windows Installation Media or a Linux distribution.
2. Boot the target machine from this USB.
3. Open the file manager and navigate to the Windows partition: `C:\Windows\System32`.

#### Method B: Using Windows Recovery Environment (No USB Required)
*Note: This method requires access to the login screen and may require the local Administrator password or BitLocker recovery key to open the Command Prompt in WinRE.*
1. On the Windows login screen, hold down the **Shift** key and click the **Power icon -> Restart**.
2. The system will boot into the blue Windows Recovery Environment (WinRE).
3. Navigate to **Troubleshoot** > **Advanced options** > **Command Prompt**.
4. *(If prompted, select the Administrator account and enter the password).*
5. Once the Command Prompt opens, navigate to the System32 folder. 
   > ⚠️ **Important Note for WinRE:** The Windows drive letter often changes in Recovery Mode (e.g., it might be `D:` or `E:` instead of `C:`). Type `dir C:\Windows` or `dir D:\Windows` to find the correct drive letter.
6. Navigate to that drive's System32 folder (e.g., `D:\Windows\System32`).

### Step 2: Modify System Files
Whichever method you used in Step 1, once you are inside the `System32` folder:
1. *(Recommended Stealth Method)*: 
   - Rename `narrator.exe` to `narrator.exe.bak`.
   - Copy `cmd.exe` and paste it in the same folder, renaming the copy to `narrator.exe`.
2. *(Alternative Classic Method)*: Replace `sethc.exe` with `cmd.exe`.
3. Close the file manager or command prompt.
   - *If using Method B (WinRE)*: Select **Continue** to boot normally back into Windows.

### Step 3: Trigger the Bypass
1. Wait for the computer to reach the normal login screen.
2. Trigger the modified executable:
   - **If using Narrator method**: Click the Accessibility icon (or press `Win + Ctrl + Enter`) and select **Narrator**.
   - **If using Sticky Keys method**: Press the **Shift key 5 times**.
3. A Command Prompt window will open.

### Step 4: Elevate the Command Prompt
The initial prompt may have minor restrictions. To bypass them and get a full-fledged terminal:
```cmd
cmd
```
*Press Enter. You now have an unrestricted command prompt.*

### Step 5: Execute Applications
You can now launch applications directly. For example, to run Google Chrome:

```cmd
cd "C:\Program Files\Google\Chrome\Application"
chrome.exe
```
💥 **Boom!** Chrome is now running directly on the lock screen.

---

## ✨ Key Features & Capabilities

- **Multiple Entry Vectors**: Works via external Bootable USB OR internally via Windows Recovery Environment (Shift+Restart).
- **No Login Required**: Access and run user applications without authenticating.
- **Multi-Process Support**: Fully supports modern, multi-process applications (no legacy single-process limitations).
- **Full System Tool Access**: The elevated CMD allows you to run advanced utilities like Computer Management (`compmgmt.msc`), Registry Editor (`regedit`), and more.
- **Data Persistence**: Applications run in the context of the user profile. If you open Chrome, browse, and later log into the account normally, all your tabs, cache, and session data will be saved and intact—no worries!
- **Stealth Execution**: The `narrator.exe` method bypasses common heuristic triggers associated with the Sticky Keys exploit.

---

## 📌 Additional Notes

- Always verify the exact installation path of the application you wish to run. The Chrome path provided is the default, but it may vary based on user installation choices.
- This technique exploits local physical access or recovery access. Ensuring your device is protected with **BitLocker** or similar full-disk encryption is the primary mitigation against this attack.

---

## 🤝 Contribution

If you have ideas for improvements, alternative execution methods, or better defensive mitigations, please feel free to reach out or open a discussion. Collaboration is welcome. 

If you repost this technique or reference it in your work, please credit the original author.

---

## ⚖️ Disclaimer

This technique is shared strictly for **educational and defensive purposes**. Understanding how attackers bypass security controls is essential for building better defenses. 

**Use at your own risk.** Bypassing security measures on systems you do not own or have explicit permission to test is illegal and unethical. Always act responsibly and within the bounds of the law.

---

## 📜 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.
