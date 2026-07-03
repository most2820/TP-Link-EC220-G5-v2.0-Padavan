# Padavan-ng Firmware for TP-Link EC220-G5 v2.0

Custom Padavan-ng firmware build for TP-Link EC220-G5 v2.0 (MT7620).

## Hardware Specs

| Parameter | Value |
|-----------|-------|
| SoC | MediaTek MT7620A @ 600MHz |
| CPU | 600MHz base, downclocks to 200MHz on idle (sleep mode enabled) |
| WiFi 5GHz | MT7612E v3.0.4.0 (802.11ac) |
| WiFi 2.4GHz | RT3092 v2.7.1.5 (802.11n) |
| Flash | 8MB |
| RAM | 64/128MB |
| Hardware | Shortcut Forward Engine (hardware NAT offload) |
| Build | Size-optimized firmware image (~0.5MB savings) |

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

### CPU & Hardware
| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_FIRMWARE_CPU_600MHZ` | y | Force 600MHz CPU clock |
| `CONFIG_FIRMWARE_CPU_SLEEP` | y | Enable CPU sleep on idle (downclock to 200MHz) |
| `CONFIG_FIRMWARE_WIFI5_DRIVER` | 3.0 | MT7612E v3.0.4.0 |
| `CONFIG_FIRMWARE_WIFI2_DRIVER` | 2.7 | RT3092 v2.7.1.5 |
| `CONFIG_FIRMWARE_INCLUDE_SHORTCUT_FE` | y | Hardware NAT offload |
| `CONFIG_CC_OPTIMIZE_FOR_SIZE` | y | Optimize for smaller image (~0.5MB savings) |

### Network & Security
| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_FIRMWARE_ENABLE_IPV6` | y | IPv6 support |
| `CONFIG_FIRMWARE_INCLUDE_XFRM` | y | XFRM (IPsec) modules & iptables extension (~0.2MB) |
| `CONFIG_FIRMWARE_INCLUDE_IPSET` | y | IPSet utility and kernel modules (~0.4MB) |

### Web & Authentication
| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_FIRMWARE_INCLUDE_LANG_RU` | y | Russian language support |
| `CONFIG_FIRMWARE_INCLUDE_EAP_PEAP` | y | EAP-TTLS and EAP-PEAP authentication (OpenSSL ~1.2MB) |
| `CONFIG_FIRMWARE_INCLUDE_DDNS_SSL` | y | HTTPS support for DDNS client (OpenSSL ~1.2MB) |
| `CONFIG_FIRMWARE_INCLUDE_HTTPS` | y | HTTPS support (OpenSSL ~1.2MB) |
| `CONFIG_FIRMWARE_INCLUDE_SFTP` | y | SFTP server (OpenSSL ~1.2MB, sftp-server ~0.06MB) |

### VPN
| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_FIRMWARE_INCLUDE_DROPBEAR` | y | Dropbear SSH (~0.3MB) |
| `CONFIG_FIRMWARE_INCLUDE_DROPBEAR_FAST_CODE` | y | Fast cipher/hashing (~0.06MB) |
| `CONFIG_FIRMWARE_INCLUDE_OPENVPN` | y | OpenVPN (requires IPv6, OpenSSL ~1.2MB, openvpn ~0.4MB) |
| `CONFIG_FIRMWARE_INCLUDE_SSWAN` | y | StrongSwan (XFRM ~0.2MB, strongswan ~0.7MB) |
| `CONFIG_FIRMWARE_INCLUDE_WIREGUARD` | y | WireGuard VPN module and utilities |
| `CONFIG_FIRMWARE_INCLUDE_OPENSSL_EC` | y | Elliptic Curves to OpenSSL library (~0.1MB) |
| `CONFIG_FIRMWARE_INCLUDE_OPENSSL_EXE` | y | OpenSSL executable (~0.4MB) |

### Media & Services
| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_FIRMWARE_INCLUDE_XUPNPD` | y | xUPNPd IPTV mediaserver (~0.3MB) |
| `CONFIG_FIRMWARE_INCLUDE_DNSCRYPT` | y | DNSCrypt proxy (~0.5MB) |
| `CONFIG_FIRMWARE_INCLUDE_STUBBY` | y | Stubby DNS-over-TLS (DoT) (~0.5MB) |
| `CONFIG_FIRMWARE_INCLUDE_DOH` | y | doh_proxy DNS-over-HTTPS (DoH) (~0.4MB) |
| `CONFIG_FIRMWARE_INCLUDE_WPAD` | y | WPAD support |

### System & Utilities
| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_FIRMWARE_INCLUDE_ZRAM` | y | Compressed memory support |
| `CONFIG_FIRMWARE_INCLUDE_LUA` | y | Lua scripting (~0.2MB) |
| `CONFIG_FIRMWARE_INCLUDE_EOIP` | y | EoIP Ethernet Tunnels over IP (~0.01MB) |
| `CONFIG_FIRMWARE_INCLUDE_CURL` | y | Curl support (~0.15MB) |
| `CONFIG_FIRMWARE_INCLUDE_IPERF3` | y | iPerf3 support (~0.13MB) |
| `CONFIG_FIRMWARE_INCLUDE_NFQWS` | y | nfqws (zapret DPI bypass, ~0.1MB) |

### Kernel Overrides
| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_NETFILTER_NETLINK_QUEUE` | m | Netlink queue support |
| `CONFIG_NETFILTER_XT_TARGET_NFQUEUE` | m | NFQUEUE iptables target |
| `CONFIG_IP_NF_QUEUE` | m | IP queue support |
| `CONFIG_IP6_NF_FILTER` | y | IPv6 netfilter |
| `CONFIG_IP6_NF_TARGET_REJECT` | y | IPv6 reject target |
| `CONFIG_IP6_NF_MANGLE` | m | IPv6 mangling |
| `CONFIG_IP6_NF_QUEUE` | m | IPv6 queue |
| `CONFIG_BRIDGE_NF_EBTABLES` | m | Bridge ebtables support |

### Additional Features
| Option | Value | Description |
|--------|-------|-------------|
| `CONFIG_FIRMWARE_INCLUDE_AMNEZIAWG` | y | AmneziaWG kernel module |

### Excluded by Default
| Option | Reason |
|--------|--------|
| `CONFIG_FIRMWARE_INCLUDE_ADB` | Debugging tool (Android Debug Bridge) |
| `CONFIG_FIRMWARE_INCLUDE_OPENSSH` | Alternative to Dropbear |
| `CONFIG_FIRMWARE_INCLUDE_RPL2TP` | Legacy L2TP protocol |
| `CONFIG_FIRMWARE_INCLUDE_VLMCSD` | KMS Activation (virtual license manager) |

- `CONFIG_FIRMWARE_INCLUDE_DROPBEAR_FAST_CODE` enables asymmetrical ciphers and hashes for speed (~0.06MB)
- `CONFIG_FIRMWARE_INCLUDE_AMNEZIAWG` provides modern WireGuard implementation with additional security features

## License

Based on [Padavan-ng](https://gitlab.com/hadzhioglu/padavan-ng) by hadzhioglu.