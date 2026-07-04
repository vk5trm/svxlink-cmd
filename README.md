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

### Main Menu Overview

```
SvxLink (3h 30m) RemoteTRX (Down)  SvxReflector (Down) | Temp: 48°C  Up 3d 2h
        ┌──────────── Svx Admin v3.0.0  ────────────┐
        │                                           │
        │  Choose an Category:                      │
        │ ┌───────────────────────────────────────┐ │
        │ │  1   Service Control                  │ │       Service Menu to START,STOP,RESTART and ENABLE or DISABLE a service.
        │ │  2   DTMF Control                     │ │       DTMF Menu to Send DTMF commands to your node.
        │ │  3   Monitoring                       │ │       Monitoring Menu to view system health, service status and service Logs.
        │ │  4   Configuration                    │ │       Configuration Menu to edit config,environment and TCL event files.  
        │ │  5   Audio Tools                      │ │       Audio Tools Menu to Adjust audio levels and perform audio calibration.
        │ │  6   Voice Management                 │ │       Voice Menu Change to other voice files.
        │ │  7   Maintaince                       │ │       Maintaince Menu to check and enable log rotation and update this script
        | |  8   Reboot Computer                  | |       Reboot Computer.
        │ └───────────────────────────────────────┘ │
        │          <OK>          <Quit>             │
        └───────────────────────────────────────────┘
```
### Service Control Menu Overview
```
SvxLink (3h 30m) RemoteTRX (Down)  SvxReflector (Down) | Temp: 48°C  Up 3d 2h
        ┌──────────── Svx Admin v3.0.0  ────────────┐
        │                                           │
        │  Choose an Category:                      │
        │ ┌───────────────────────────────────────┐ │
        │ │  1   Start a Service                  │ │       Select a service to start - Choose one of the managed services
        │ │  2   Stop a Service                   │ │       Select a service to Stop - Choose one of the managed services 
        │ │  3   Restart a Service                │ │       Select a service to Restart - Choose one of the managed services 
        │ │  4   Enable a Sevice at Boot          │ │       Select a service to Enable at boot - Choose one of the managed services
        │ │  5   Disable a Sevice at Boot         │ │       Select a service to Disable at boot  - Choose one of the managed services
        │ │                                       │ │
        │ └───────────────────────────────────────┘ │
        │          <OK>          <Quit>             │
        └───────────────────────────────────────────┘
```
### DTMF Menu Overview
```
SvxLink (3h 30m) RemoteTRX (Down)  SvxReflector (Down) | Temp: 48°C  Up 3d 2h
        ┌──────────── Svx Admin v3.0.0  ────────────┐
        │                                           │
        │  Choose an Category:                      │
        │ ┌───────────────────────────────────────┐ │
        │ │  1   Send DTMF Command                │ │       Send a DTMF command to your node via keyboard 
        │ │                                       │ │       - Command format can be either a full command or a macro command 
        │ │                                       │ │       - Does not need the # at the end of the command 
        │ │                                       │ │      
        │ │                                       │ │      
        │ │                                       │ │
        │ └───────────────────────────────────────┘ │
        │          <OK>          <Quit>             │
        └───────────────────────────────────────────┘
```
### Monitoring Menu Overview
```
SvxLink (3h 30m) RemoteTRX (Down)  SvxReflector (Down) | Temp: 48°C  Up 3d 2h
        ┌──────────── Svx Admin v3.0.0  ────────────┐
        │                                           │
        │  Choose an Category:                      │
        │ ┌───────────────────────────────────────┐ │
        │ │  1   Show Detail Status               │ │       Shows detailed status of a service - Choose one of the managed services
        │ │  2   Follow Live Logs                 │ │       Follow live logs - Choose one of the managed services 
        │ │  3   System Health Check              │ │       Gives you disk space memory space and top 10 processes 
        │ │                                       │ │      
        │ │                                       │ │      
        │ │                                       │ │
        │ └───────────────────────────────────────┘ │
        │          <OK>          <Quit>             │
        └───────────────────────────────────────────┘
```
### Configuration Menu Overview
```
SvxLink (3h 30m) RemoteTRX (Down)  SvxReflector (Down) | Temp: 48°C  Up 3d 2h
        ┌──────────── Svx Admin v3.0.0  ────────────┐
        │                                           │
        │  Choose an Category:                      │
        │ ┌───────────────────────────────────────┐ │
        │ │  1   Edit config Files                │ │       Edit the main confguration files - See below
        │ │  2   Edit the Enviroment default      │ │       Edit the Enviroment default files - Choose one of the managed services
        │ │  3   Edit the main Event scripts      │ │       Edit the main Event Script files - See below
        │ │  4   Edit Module Event files          │ │       Edit the Module Event files - See below
        │ │                                       │ │      
        │ │                                       │ │
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

Use the "Edit main events scripts"  and module events scripts menu options for safe editing of Events TCL files. It first makes a copy of the original file but comments out all the lines exept the "namespace" and the "close of namespace }", Uncomment the lines you wish to change from the "proc" statement to the "]" before the next "proc" Refer to the [Events-Handling-System ](https://github.com/sm0svx/svxlink/wiki/Events-Handling-System)for detailed configuration options. 
  To undo an edited eventscript TCL file go to the relevent directory and delete the relevent TCL file. This will return you to the default config

  Edit these files with CARE - YOU HAVE BEEN WARNED
  It is very easy to make mistakes editing both these TCL files and have weird things happen and/or get lots of errors in the log.  

### Audio Tools Menu Overview
```
SvxLink (3h 30m) RemoteTRX (Down)  SvxReflector (Down) | Temp: 48°C  Up 3d 2h
        ┌──────────── Svx Admin v3.0.0  ────────────┐
        │                                           │
        │  Choose an Category:                      │
        │ ┌───────────────────────────────────────┐ │
        │ │  1   Alsa Mixer                       │ │       Start AlsaMixer to adjust audio levels - Auto saves on exit
        │ │  2   Audio Calibration tool           │ │       Start Audio Calibration tool - See below 
        │ │  3   Level Detector Calibration       │ │       Start Level Detector Calibration tool for adjusting CTCSS detection
        │ │                                       │ │        After the level detector calibration has finished put these values
        │ │                                       │ │        from the calibration into your RX section of your svxlink.conf file         
        │ │                                       │ │                   SIGLEV_SLOPE= [value]  
        │ └───────────────────────────────────────┘ │                   SIGLEV_OFFSET= [value]
        │          <OK>          <Quit>             │                   CTCSS_SNR_OFFSETS= [value]
        └───────────────────────────────────────────┘
```
### Audio Calibration tool

#### Receiver Calibration (-r flag):

1. Set RF signal generator to your frequency with:
   - 1000 Hz tone
   - 2400 Hz deviation
2. Adjust level using Alsamixer for coarse adjustment.
3. Use + or - in devcal for fine adjustment.
4. Adjust until the tone dev at bottom left reads 2400.
5. Record PREAMP= dB value into svxlink.conf RX section.

NOTE: -12 dBFS corresponds to 2404 Hz deviation.

#### Transmitter Calibration (-t flag):

1. Hit 'T' on keyboard to transmit.
2. Use a deviation meter OR Bessel 0 Null method.
3. Turn off all TX CTCSS tones.
4. Start with audio output in Alsamixer MUTED.
5. Test TX modulation:
   - 5a: Spectrum analyzer (zoomed on carrier amplitude) OR
   - 5b: SSB receiver in CW mode (tune to carrier).
6. Unmute Alsamixer output.
7. devcal applies a 1000 Hz tone.
8. Adjust modulation until carrier is minimized (Bessel null).
9. Use Alsamixer (coarse) then + or - in devcal (fine).
10. Record MASTER_GAIN= dB value into svxlink.conf TX section.

NOTE: SSB receivers should tune to carrier frequency - 1 kHz.

### Voice Management Menu Overview
```
SvxLink (3h 30m) RemoteTRX (Down)  SvxReflector (Down) | Temp: 48°C  Up 3d 2h
        ┌──────────── Svx Admin v3.0.0  ────────────┐
        │                                           │
        │  Choose an Category:                      │
        │ ┌───────────────────────────────────────┐ │
        │ │  1   Switch Voices                    │ │       The "Switch Voices Menu" lets you change voices on your system.
        │ │                                       │ │        It can handle either a normal directory with voice files,
        │ │                                       │ │        a single GIT repository with one set of voice files
        │ │                                       │ │        or a voice repository with multiple voices like my
        │ │                                       │ │        Australian voices repository. See link at bottom of page. 
        │ │                                       │ │
        │ └───────────────────────────────────────┘ │
        │          <OK>          <Quit>             │
        └───────────────────────────────────────────┘
```

### Maintaince Menu Overview
```
SvxLink (3h 30m) RemoteTRX (Down)  SvxReflector (Down) | Temp: 48°C  Up 3d 2h
        ┌──────────── Svx Admin v3.0.0  ────────────┐
        │                                           │
        │  Choose an Category:                      │
        │ ┌───────────────────────────────────────┐ │
        │ │  1   Check Log Rotation               │ │       Checks if Log rotation if not there sets it up on your system.
        │ │  2   Update SVXLink Cmd               │ │       Updates SVXLink Cmd from this GITHub repository.
        │ │                                       │ │      
        │ │                                       │ │      
        │ │                                       │ │     
        │ │                                       │ │
        │ └───────────────────────────────────────┘ │
        │          <OK>          <Quit>             │
        └───────────────────────────────────────────┘
```
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
- [VK5TRM Australian voices](https://github.com/vk5trm/en_AU-VK5TRM) — Voice repository.
