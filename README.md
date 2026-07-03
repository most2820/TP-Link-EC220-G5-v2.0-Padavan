# Padavan-ng Firmware for TP-Link EC220-G5 v2.0

Custom Padavan-ng firmware build for TP-Link EC220-G5 v2.0 (MT7620).

## Hardware Specs

| Parameter | Value |
|-----------|-------|
| SoC | MediaTek MT7620A @ 600MHz |
| WiFi 5GHz | MT7612E (802.11ac) |
| WiFi 2.4GHz | RT3092 (802.11n) |
| Flash | 8MB |
| RAM | 64/128MB |

## Included Features

### VPN
- OpenVPN (client/server)
- WireGuard
- AmneziaWG
- StrongSwan (IPsec/IKEv2)

### DNS Security
- DNSCrypt proxy
- Stubby (DNS-over-TLS)
- DoH proxy (DNS-over-HTTPS)

### Network Tools
- Curl
- iPerf3 (bandwidth testing)
- nfqws (zapret DPI bypass)

### System
- Dropbear SSH (with fast crypto)
- ZRAM (compressed memory)
- LUA scripting
- EoIP tunnels
- Shortcut Forward Engine (hardware NAT offload)

## Installation

### Web Interface Upgrade
1. Download `.bin` firmware file from [Releases](../../releases)
2. Open router web interface (192.168.1.1)
3. Go to System Management > Firmware Upgrade
4. Upload the `.bin` file
5. Wait for reboot (~2 minutes)

### TFTP Recovery (if brick)
1. Download `tp_recovery.bin` and `tftpd64.464.zip`
2. Set PC IP to `192.168.0.66`
3. Connect PC to **LAN port 1**
4. Power on router while holding **Reset** button for 5+ seconds
5. Upload `tp_recovery.bin` via TFTP server

## Build Configuration

Key settings in `build.config`:

| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_FIRMWARE_CPU_600MHZ` | y | Force 600MHz CPU clock |
| `CONFIG_FIRMWARE_CPU_SLEEP` | y | Enable CPU sleep on idle |
| `CONFIG_FIRMWARE_WIFI5_DRIVER` | 3.0 | MT7612E v3.0.4.0 |
| `CONFIG_FIRMWARE_WIFI2_DRIVER` | 2.7 | RT3092 v2.7.1.5 |
| `CONFIG_FIRMWARE_INCLUDE_SHORTCUT_FE` | y | Hardware NAT offload |
| `CONFIG_CC_OPTIMIZE_FOR_SIZE` | y | Optimize for smaller image |

## License

Based on [Padavan-ng](https://gitlab.com/hadzhioglu/padavan-ng) by hadzhioglu.