<div align="center">

  <!-- Header / Poster Banner -->
  <img src="https://capsule-render.vercel.app/api?type=waving&color=auto&height=220&section=header&text=ThermalGuard&fontSize=70&fontColor=fff&animation=fadeIn&fontAlignY=38&desc=Graduated%20Thermal%20Throttling%20for%20Android&descAlignY=62&descAlign=50" width="100%" alt="ThermalGuard Banner" />

  # ÔÜí ThermalGuard
  ### *Graduated Thermal Throttling Module for Magisk, KernelSU, and APatch*
  
  **Developed with ❤️ by 26August**

  [![Root Manager](https://img.shields.io/badge/Root-Magisk%20%7C%20KernelSU%20%7C%20APatch-orange.svg?style=for-the-badge)](#supported-platforms)
  [![Architecture](https://img.shields.io/badge/Arch-arm64-blue.svg?style=for-the-badge)](#supported-platforms)
  [![Android Version](https://img.shields.io/badge/Android-10%2B%20%28API%2029%2B%29-green.svg?style=for-the-badge)](#supported-platforms)
  
  [![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-Donate-yellow.svg?style=for-the-badge&logo=buy-me-a-coffee)](https://buymeacoffee.com/raininaugust26)

  <p align="center">
    <b>Keeps performance high under heat by shaping throttle curves instead of disabling thermal protection. Always has an emergency failsafe.</b>
  </p>

</div>

---

## ­ƒôî Overview

Many phones drop CPU and GPU frequencies too aggressively when hot, causing stuttering in games and heavy apps. **ThermalGuard** shifts and flattens the throttle curve ÔÇö it does **not** turn protection off. 

* When temperature approaches safe limits, the module lowers performance smoothly.
* When critical limits are exceeded, all tweaks are removed and factory defaults are restored.

---

## ÔÜÖ´©Å How It Works

### ­ƒîí´©Å Thermal Zones

| Zone | Device Temp | Action |
| :--- | :--- | :--- |
| **Normal** | Below 38┬░C | No tweaks. All stock settings. |
| **Push** | 38ÔÇô42┬░C | Raise stock throttle thresholds a few degrees. Maintain max clock. |
| **Limit** | 42ÔÇô45┬░C | Step down max frequency 5% per step. Reduce charging current. Throttle background processes. |
| **Critical** | Above 45┬░C | Remove all tweaks. Restore defaults. Lock module for 5 minutes. |

### ­ƒøí´©Å Safety Mechanisms

* **Hard Limits:** CPU/GPU die temperature above **85┬░C** or battery above **48┬░C** always triggers the *Critical zone*, regardless of user settings. These limits cannot be raised from the UI.
* **Hysteresis:** A **2┬░C hysteresis** prevents zone flapping. The temperature must drop below a zone's threshold minus the hysteresis value before downgrading.

---

## Ô£¿ Features

* ­ƒôè **Temperature Monitor** ÔÇö Reads CPU, GPU, battery, and skin temperatures from `/sys/class/thermal` every 2 seconds (adaptive).
* ­ƒÄ» **Zone Engine** ÔÇö Four zones with distinct actions and hysteresis.
* ­ƒÜ¿ **Fail-Safe** ÔÇö Critical temperature or missing sensor: remove all tweaks, restore original values, send notification.
* ­ƒº® **SoC Detection** ÔÇö Automatic path map selection for Qualcomm, MediaTek, Exynos, and Tensor. Unknown devices run in read-only mode.
* ­ƒôë **Step-Wise Frequency** ÔÇö Adjust `scaling_max_freq` for CPU and GPU in small steps, not jumps.
* ­ƒöî **Charging Control** ÔÇö Reduce charging current in Limit zone when screen is on.
* ­ƒô▒ **Per-App Profiles** ÔÇö Detect foreground application; activate Gaming or Saver profile automatically.
* ­ƒîÉ **WebUI** ÔÇö Dashboard, profile editor, security panel, and event history. Works with KernelSU and APatch managers.
* ­ƒöä **Bootloop Protection** ÔÇö If boot fails twice consecutively, the module disables itself automatically.

---

## ­ƒô▓ Supported Platforms

| Platform / Requirement | Details |
| :--- | :--- |
| **Magisk** | v24+ |
| **KernelSU** | Any recent version |
| **APatch** | Any recent version |
| **Architecture** | `arm64` only |
| **Android Version** | Android 10+ (API 29+) |

---

## ­ƒÜÇ Installation

1. Download the latest release `.zip` file.
2. Flash via your root manager (**Magisk**, **KernelSU**, or **APatch**).
3. Reboot your device.
4. Open the **WebUI** from your root manager to configure.

### ­ƒöì Automated Installer Actions
The installer automatically:
* Detects your SoC and loads the appropriate path map.
* Tests writable thermal and CPU nodes.
* Backs up all original values to `/data/adb/thermalguard/backup/`.
* Checks for conflicting thermal modules (refuses to install alongside them).

---

## ­ƒÄø´©Å Profiles

| Profile | Description |
| :--- | :--- |
| **Automatic** | Balanced behavior. Scales with heat; no manual override. |
| **Gaming** | Keeps max clocks longer. Gradual step-down when limit is reached. |
| **Saver** | Aggressive cooling. Caps clocks earlier; keeps battery cool. |

> **Note:** Profile thresholds can be tuned in the WebUI. Values that would exceed safe zones are clamped at the slider boundary and labeled.

---

## ­ƒôé Directory Structure

```text
thermalguard/
Ôö£ÔöÇÔöÇ module.prop              # Module metadata
Ôö£ÔöÇÔöÇ customize.sh             # Installer: SoC detection, node checks, backup
Ôö£ÔöÇÔöÇ post-fs-data.sh          # Early boot: boot marker, initial failsafe check
Ôö£ÔöÇÔöÇ service.sh               # Daemon: temp monitor ÔåÆ zone engine ÔåÆ apply actions
Ôö£ÔöÇÔöÇ bin/
Ôöé   Ôö£ÔöÇÔöÇ zone_engine.sh       # Zone logic + hysteresis
Ôöé   ÔööÔöÇÔöÇ failsafe.sh          # Restore original values, lock module
Ôö£ÔöÇÔöÇ soc/                     # Path maps per chipset
Ôöé   Ôö£ÔöÇÔöÇ qcom.conf
Ôöé   Ôö£ÔöÇÔöÇ mtk.conf
Ôöé   Ôö£ÔöÇÔöÇ exynos.conf
Ôöé   ÔööÔöÇÔöÇ tensor.conf
Ôö£ÔöÇÔöÇ config/
Ôöé   ÔööÔöÇÔöÇ profiles.json        # Thresholds and actions per profile
Ôö£ÔöÇÔöÇ backup/                  # Original values (created at install time)
Ôö£ÔöÇÔöÇ state/                   # Runtime state (created at install time)
Ôö£ÔöÇÔöÇ logs/                    # Daemon and install logs
Ôö£ÔöÇÔöÇ sepolicy.rule            # SELinux rules
ÔööÔöÇÔöÇ webroot/                 # WebUI
    Ôö£ÔöÇÔöÇ index.html
    Ôö£ÔöÇÔöÇ app.js
    ÔööÔöÇÔöÇ style.css
	
	

## Data Locations

| Path | Purpose |
|------|---------|
| `/data/adb/thermalguard/status.json` | Real-time status (zone, temps, freqs) |
| `/data/adb/thermalguard/backup/` | Original sysfs values before modification |
| `/data/adb/thermalguard/state/` | Zone state, lockout timer, history |
| `/data/adb/thermalguard/logs/daemon.log` | Daemon activity log |
| `/data/adb/thermalguard/config/profiles.json` | Profile configuration |

## Compatibility Notes

- Requires root with write access to `/sys/class/thermal` and `/sys/devices/system/cpu`. The installer tests these nodes at installation time.
- Some phones lock thermal in firmware or TrustZone. On these devices, module effects are partial and the UI shows a warning.
- SELinux rules are applied via `sepolicy.rule`. SELinux is never set to permissive.
- The module detects and refuses to install alongside other thermal modules.

## Safety

This module shapes throttle curves. It does **not**:
- Spoof temperature sensors
- Disable battery protection
- Overclock beyond specifications

Hard limits (CPU/GPU die 85┬░C, battery 48┬░C) are enforced in code and cannot be bypassed from the UI.

**Disclaimer:** Use at your own risk. Modifying thermal behavior can affect device longevity and warranty coverage.

## Adding SoC Support

If your device is detected as unknown (read-only mode):

1. Identify your chipset and available thermal sysfs nodes.
2. Create a new `.conf` file in `soc/` following the format of existing maps.
3. Test that the paths are correct for your device.
4. Submit a pull request with the new SoC profile.

Full instructions: [docs/SOC_GUIDE.md](docs/SOC_GUIDE.md)

## Community & Security

| Document | Purpose |
|----------|---------|
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to report bugs, propose features, and open PRs |
| [SECURITY.md](SECURITY.md) | Private vulnerability reporting and project scope |
| [docs/FAILSAFE_EXPLAINED.md](docs/FAILSAFE_EXPLAINED.md) | Hard limits, lockout, bootloop protection |
| [docs/SOC_GUIDE.md](docs/SOC_GUIDE.md) | Chipset detection and path map format |
| [docs/BOOTLOOP_RECOVERY.md](docs/BOOTLOOP_RECOVERY.md) | Stuck on logo? Disable module safely (incl. Samsung A05s) |

**Security issues:** do not open public issues. Use [GitHub Security Advisories](https://github.com/26August/ThermalGuard/security/advisories/new) or email `26August@gmail.com`.

## License

GNU General Public License v3.0. See [LICENSE](LICENSE).

This project is provided as-is for educational and personal use. The authors are not responsible for any damage to your device.
