# Parahub Mesh

Custom OpenWrt firmware for self-organizing mesh networks using **batman-adv** (L2) and **Yggdrasil** (overlay). Nodes auto-configure on first boot with zero manual setup.

## Architecture

```
                    Internet
                       |
              [Bumblebee nodes]         ← L3 gateways (WireGuard VPN, guest isolation, DoH)
                 /    |    \
            batman-adv mesh (802.11s)   ← L2 transport
               /      |      \
           [Bee nodes]                  ← L2 relays (minimal, low-power)
```

Two node roles:

| Role | Function | Packages | Devices |
|------|----------|----------|---------|
| **Bumblebee** | L3 Gateway — full stack | batman-adv, Yggdrasil, WireGuard, tc, DoH | AXT1800, MT3000, MT6000, AX53U |
| **Bee** | L2 Transport — mesh relay | batman-adv, Yggdrasil | AR300M16, CPE710 |

### Network layout per node

- **Private WiFi** (`Parahub`, 5GHz) — WPA3/SAE, 802.11r/k/v seamless roaming across all nodes
- **Public WiFi** (`parahub.io/free`, 2.4GHz) — OWE encrypted, guest-isolated on Bumblebees
- **Mesh backhaul** (802.11s on both bands) — SAE-encrypted, batman-adv BATMAN_V routing
- **Yggdrasil overlay** — IPv6 management plane, OTA updates, Parahub services at full speed

### Guest internet path (Bumblebee)

```
Guest device → parahub.io/free → guest zone → policy routing (table 100) → WireGuard VPN → exit
```

VPN tunnel is auto-configured via heartbeat API. DNS is forwarded through VPN (no leak).

## Supported devices

| Device | Target | Role | Notes |
|--------|--------|------|-------|
| GL.iNet GL-AXT1800 (Slate AX) | qualcommax/ipq60xx | Bumblebee | WiFi 6, 512MB RAM |
| GL.iNet GL-MT3000 (Beryl AX) | mediatek/filogic | Bumblebee | WiFi 6, compact |
| GL.iNet GL-MT6000 (Flint 2) | mediatek/filogic | Bumblebee | WiFi 6, 1GB RAM |
| Asus RT-AX53U | ramips/mt7621 | Bumblebee | WiFi 6, DSA switch |
| GL.iNet GL-AR300M16 (16MB) | ath79/generic | Bee | Tiny, 2.4GHz only |
| TP-Link CPE710 v1 | ath79/generic | Bee | 5GHz outdoor, 23dBi directional |

## Building

### Prerequisites

- Linux x86_64
- `wget`, `zstd`, `make`, `python3`
- `MESH_HEARTBEAT_KEY` in environment or in `/opt/parahub/.env`

### Build firmware

```bash
./scripts/build.sh <device>
```

The script automatically downloads the OpenWrt Image Builder on first run (~1.5GB per target).

**Examples:**

```bash
./scripts/build.sh axt1800      # GL-AXT1800 Bumblebee
./scripts/build.sh cpe710       # CPE710 Bee
./scripts/build.sh mt6000       # GL-MT6000 Bumblebee

# Custom OpenWrt version
OPENWRT_VERSION=25.12.0 ./scripts/build.sh mt3000

# Extra packages
PACKAGES_EXTRA="nano htop" ./scripts/build.sh axt1800
```

Output firmware lands in `output/`. A `manifest.json` is maintained for OTA updates.

### Flash

Standard OpenWrt sysupgrade:

```bash
sysupgrade -v /tmp/openwrt-*-sysupgrade.bin
```

Or via LuCI web UI: System > Backup/Flash Firmware.

## How it works

### Zero-touch first boot

The `99-parahub-mesh` uci-defaults script runs once on first boot and configures everything:

1. **Identity** — derives hostname and unique subnets from hardware MAC (`Parahub-XXXX`)
2. **Network** — batman-adv mesh, private bridge, guest isolation (Bumblebee), WireGuard stub
3. **WiFi** — dual-band mesh backhaul, private AP (802.11r roaming), public AP
4. **Firewall** — zone-based (lan/guest/wan/vps_gateway/yggdrasil), guest kill-switch
5. **Services** — heartbeat (5min), gateway health check (2min), OTA updates (nightly)
6. **Yggdrasil** — generates node keys, connects to VPS peers (Bumblebee) or multicast (Bee)

### OTA updates

Nodes check `manifest.json` nightly, compare SHA256, and auto-sysupgrade. Node identity (MAC, subnets, keys) is preserved across updates via `sysupgrade.conf`.

### Heartbeat

Every 5 minutes, nodes phone home to the Parahub API with status (uptime, clients, batman neighbors, Yggdrasil address). The API responds with VPN configuration, paid client lists, and firmware update info.

### Gateway election

`parahub-gw-check` monitors WireGuard tunnel health. When the tunnel is active and healthy, it promotes the node to `gw_mode=server` in batman-adv, advertising itself as an internet gateway to Bee nodes.

## Scripts

| Script | Description |
|--------|-------------|
| `parahub-heartbeat` | Status reporting + VPN auto-config |
| `parahub-autoupdate` | OTA firmware updates with SHA256 verification |
| `parahub-gw-check` | Gateway health monitoring + batman-adv mode promotion |
| `parahub-speed-control` | Per-client bandwidth shaping on guest network |
| `parahub-vps-setup` | WireGuard tunnel configuration (called by heartbeat) |
| `parahub-mullvad` | Optional Mullvad VPN for lower-latency guest exit |

## License

[MIT](LICENSE)
