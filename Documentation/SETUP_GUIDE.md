# Setup Guide (Windows, beginner friendly)

This sets up your PC so Claude Code can build Capital World **on your computer**.
Do the steps in order. Total time is about 2–4 hours, mostly waiting for downloads.
If any step shows an error, copy the exact error text and paste it to Claude.

Your PC: RTX 3070, Ryzen 5 5500, 16 GB RAM, over 200 GB free SSD space. That's enough.
Tip: with 16 GB of RAM, close your browser and other heavy apps while the Unreal Editor is open or code is compiling.

---

## Step 1 — Update your graphics driver
1. Open **NVIDIA App** (or GeForce Experience). If you don't have it, download it from nvidia.com.
2. Install the latest **Game Ready** or **Studio** driver.
3. Restart your PC.

## Step 2 — Install Git for Windows (includes Git LFS)
1. Go to **https://git-scm.com/download/win** and download the 64-bit installer.
2. Run it. **The default options are fine.** Make sure "Git LFS" is ticked. It is by default.
3. Open **PowerShell** (Start menu → type `PowerShell`) and run:
   ```powershell
   git --version
   git lfs install
   ```
   You should see a version number, then `Git LFS initialized.`
4. Tell Git who you are (use your GitHub username and email):
   ```powershell
   git config --global user.name "YOUR_GITHUB_USERNAME"
   git config --global user.email "YOUR_EMAIL"
   ```

## Step 3 — Get the project onto your PC
Use a short folder path that is **not** inside OneDrive. Unreal has problems with long paths and with folders that sync to the cloud.
```powershell
mkdir C:\Dev
cd C:\Dev
git clone https://github.com/callofguns/Capital-World.git
cd Capital-World
```
The first time, a GitHub sign-in window opens. Sign in. Git remembers it after that.

## Step 4 — Make sure GitHub can never charge you
Large game files are stored with Git LFS. GitHub Free currently includes **10 GiB storage and 10 GiB download bandwidth per month**.
If your GitHub account has **no payment method**, GitHub blocks LFS when you hit the limit instead of charging you.
- Check: github.com → your profile picture → **Settings** → **Billing and licensing**. Don't add a card.
- Claude will warn you before storage gets close to the limit.

## Step 5 — Install the Epic Games Launcher and Unreal Engine 5.8
1. Download the **Epic Games Launcher** from **https://www.unrealengine.com/download** and install it. Sign in or create a free Epic account.
2. In the launcher, click **Unreal Engine** (left side) → **Library** tab.
3. Next to **Engine Versions**, click **+**. Pick the newest **5.8.x** in the dropdown.
4. Click **Install**. Before it starts, click the arrow or **Options**:
   - **Untick "Editor symbols for debugging"**. It's huge and you don't need it now.
   - Leave everything else on the defaults.
5. Wait for the download, which is many GB. You don't need to open the editor yet.
6. Write down the exact version number shown, for example `5.8.1`. Claude will ask for it.

## Step 6 — Set up Visual Studio for Unreal C++
You already have Visual Studio. It needs the right version and parts.
1. Open the **Visual Studio Installer** from the Start menu.
2. Check your version. For UE 5.8 you need **Visual Studio 2022 version 17.14 or later**, or **Visual Studio 2026**. If you have an older 2022 version, click **Update** first.
3. Click **Modify** on your Visual Studio and tick these **workloads**:
   - **Desktop development with C++**
   - **Game development with C++**. In the right panel, make sure **Unreal Engine installer** and a **Windows 10 or 11 SDK** are ticked.
   - **.NET desktop development**
4. Click **Modify** or **Install** and wait.
5. Epic's official checklist, if anything looks different:
   https://dev.epicgames.com/documentation/en-us/unreal-engine/setting-up-visual-studio-development-environment-for-cplusplus-projects-in-unreal-engine

## Step 7 — Install Blender (free)
1. Go to **https://www.blender.org/download/** and install the current version (the LTS version is the most stable).
2. Note where it installed, usually `C:\Program Files\Blender Foundation\Blender <version>\blender.exe`. Claude uses this to make low-poly models with scripts.

## Step 8 — Install Claude Code
1. Follow the official instructions: **https://code.claude.com/docs/en/getting-started/installation**
   At the time of writing, the Windows install is this command in PowerShell:
   ```powershell
   irm https://claude.ai/install.ps1 | iex
   ```
   If the official page says something different, follow the page.
2. Close and reopen PowerShell, then check it works:
   ```powershell
   claude --version
   ```

## Step 9 — Start working with Claude on your PC
Claude on your PC **can't see the earlier cloud chat**. Everything decided there is saved in this repo's docs, so it starts with full context.
```powershell
cd C:\Dev\Capital-World
git checkout main
git pull
claude
```
Then paste this as your first message:
> Read CLAUDE.md and everything in Documentation/, starting with PROJECT_STATE.md. Then continue Milestone 0: check my tools are installed correctly, create the CapitalWorld Unreal 5.8 C++ project, compile it, and run the first test. Give me a simple overview when done.

Claude will ask permission before running commands. You can approve each one, or allow the ones that are safe.

---

## Quick checklist
- [ ] Graphics driver updated
- [ ] Git and Git LFS installed (`git lfs install` done)
- [ ] Repo cloned to `C:\Dev\Capital-World`
- [ ] No payment method on GitHub
- [ ] Unreal Engine 5.8.x installed (version noted: ______)
- [ ] Visual Studio has the three workloads
- [ ] Blender installed (path noted: ______)
- [ ] Claude Code installed (`claude --version` works)
