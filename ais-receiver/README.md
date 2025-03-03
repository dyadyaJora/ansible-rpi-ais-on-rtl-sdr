# AIS Receiver Ansible Role

An Ansible role for setting up a complete AIS (Automatic Identification System) receiver system using RTL-SDR hardware. This role installs and configures the necessary software to receive AIS signals from ships and vessels, process them, and optionally forward them to various AIS aggregation services.

## Requirements

- Raspberry Pi or compatible Linux system (ARM-based)
- RTL-SDR USB dongle
- Antenna suitable for marine VHF frequencies
- SSH access to the target machine
- The target system must be able to install armhf packages

## Role Variables

### RTL-AIS Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `rtl_ais_version` | Version of rtl-ais package to install | `0.4.3-stable` |
| `rtl_ais_device_index` | RTL-SDR device index to use | `0` |
| `rtl_ais_extra_opts` | Extra options for rtl_ais | `""` |
| `rtl_ais_host` | Host IP to bind rtl_ais | `127.0.0.1` |
| `rtl_ais_port` | Port for rtl_ais to listen on | `10110` |
| `rtl_ais_user` | User to run rtl_ais service as | `root` |
| `rtl_ais_group` | Group to run rtl_ais service as | `root` |

### AIS Control & Dispatcher Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `aiscontrol_port` | Port for AIS Control web interface | `8080` |
| `aiscontrol_ws_port` | WebSocket port for AIS Control | `8081` |
| `aisdispatcher_name` | Name of the AIS station | `rPiAIS001` |
| `aisdispatcher_receivers` | List of receivers to forward AIS data to | See example below |

Example for `aisdispatcher_receivers`:

```yaml
aisdispatcher_receivers:
  - name: "Local OpenCPN"
    host: "127.0.0.1"
    port: "10110"
  - name: "AISHub"
    host: "data.aishub.net"
    port: "2222"
  - name: "Marine Traffic"
    host: "5.5.5.5"
    port: "5555"
```

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Install AIS Receiver
  hosts: rpi_devices
  become: true
  vars:
    aisdispatcher_name: "rPiAIS001"
    aisdispatcher_receivers:
      - name: "AISHub"
        host: "data.aishub.net"
        port: "2222"
  roles:
    - ais-receiver
```

## Components

This role installs and configures:

* RTL-SDR driver and libraries
* RTL-AIS software for decoding AIS signals
* AIS Control web interface
* AIS Dispatcher for sharing data with multiple services

## License

GPL-2.0-or-later

## Author Information

Created by dyadyaJora (jora.dev)
