# SvxLink Admin (svx)

Interactive terminal tool for administering [SvxLink](https://github.com/sm0svx/svxlink) services on a Raspberry Pi. Simplifies management of repeater systems with a user-friendly menu interface.

This is a modified fork of the original [svxlink-cmd](https://github.com/audric/svxlink-cmd) by [Audric IW1GEU](https://github.com/audric).

## Table of Contents

- [Features](#features)
- [Services Managed](#services-managed)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Features

- **Service Control**: Start / Stop / Restart services
- **DTMF Control**: Send DTMF commands from the menu
- **Monitoring**: Detailed service status and live log following (journald or file)
- **Configuration**: Edit configuration, events scripts and environment files
- **Boot Management**: Enable / Disable services at boot and reboot system
- **Audio Tools**: 
  - Alsamixer integration - Auto saves your settings on exit. (auto-detects USB audio device)
  - Audio Calibration tool for adjusting audio level on Rx and TX
  - Signal detector Calibration Tool for adjusting CTCSS tone sensitivity
    
- **Change Voices**: Change voices in a Directory or GIT repository with single or multiple voices from the menu
- **Maintenance**: Log rotation check and auto-fix
- **System Info**: Live system display showing service status, CPU temperature, uptime, and last boot time

## Services Managed

- `svxlink.service` — Main repeater controller
- `remotetrx.service` — Remote transceiver
- `svxreflector.service` — Reflector/conference server

## Requirements

- **Hardware**: Raspberry Pi (or Debian-based system)
- **Software**:
  - SvxLink installed and configured
  - `dialog` (auto-detected; the script will offer to install if missing) - Whiptail is no longer surpported
  - `systemd` (for service management)

## Installation

```bash
sudo curl -sL https://raw.githubusercontent.com/vk5trm/svxlink-cmd/master/svx -o /usr/local/bin/svx && sudo chmod +x /usr/local/bin/svx
```

### Uninstall

```bash
sudo rm /usr/local/bin/svx
```

## Usage
Please note that sudo is no longer required

```bash
svx
```

This will launch the interactive admin menu. Navigate using arrow keys and select options with Enter.

### Menu Overview

```
○ SvxLink  ○ RemoteTRX  ○ SvxReflector | 48°C  Up 3d 2h  Since 2026-03-28 14:30
┌──────────── Svx Admin v2.8.0  ────────────┐
│                                           │
│  Choose an action:                        │
│ ┌───────────────────────────────────────┐ │
│ │  1  Start service                     │ │
│ │  2  Stop service                      │ │
│ │  3  Restart service                   │ │
│ │  ─  ─── DTMF Control────────          │ │ 
│ │  4  Send DTMF Command                 │ │
│ │  ─  ─── Monitoring ─────────          │ │
│ │  5  Show detailed status              │ │
│ │  6  Follow live logs                  │ │
│ │  7  System health check               │ │
│ │  ─  ─── Configuration ──────          │ │
│ │  8  Edit config file                  │ │
│ │  9  Edit environment defaults         │ │
│ │ 10  Edit main event scripts (.tcl)    │ │
| | 11  Edit modules event scripts (.tcl) | |
│ │  ─  ─── Audio ──────────────          │ │
│ │ 12  Alsamixer                         │ │
│ │ 13  Audio Calibration tool            │ │
│ │ 14  Signal Level Detector Calibration │ │
| |  ─  ─── Voices ─────────────          | │
| | 15  Switch Voices                     | |
│ │  ─  ─── Maintenance ────────          │ │
│ │ 16  Check log rotation                │ │
│ │  ─  ────── Boot  ───────────          │ │
│ │ 17  Enable service at boot            │ │
│ │ 18  Disable service at boot           │ │
| | 19  Reboot Computer                   | | 
│ └───────────────────────────────────────┘ │
│          <OK>          <Quit>             │
└───────────────────────────────────────────┘
```

## Configuration

SvxLink configuration files are typically located in:

- [**SVXLink config**:](https://www.svxlink.org/doc/man/man5/svxlink.conf.5.html) `/etc/svxlink/svxlink.conf`
- [**RemoteTRX config**:](http://svxlink.org/doc/man/man5/remotetrx.conf.5.html) `/etc/svxlink/remotetrx.conf`
- [**SVXReflector config**:]( http://svxlink.org/doc/man/man5/svxreflector.conf.5.html)`/etc/svxlink/svxreflector.conf`
- **Environment**: `/etc/default/`
- [**Main Event scripts**:](https://github.com/sm0svx/svxlink/wiki/Events-Handling-System) `/usr/share/svxlink/events.d/` and `/usr/share/svxlink/events.d/local/`
- **Modules config**: `/etc/svxlink/svxlink.d/`
                    - [Module_Help:](https://www.svxlink.org/doc/man/man5/ModuleHelp.conf.5.html)
                    - [Module_Echolink:](https://www.svxlink.org/doc/man/man5/ModuleEchoLink.conf.5.html)
                    - [Module_Parrot:](https://www.svxlink.org/doc/man/man5/ModuleParrot.conf.5.html)
                    - [Module_TCLVoiceMail:](https://www.svxlink.org/doc/man/man5/ModuleTclVoiceMail.conf.5.html)
                    - [Module_DTMFrepeater:]( https://www.svxlink.org/doc/man/man5/ModuleDtmfRepeater.conf.5.html)
                    - [Module_PropagationMonitor:](https://www.svxlink.org/doc/man/man5/ModulePropagationMonitor.conf.5.html)
                    - [Module_SelCallEnc:](https://www.svxlink.org/doc/man/man5/ModuleSelCallEnc.conf.5.html)
  
Use the "Edit config file" menu option for safe editing of configuration files. Click on the above config file for detailed configuration options.

Use the "Edit main events scripts" & "Edit modules event scripts" menu option for safe editing. Refer to the [Events-Handling-System ](https://github.com/sm0svx/svxlink/wiki/Events-Handling-System)for detailed configuration options. Edit these files with CARE. Remove any funchions you are not changing but leave the
```bash
namespace eval ${::logic_name} {
```
and the closing lines from the at the bottom of the file
```bash
# end of namespace
}
# This file has not been truncated
# 
```
***WARNING*** It is very easy to make mistakes and have weird things happen and/or get lots of errors in the log. To undo an edited eventscript TCL file go to the /usr/share/svxlink/events.d/local directory and delete the relevent TCL file. This will return you to the default config 

The "Switch Voices Menu" lets you change voices on your system. It can handle either a normal directory with voice files, A single GIT repository with one set of voice files or a voice repository with multiple voices like my [Australian voices](https://github.com/vk5trm/en_AU-VK5TRM) repository.

## Troubleshooting

### "dialog: command not found"
The script requires `dialog` for the menu interface. The script will offer to install it, or manually install with:
```bash
sudo apt-get install dialog
```

### Services not starting
Check the service status and logs:
```bash
sudo systemctl status svxlink.service
sudo journalctl -u svxlink.service -n 50
```

### Audio device not detected
Verify USB audio device is connected and recognized:
```bash
arecord -l
aplay -l
```

Then run audio calibration from the menu to detect and configure the device.

### Log files not rotating
Use menu option 16 (Check log rotation) to verify log rotation configuration and auto-fix issues.

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Test thoroughly
5. Submit a pull request

For bug reports or feature requests, please open an [issue](https://github.com/vk5trm/svxlink-cmd/issues).

## Authors

- [Audric IW1GEU](https://github.com/audric) — Original creator
- [Rob VK5TRM](https://github.com/vk5trm) — This Modified Version

## License

GPL-3.0 — See LICENSE file for details

---

**Related Projects:**
- [SvxLink](https://github.com/sm0svx/svxlink) — Main SvxLink project
- [Original svxlink-cmd](https://github.com/audric/svxlink-cmd) — Original repository
