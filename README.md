# SvxLink Admin (svx)

Interactive terminal tool for administering [SvxLink](https://github.com/sm0svx/svxlink) services on a Raspberry Pi. Simplifies management of repeater systems with a user-friendly menu interface.

This is a maintained fork of the original [svxlink-cmd](https://github.com/audric/svxlink-cmd) by [Audric IW1GEU](https://github.com/audric).

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
- **Monitoring**: Detailed service status and live log following (journald or file)
- **Configuration**: Edit configuration, GPIO, events scripts and environment files
- **Boot Management**: Enable / Disable services at boot
- **GPIO Setup**: GPIO pin setup
- **Audio Tools**: 
  - Alsamixer integration (auto-detects USB audio device)
  - Audio Calibration tool for adjusting audio level on Rx and TX
  - Signal detector Calibration Tool for adjusting CTCSS tone sensitivity
- **Maintenance**: Log rotation check and auto-fix
- **System Info**: Live system display showing service status, CPU temperature, uptime, and last boot time

## Services Managed

- `svxlink.service` — Main repeater controller
- `remotetrx.service` — Remote transceiver
- `svxreflector.service` — Reflector/conference server
- `svxlink_gpio_setup.service` — GPIO pin setup

## Requirements

- **Hardware**: Raspberry Pi (or Debian-based system)
- **Software**:
  - SvxLink installed and configured
  - `dialog` or `whiptail` (auto-detected; the script will offer to install if missing)
  - `sudo` privileges (script must be run as root)
  - `systemd` (for service management)
  - `journalctl` (for log viewing)

## Installation

```bash
sudo curl -sL https://raw.githubusercontent.com/vk5trm/svxlink-cmd/master/svx -o /usr/local/bin/svx && sudo chmod +x /usr/local/bin/svx
```

### Uninstall

```bash
sudo rm /usr/local/bin/svx
```

## Usage

```bash
sudo svx
```

This will launch the interactive admin menu. Navigate using arrow keys and select options with Enter.

### Menu Overview

```
○ SvxLink  ○ RemoteTRX  ○ SvxReflector | 48°C  Up 3d 2h  Since 2026-03-28 14:30
┌──────────── Svx Admin v1.0.0 ────────────┐
│                                           │
│  Choose an action:                        │
│ ┌───────────────────────────────────────┐ │
│ │  1  Start service                     │ │
│ │  2  Stop service                      │ │
│ │  3  Restart service                   │ │
│ │  ─  ─── Monitoring ─────────          │ │
│ │  4  Show detailed status              │ │
│ │  5  Follow live logs                  │ │
│ │  6  System health check               │ │
│ │  ─  ─── Configuration ──────          │ │
│ │  7  Edit config file                  │ │
│ │  8  Edit GPIO config                  │ │
│ │  9  Edit environment defaults         │ │
│ │ 10  Edit event scripts (.tcl)         │ │
│ │  ─  ─── Audio ──────────────          │ │
│ │ 11  Alsamixer                         │ │
│ │ 12  Audio Calibration tool            │ │
│ │ 13  Signal Level Detector Calibration │ │
│ │  ─  ─── Maintenance ────────          │ │
│ │ 14  Check log rotation                │ │
│ │  ─  ────── Boot  ───────────          │ │
│ │ 15  Enable service at boot            │ │
│ │ 16  Disable service at boot           │ │
│ └───────────────────────────────────────┘ │
│          <OK>          <Quit>             │
└───────────────────────────────────────────┘
```

## Configuration

SvxLink configuration files are typically located in:

- **Main config**: `/etc/svxlink/svxlink.conf`
- **GPIO config**: `/etc/svxlink/gpio.conf`
- **Environment**: `/etc/default/svxlink`
- **Event scripts**: `/usr/share/svxlink/events.d/` or `/etc/svxlink/events.d/`

Use the "Edit config file" menu option for safe editing. Refer to the [SvxLink.conf documentation](https://www.svxlink.org/doc/man/man5/svxlink.conf.5.html) for detailed configuration options.

Use the "Edit events scripts" menu option for safe editing. Refer to the [Events-Handling-System ](https://github.com/sm0svx/svxlink/wiki/Events-Handling-System)for detailed configuration options. Edit these files with CARE. It is very easy to make mistakes and have weird things happen and/or get lots of errors in the log. To undo an edited eventscript TCL file go to the /usr/share/svxlink/events.d/local directory and delete the relevent TCL file. This will return you to the default config 

## Troubleshooting

### "dialog: command not found"
The script requires `dialog` or `whiptail` for the menu interface. The script will offer to install it, or manually install with:
```bash
sudo apt-get install dialog
```

### Permission denied when running `svx`
The script must be run with `sudo`:
```bash
sudo svx
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
Use menu option 14 (Check log rotation) to verify log rotation configuration and auto-fix issues.

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
- [Rob VK5TRM](https://github.com/vk5trm) — Current maintainer

## License

GPL-3.0 — See LICENSE file for details

---

**Related Projects:**
- [SvxLink](https://github.com/sm0svx/svxlink) — Main SvxLink project
- [Original svxlink-cmd](https://github.com/audric/svxlink-cmd) — Original repository
