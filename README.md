# Ultimate Windows Optimization Guide - Script Analysis

> **IMPORTANT DISCLAIMER**: This document provides an in-depth analysis of the PowerShell scripts in this optimization guide. Running these scripts can make significant changes to your Windows system. **Always create a system restore point** before running any optimization scripts, and **understand what each script does** before executing it.

---

## Table of Contents

1. [General Information](#general-information)
2. [1 Check - System Diagnostics](#1-check---system-diagnostics)
3. [2 Refresh - Windows Reinstallation](#2-refresh---windows-reinstallation)
4. [3 Setup - Initial Configuration](#3-setup---initial-configuration)
5. [4 Installers - Software Installation](#4-installers---software-installation)
6. [5 Graphics - GPU & Display Configuration](#5-graphics---gpu--display-configuration)
7. [6 Windows - System Optimization](#6-windows---system-optimization)
8. [7 Hardware - Hardware Optimization](#7-hardware---hardware-optimization)

---

## General Information

### Common Script Features

All scripts in this collection share the following characteristics:

1. **Administrator Privileges**: Every script checks for and requires administrator privileges. If not running as admin, the script will automatically restart itself with elevated permissions.

2. **Download Helper Function**: Most scripts include a `Get-FileFromWeb` function that downloads files from the internet with a progress bar display.

3. **User Interaction**: Scripts use console menus to allow users to choose between options (typically "Optimize" vs "Default").

4. **Registry Modifications**: Many scripts modify Windows Registry values to change system behavior.

### Risk Levels

Each script analysis includes a risk assessment:
- 🟢 **Low Risk** - Reversible changes, minimal system impact
- 🟡 **Medium Risk** - Significant changes but can be reverted
- 🔴 **High Risk** - Potentially destructive or difficult to reverse

---

## 1 Check - System Diagnostics

### 1-Space-Check.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Displays the percentage of free space on the system drive and opens File Explorer to "This PC".

**What it does**:
- Retrieves the system drive (typically C:)
- Calculates and displays free space percentage
- Opens File Explorer for manual inspection

**Warnings**: None. This is a read-only diagnostic script.

---

### 2-Ram-Check.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Downloads and launches CPU-Z to check RAM configuration.

**What it does**:
- Downloads CPU-Z from the official TechPowerUp website
- Extracts and runs the portable version
- Allows user to verify XMP/DOCP/EXPO profiles, RAM slots, and dual-channel configuration

**Warnings**:
- Downloads software from the internet
- CPU-Z is a reputable system information tool

**Why use it**: To verify that RAM is properly configured (XMP enabled, correct slot placement for dual-channel).

---

### 3-Gpu-Check.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Downloads and launches GPU-Z to verify GPU settings.

**What it does**:
- Downloads GPU-Z from TechPowerUp
- Launches the tool for GPU diagnostics
- Helps verify PCIe bus interface speed and monitor connection

**Warnings**:
- Downloads software from the internet
- GPU-Z is a reputable GPU information tool

**Why use it**: To check if GPU is running at full PCIe speed and using the correct display output.

---

### 4-Bios-Update.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Opens a Google search for BIOS updates based on your motherboard model.

**What it does**:
- Uses WMI to retrieve the motherboard product ID
- Opens a web browser with a Google search for that ID

**Warnings**: None. This script only reads system information and opens a browser.

---

### 5-Bios-Settings.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Provides recommended BIOS settings and offers to restart into BIOS.

**What it does**:
- Displays recommended BIOS settings for Intel and AMD systems:
  - **Intel**: Enable XMP, disable C-states, enable Resizable BAR
  - **AMD**: Enable DOCP/EXPO, enable PBO, AMD Cool'n'Quiet, Resizable BAR
- Offers option to restart directly into BIOS/UEFI settings using `shutdown /r /fw`

**Warnings**:
- ⚠️ Incorrectly changing BIOS settings can prevent your system from booting
- ⚠️ C-state and power settings affect power consumption and heat
- Always research settings before changing them in BIOS

---

### 6-Cpu-Test.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Downloads and launches Prime95 for CPU stress testing.

**What it does**:
- Downloads Prime95 from the official website
- Extracts and launches the stress testing tool
- Provides instructions on how to use it

**Warnings**:
- ⚠️ Prime95 will max out your CPU, causing high temperatures
- ⚠️ Can potentially reveal cooling issues or unstable overclocks
- ⚠️ Extended stress testing can wear components faster
- Monitor temperatures during testing

**Signs of problems it helps identify**:
- Stuttering, freezing, BSODs
- Corrupted installations, random crashing
- CRC errors, performance degradation

---

### 7-Ram-Test.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Downloads and configures TM5 (TestMem5) for RAM stability testing.

**What it does**:
- Downloads TM5 (TestMem5) memory testing tool
- Downloads and installs 7-Zip for extraction
- Creates a specific configuration file for thorough testing
- Launches TM5 with the configuration

**Warnings**:
- ⚠️ RAM testing is CPU-intensive and generates heat
- ⚠️ Installs 7-Zip silently on your system
- Testing can take several hours for thorough results

**Troubleshooting advice provided**:
- RAM timing adjustments
- Voltage considerations
- Ordering of RAM sticks

---

### 8-Gpu-Test.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Downloads and launches FurMark for GPU stress testing.

**What it does**:
- Downloads FurMark GPU stress testing tool
- Launches the installer

**Warnings**:
- ⚠️ FurMark creates extreme GPU load and heat
- ⚠️ Monitor GPU temperature carefully
- ⚠️ Can reveal unstable GPU overclocks

**What to monitor**: GPU temperature, framerate stability, visual artifacts, driver crashes

---

### 9-Hw-Info.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Downloads and launches HWiNFO64 for comprehensive hardware monitoring.

**What it does**:
- Downloads HWiNFO64 portable version
- Extracts and launches the monitoring tool

**Warnings**: None. HWiNFO64 is a read-only monitoring tool.

---

## 2 Refresh - Windows Reinstallation

### 3 Reinstall.ps1
**Risk Level**: 🔴 High Risk

**Purpose**: Downloads the official Windows Media Creation Tool for reinstalling Windows.

**What it does**:
- Offers choice between Windows 10 or Windows 11
- Downloads the official Media Creation Tool from Microsoft
- Launches the tool for creating installation media

**Warnings**:
- ⚠️ Reinstalling Windows will erase all data on the target drive
- ⚠️ Backup all important data before proceeding
- This is the official Microsoft tool

---

### 4-Autounattend.ps1
**Risk Level**: 🔴 High Risk

**Purpose**: Creates an autounattend.xml file for automated Windows installation.

**What it does**:
- Generates an unattended installation configuration file
- **Bypasses Windows 11 hardware requirements** (TPM, RAM, Secure Boot, CPU checks)
- Creates a local administrator account with customizable username
- Skips OOBE (Out-of-Box Experience) screens
- Disables telemetry and dynamic updates during setup
- Prompts for USB drive letter to copy the file

**Warnings**:
- ⚠️ **Creates an account with no password by default**
- ⚠️ **Bypasses Windows 11 security requirements** - reduces system security
- ⚠️ Disables Windows activation auto-activation
- ⚠️ Sets locale to US English - may need adjustment for other regions
- The script instructs to disconnect ethernet during installation

**Registry bypasses include**:
- `BypassTPMCheck` - Skips TPM requirement
- `BypassRAMCheck` - Skips RAM requirement
- `BypassSecureBootCheck` - Skips Secure Boot requirement
- `BypassCPUCheck` - Skips CPU compatibility check
- `BypassStorageCheck` - Skips storage requirement

---

### 6-To-Bios.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Restarts the computer directly into BIOS/UEFI settings.

**What it does**:
- Uses `shutdown /r /fw` to restart into firmware settings
- Requires user confirmation before restarting

**Warnings**: Save all work before running - the system will restart immediately after confirmation.

---

## 3 Setup - Initial Configuration

### 1-Activation-Home-To-Pro.ps1
**Risk Level**: 🔴 High Risk

**Purpose**: Assists in upgrading Windows 10/11 Home to Pro edition.

**What it does**:
- Opens Device Manager to disable internet (to prevent activation check)
- Copies a **generic Windows Pro key** to clipboard: `VK7JG-NPHTM-C97JM-9MPGT-3V66T`
- Opens the Windows activation screen
- Creates a desktop shortcut to re-enable internet

**Warnings**:
- ⚠️ **This key is a generic upgrade key, NOT a license**
- ⚠️ You will still need to purchase a Windows Pro license
- ⚠️ Using this without a valid license may violate Microsoft terms
- The generic key only allows the upgrade, not full activation

---

### 9-Background-Apps.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Disables or enables Windows background apps.

**What it does**:
- **Option 1 (Recommended)**: Sets `LetAppsRunInBackground` to `2` (disabled) via Group Policy
- **Option 2**: Removes the restriction, returning to default

**Warnings**:
- ⚠️ Disabling background apps may break some app notifications
- ⚠️ Some apps like Mail, Calendar may not sync properly
- ⚠️ Requires restart to apply

**Benefits**: Reduces system resource usage from background UWP apps.

---

### 10-Edge-Updates.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Opens the uBlock Origin extension page in Microsoft Edge.

**What it does**:
- Simply launches Edge to the uBlock Origin extension page

**Warnings**: None - this just opens a webpage.

---

## 4 Installers - Software Installation

### 1-Installers.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Interactive menu for installing common gaming and productivity software.

**Available Installations**:
| # | Software | Silent? |
|---|----------|---------|
| 2 | 7-Zip | Yes |
| 3 | Battle.net | No |
| 4 | Discord | Yes |
| 5 | Electronic Arts (EA App) | No |
| 6 | Epic Games | Yes |
| 7 | Escape From Tarkov | No |
| 8 | GOG Galaxy | No |
| 9 | Google Chrome | Yes (+ opens uBlock Origin) |
| 10 | League of Legends | No |
| 11 | Notepad++ | Yes |
| 12 | OBS Studio | Yes |
| 13 | Roblox | No |
| 14 | Rockstar Games Launcher | No |
| 15 | Steam | Yes |
| 16 | Ubisoft Connect | Yes |
| 17 | Valorant | No |

**Warnings**:
- ⚠️ Downloads from official sources but URLs may become outdated
- ⚠️ Roblox installer may attempt to reinstall Microsoft Edge
- ⚠️ Some installers run silently without user prompts

**Best Practices Mentioned**:
- Disable hardware acceleration in apps
- Turn off "run at startup"
- Deactivate overlays (reduces GPU usage and latency)

---

### 2-Spotify.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Downloads and installs Spotify.

**What it does**:
- Downloads Spotify installer from official CDN
- Launches the installer

**Warnings**: Standard software installation from official source.

---

## 5 Graphics - GPU & Display Configuration

### 1-Clean-Driver.ps1
**Risk Level**: 🔴 High Risk

**Purpose**: Prepares system for clean GPU driver installation using DDU.

**What it does**:
1. Downloads DDU (Display Driver Uninstaller) from Guru3D
2. Extracts DDU to temp folder
3. Creates a pre-configured settings file with aggressive cleanup options:
   - Remove monitors
   - Remove AMD/NVIDIA directories
   - Remove GeForce Experience
   - Remove PhysX, NVIDIA Control Panel, etc.
   - **Prevent Windows Update from downloading drivers**
4. Creates desktop shortcuts for Safe Mode toggle and DDU
5. **Restarts system into Safe Mode**

**Warnings**:
- ⚠️ **Automatically restarts into Safe Mode**
- ⚠️ You will have no display drivers after running DDU
- ⚠️ Must install new drivers afterward
- ⚠️ Disables Windows Update driver downloads

**Registry modifications**:
- `SearchOrderConfig = 0` - Prevents automatic driver downloads

---

### 2-Amd-Driver.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Downloads and installs AMD Adrenalin graphics driver.

**What it does**:
- Downloads AMD Adrenalin Edition driver (version 24.6.1)
- Runs silent installation

**Warnings**:
- ⚠️ Hardcoded driver version - may become outdated
- ⚠️ Consider using DDU first for clean installation

---

### 4-Nvidia-Driver.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Downloads and installs NVIDIA graphics drivers.

**What it does**:
- **Option 1**: Automatically downloads the latest NVIDIA driver
  - Queries NVIDIA API for latest version
  - Detects Windows version and architecture
  - Downloads, extracts with 7-Zip, and installs
- **Option 2**: Downloads NVCleanstall for custom driver installation

**Warnings**:
- ⚠️ Installs 7-Zip silently as a dependency
- ⚠️ Uses standard NVIDIA installer (includes bloat)
- NVCleanstall option allows more control

---

### 5-Nvidia-Settings.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Applies optimized NVIDIA Control Panel settings via NVIDIA Profile Inspector.

**What it does when "On" is selected**:
1. Sets "Adjust image settings with preview" to Performance mode
2. Downloads NVIDIA Profile Inspector
3. Imports a custom profile with optimized settings:

**Key settings applied**:
| Setting | Value | Purpose |
|---------|-------|---------|
| Maximum pre-rendered frames | 1 | Reduces input lag |
| Vertical Sync | Fast Sync | Low-latency vsync |
| Power management mode | Prefer max performance | Prevents GPU downclocking |
| Texture filtering quality | High performance | Faster rendering |
| Shader cache | Unlimited | Reduces stutter |
| Threaded optimization | On | Better CPU utilization |
| G-SYNC | Off | For competitive gaming |
| VRR | Off | Disabled globally |

**Warnings**:
- ⚠️ Disables G-SYNC globally (may cause tearing)
- ⚠️ Power management at max = higher power/heat
- ⚠️ Some settings may reduce visual quality

---

### 8-Sound.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Opens sound settings and optionally enables Loudness Equalization.

**What it does**:
- Opens the Sounds control panel
- If Loudness EQ selected:
  - Creates a PowerShell script to enable loudness equalization
  - Modifies audio device registry settings
  - Restarts audio service

**Warnings**:
- ⚠️ Modifies audio device registry properties
- ⚠️ May not work with all audio drivers
- ⚠️ May need to rollback audio driver if it fails

---

### 9-Msi-Mode.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Enables or disables MSI (Message Signaled Interrupts) mode for GPUs.

**What it does**:
- Iterates through all display devices
- Sets `MSISupported` registry value to enable/disable MSI mode

**What is MSI Mode?**
MSI mode can reduce interrupt latency for GPU operations, potentially improving performance and reducing input lag in games.

**Warnings**:
- ⚠️ Requires restart to take effect
- ⚠️ Some GPUs may not support MSI mode properly
- ⚠️ Can cause system instability on some hardware

---

### 10-Direct-X.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Installs DirectX End-User Runtime (June 2010).

**What it does**:
- Downloads DirectX redistributable from Microsoft
- Installs 7-Zip for extraction
- Extracts and runs the DirectX installer

**Warnings**:
- This installs legacy DirectX components (d3dx9, d3dx10, d3dx11, etc.)
- Needed for older games that require these DLLs

---

### 11-C++.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Installs all Visual C++ Redistributable packages.

**What it does**:
Downloads and installs VC++ Redistributables for:
- 2005 (x86 & x64)
- 2008 (x86 & x64)
- 2010 (x86 & x64)
- 2012 (x86 & x64)
- 2013 (x86 & x64)
- 2015-2022 (x86 & x64)

**Warnings**:
- All installers run silently
- This is a standard requirement for many applications

---

## 6 Windows - System Optimization

### 1-Start-Menu-Taskbar.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Cleans up or restores the Windows Start Menu and Taskbar.

**What "Clean" option does**:
1. **Taskbar cleanup**:
   - Unpins all taskbar icons except File Explorer
   - Removes Windows Widgets
   - Left-aligns taskbar (Windows 11)
   - Removes Search, Task View, Chat, Copilot buttons
   - Removes Meet Now (Windows 10)
   - Hides Security Center taskbar icon
   - Shows all system tray icons
   
2. **Start Menu cleanup**:
   - Downloads a clean `start2.bin` for Windows 11
   - Creates empty start menu layout for Windows 10

**Warnings**:
- ⚠️ Downloads file from GitHub for Windows 11 start menu
- ⚠️ Kills explorer.exe multiple times
- ⚠️ Requires restart to fully apply

---

### 9-Power-Plan.ps1
**Risk Level**: 🟡 Medium Risk

**Purpose**: Applies an aggressive "Ultimate Performance" power plan with additional tweaks.

**What "On" option does**:
1. Duplicates Windows' hidden "Ultimate Performance" power plan
2. **Deletes all other power plans**
3. Applies aggressive settings:

**Power Settings Applied**:
| Setting | Value | Impact |
|---------|-------|--------|
| Hibernate | Disabled | Saves disk space |
| Sleep | Disabled | Never sleeps |
| Fast Boot | Disabled | Cleaner shutdowns |
| CPU Core Parking | Disabled | All cores always active |
| Power Throttling | Disabled | No thermal throttling |
| USB Selective Suspend | Disabled | USB always powered |
| PCIe Link Power Management | Off | No power saving |
| Minimum Processor State | 100% | CPU always at max |
| Display timeout | Never | Display stays on |

**Laptop-specific settings**:
- Intel Graphics: Maximum performance
- AMD Power Slider: Best performance
- Battery warnings: Disabled
- Battery saver: Disabled

**Warnings**:
- ⚠️ **Significantly increases power consumption**
- ⚠️ **May cause overheating on laptops**
- ⚠️ **Deletes default power plans** (can restore with option 2)
- ⚠️ Disables all battery protection features
- ⚠️ May reduce battery lifespan on laptops

---

### 12-Registry.ps1
**Risk Level**: 🔴 High Risk

**Purpose**: Applies hundreds of registry tweaks for optimization.

**This is a massive script (2200+ lines) that modifies:**

**Accessibility (Disables)**:
- Narrator, Sticky Keys, Filter Keys, Toggle Keys
- All accessibility sound alerts
- High contrast shortcuts

**File Explorer**:
- Opens to "This PC" instead of Quick Access
- Shows file extensions
- Disables recent files in Quick Access
- Enables full path in title bar

**Privacy (Disables)**:
- Location services
- Voice activation
- Notification access
- Account info access
- Contacts, Calendar, Phone calls, Email, Messaging access
- Diagnostic data collection
- Activity history
- Advertising ID

**Gaming**:
- Disables Game Bar/DVR
- Enables Game Mode
- Disables Xbox button on controller

**Visual Effects (Disables)**:
- Window animations
- Menu fade effects
- Tooltip animations
- Window drag previews
- Transparency effects

**System**:
- Disables Remote Assistance
- Disables Automatic Maintenance
- Disables Delivery Optimization
- Disables Windows sounds
- Disables Autoplay
- Sets dark theme

**Performance**:
- Win32PrioritySeparation = 26 (optimizes foreground apps)
- Mouse pointer to "None" scheme
- Disables enhance pointer precision

**Warnings**:
- ⚠️ **Massive number of changes**
- ⚠️ **Disables many Windows features**
- ⚠️ Some apps may not work correctly
- ⚠️ Removes all system sounds
- ⚠️ Changes visual appearance significantly
- ⚠️ "Default" option can restore most settings

---

### 15 Bloatware.ps1
**Risk Level**: 🔴 High Risk

**Purpose**: Removes Windows bloatware and UWP apps.

**What "Remove All Bloatware" does**:

1. **UWP App Removal**:
   - Removes ALL UWP apps except NVIDIA apps
   - Re-installs essential apps:
     - WindowsAppRuntime.CBS (needed for Windows 11 Explorer)
     - HEVC Video Extension
     - HEIF Image Extension
     - Paint (Windows 11)
     - Photos
     - Notepad (Windows 11)

2. **Windows Capabilities Removal**:
   - Steps Recorder
   - Quick Assist
   - Internet Explorer
   - Windows Hello Face
   - Math Recognizer
   - Windows Media Player
   - PowerShell ISE
   - WordPad
   - Windows Fax and Scan
   - OpenSSH Client
   - And more...

3. **Legacy Features Disabled**:
   - WCF Services
   - Print to PDF/XPS
   - Remote Differential Compression
   - SMB 1.0
   - PowerShell 2.0
   - Work Folders Client

4. **Additional Cleanup**:
   - Microsoft Update Health Tools
   - OneDrive
   - Old Snipping Tool
   - Remote Desktop Connection

**Warnings**:
- ⚠️ **Removes Windows Store by default** (can reinstall with option 3)
- ⚠️ **OneDrive is completely uninstalled**
- ⚠️ Many built-in Windows apps will be gone
- ⚠️ Network drivers are preserved (commented out)
- ⚠️ Some apps may be needed for specific use cases

**Recovery Options**:
- Option 3: Reinstall Store
- Option 4: Reinstall ALL UWP apps
- Option 5-9: Various reinstall options

---

## 7 Hardware - Hardware Optimization

### 1-Mouse-Optimization.ps1
**Risk Level**: 🟢 Low Risk

**Purpose**: Downloads Mouse Movement Recorder and provides mouse optimization tips.

**What it does**:
- Downloads Mouse Movement Recorder tool
- Displays optimization recommendations:
  - Keep wireless dongle close to mouse
  - Disable angle snapping
  - Turn off motion sync
  - Set lowest debounce time
  - Use maximum polling rate
  - Recommended DPI settings by resolution

**DPI Recommendations**:
| Resolution | Recommended DPI |
|------------|-----------------|
| 1080p | 400 DPI |
| 1440p | 800 DPI |
| 4K | 1600 DPI |

**Additional Tips**:
- Enable raw input in games
- Set Windows pointer speed to 6/11
- Disable "Enhance pointer precision"
- Use 100% display scaling

**Warnings**:
- High polling rates (8000Hz) may cause issues with some games/PCs
- These are general recommendations, not automated changes

---

## Summary

### Scripts by Risk Level

**🟢 Low Risk (Safe to run)**:
- 1-Space-Check.ps1
- 2-Ram-Check.ps1
- 3-Gpu-Check.ps1
- 4-Bios-Update.ps1
- 6-To-Bios.ps1
- 9-Hw-Info.ps1
- 10-Edge-Updates.ps1
- 2-Spotify.ps1
- 10-Direct-X.ps1
- 11-C++.ps1
- 1-Mouse-Optimization.ps1

**🟡 Medium Risk (Review before running)**:
- 5-Bios-Settings.ps1
- 6-Cpu-Test.ps1
- 7-Ram-Test.ps1
- 8-Gpu-Test.ps1
- 9-Background-Apps.ps1
- 1-Installers.ps1
- 2-Amd-Driver.ps1
- 4-Nvidia-Driver.ps1
- 5-Nvidia-Settings.ps1
- 8-Sound.ps1
- 9-Msi-Mode.ps1
- 1-Start-Menu-Taskbar.ps1
- 9-Power-Plan.ps1

**🔴 High Risk (Understand fully before running)**:
- 3 Reinstall.ps1
- 4-Autounattend.ps1
- 1-Activation-Home-To-Pro.ps1
- 1-Clean-Driver.ps1
- 12-Registry.ps1
- 15 Bloatware.ps1

---

## Best Practices

1. **Always create a System Restore Point** before running any scripts
2. **Run scripts one at a time** and verify changes work correctly
3. **Research any setting** you don't understand before applying it
4. **Keep a backup** of important files
5. **Document changes** you make so you can undo them if needed
6. **Test in non-critical environments** first if possible

---

*Document generated from script analysis. Last updated: December 2024*
