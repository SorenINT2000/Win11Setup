# Windows 11 Setup Guide

## Initial Safety & Tools

Before making system-level changes, establish a recovery point and install management tools.

- Create a System Restore Point
    1. Win + S > "System Restore Point"
    2. Select the `C:` drive and click Configure.
    3. Ensure "Turn on system protection" is toggled on.
    4. Click Create, name it Clean_Install_Base, and click OK.

- Essential Management Apps
    - [Policy Plus](https://github.com/Fleex255/PolicyPlus/releases): For GPO management on Home/Pro.
    - [Process Explorer](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer): Replace Task Manager (`Options` > `Replace Task Manager`).
    - [Everything 1.5a](https://www.voidtools.com/everything-1.5a/) + [EverythingToolbar](https://github.com/srwi/EverythingToolbar/releases).
    - [Power Toys](https://learn.microsoft.com/en-us/windows/powertoys/install).

## Privacy & Telemetry Hardening (GPO)

Open **Policy Plus** and navigate to the following paths. These settings prevent Windows from "self-healing" the telemetry tasks you disable later.

### Data Collection & OneSettings

`Windows Components` > `Data Collection and Preview Builds`
    - **Allow Diagnostic Data**: Set to **Enabled**. This enables the enforcement of a specific configuration level that we can choose.
        - Set the dropdown that appears to "Send required diagnostic data" ("Diagnostic data off" is ignored by Home/Pro and will default to Basic anyway).
    - **Disable OneSettings Downloads**: Set to **Enabled**. OneSettings is known for **re-enabling disabled services (!!)** and sharing information about the system with Microsoft.

### Start Menu & Search

### Settings App
`Privacy` > `Security`
    - Set **Feedback frequency** to **Never**.

## Microsoft Store

- Microsoft Defender
- Microsoft Sticky Notes
- Lenovo Vantage
- Powershell 7
- Windows Terminal

## Fonts

- [Fira Code Nerd Font](https://www.nerdfonts.com/font-downloads)

## CLI Tools

- WSL
- [Starship](https://starship.rs/installing/)
- [Git](https://git-scm.com/install/windows)
- [Github](https://cli.github.com/)
- [fnm](https://github.com/Schniz/fnm) + Node
- [UV](https://docs.astral.sh/uv/getting-started/installation/)

## Apps
- [Firefox Developer Edition](https://www.firefox.com/en-US/channel/desktop/developer/)
- [OBS](https://obsproject.com/)
    - [ScrCpy](https://github.com/Genymobile/scrcpy/blob/master/doc/windows.md)
- [Steam](https://store.steampowered.com/about/)
- [Cursor](https://cursor.com/get-started)
- [Docker](https://www.docker.com/products/docker-desktop/)
- [WindHawk](https://windhawk.net/)

## Windows Settings

- Create a **Windows Developer Drive** to place git-tracked repositories
    - Go to the Dev Home app > Settings > Privacy > toggle off "Send diagnostic data"
    - Enable performance mode in antivirus settings for dev drive

## Git Configuration

### SSH Configs

1. Personal Account SSH

```pwsh
> ssh-keygen -t ed25519 -C "sjschultz11@gmail.com" -f "$HOME\.ssh\id_ed25519_personal"
```

2. Work Account SSH

```pwsh
> ssh-keygen -t ed25519 -C "sschultz@magicmusicapp.com" -f "$HOME\.ssh\id_ed25519_work"
```

3. SSH Config file

```pwsh
> notepad ~\.ssh\config
```

Paste into the SSH config file:
```
# Personal - GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal

# Work - GitLab
Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
```

### Global Git Config for SSH Authentication & Commit Signing

In `~/.gitconfig`:
```
[user]
    name = Soren Schultz
    email = sjschultz11@gmail.com
    signingkey = ~/.ssh/id_ed25519_personal.pub

[gpg]
    format = ssh

[commit]
    gpgsign = true

; If in work directory, use work config
[includeIf "gitdir:D:/MagicMusic/"]
    path = ~/.gitconfig-work
```

In `~/.gitconfig-work`:
```
[user]
    email = sschultz@magicmusicapp.com
    signingkey = ~/.ssh/id_ed25519_work.pub
```

### Github/Gitlab Settings config

- Add personal SSH public key from `~/.ssh/id_ed25519_personal.pub` to github as both an Authentication key and a Signing key.

- Add work SSH public key from `~/.ssh/id_ed25519_work.pub` to gitlab as both an Authentication key and a Signing key.

### Verification
Run:
```pwsh
ssh -T git@github.com
ssh -T git@gitlab.com
```

In any git repository outside ~/MagicMusic:
```pwsh
> git config --show-origin user.email
file:C:/Users/sjsch/.gitconfig  sjschultz11@gmail.com
```

And in any git repository inside ~/MagicMusic:
```pwsh
> git config --show-origin user.email
file:C:/Users/sjsch/.gitconfig-work     sschultz@magicmusicapp.com
```


## Task Scheduler Tasks to Disable

- Microsoft > Windows > Application Experience
    - Disable all
- Microsoft > Windows > Customer Experience Improvement Plan
    - Disable all
- Microsoft > Windows > DiskDiagnostic
    - Disable all
- Microsoft > Windows > Feedback > Siuf
    - Disable all
- Microsoft > Windows > Device Information
    - Disable all
- Microsoft > Windows > Maps
    - Disable all

## Disable Windows Update from Undoing Changes

We need to disable Windows from downloading configuration files to reset any of our configuration settings.
OneSettings is the task that is responsible for this. 

1. Download 

2. Navigate to Windows Components > Data Collection and Preview Builds

3. Set "Disable OneSettings Downloads" to Enabled and "Enable OneSettings Auditing" to Disabled

# WindHawk Configuration

- Windows 11 Taskbar Styler: Luminosity (Aeris)
- Windows 11 Start Menu Styler: TintedGlass
- Windows 11 File Explorer Styler: TintedGlass
- Disable rounded corners in Windows 11 (Settings > Advanced Settings > Add dwm.exe to Process inclusion list)