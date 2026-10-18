# VBS Resolume Arena Redundant

Bitfocus Companion module for controlling **Resolume Arena in MAIN + BACKUP redundancy**, with live state feedback, dynamic presets, diagnostics, Video Router control, monitor snapshots and Stream Deck+ rotary support.

**Version 1.0.0 — Initial public release**  
© 2026 Video Bridge Solutions — Pierre-Yves Halbig

<img width="1047" height="1103" alt="Greenshot 2026-10-18 10 19 01" src="https://github.com/user-attachments/assets/55160e9a-ebd4-478a-bd9d-892c4f90822f" />

## What it does

VBS Resolume Arena Redundant lets one Companion connection control one or two Resolume Arena machines.

- **MAIN** is the reference machine for composition structure, names, presets and thumbnails.
- **BACKUP** receives the same logical live commands in parallel and is compared continuously with MAIN.
- **REST** is used for composition data, discovery, thumbnails and supported API commands.
- **WebSocket** is used for realtime state, dynamic names and feedback updates.
- **OSC** is used where Resolume transport/routing control is better suited to OSC.

BACKUP is optional. With BACKUP disabled, the module works as a full single-machine Resolume controller.

## Highlights

- Parallel MAIN/BACKUP control designed for live use.
- Automatic REST/WebSocket recovery after Resolume or network restarts.
- Dynamic Column, Layer, Group, Deck and Clip names.
- Automatic refresh after composition renames and structural edits.
- Low-latency feedback engine with on-demand realtime subscriptions for large compositions.
- Dynamic preset generation with optional Clip thumbnails.
- Video Router discovery and dynamic Input control.
- Layer Group control and logical MAIN/BACKUP remapping.
- Selected Object workflow following the object selected in Resolume.
- Optional low-frame-rate Monitor / Screen previews.
- Stream Deck+ rotary presets for composition, layers, clips, groups, Video Routers and effects.
- Redundancy comparison, parameter synchronization and optional structure synchronization.
- Connection diagnostics for MAIN and BACKUP: Resolume version, REST/WebSocket state, API latency, uptime, reconnects, disconnects and last error.

## Requirements

- Bitfocus Companion with Node 22 module runtime support.
- Resolume Arena/Avenue with the Webserver enabled.
- REST/WebSocket access, normally on port **8080**.
- OSC Input enabled when using transport/routing functions, normally on port **7000**.
- Network/firewall rules allowing Companion to reach the Resolume machines.

## Install the module in Companion

Download the latest `.tgz` release of **VBS Resolume Arena Redundant** from the GitHub Releases page.

In Bitfocus Companion:

1. Open **Modules**.
2. Click **Import module package**.
3. Select the downloaded `.tgz` file.
4. Wait for the import to complete.
5. Click **Refresh modules list** if the new module does not appear immediately.

The module should now be available as:

**VBS Resolume Arena Redundant**

### Select the module version

If several versions of the module are installed on the same Companion system:

1. Open **Connections**.
2. Create a new connection or edit an existing VBS Resolume Arena Redundant connection.
3. Select **VBS Resolume Arena Redundant** as the module.
4. Select **Version 1.0.0** from the available module versions.
5. Save the connection.

This is particularly important on development systems that previously used test versions such as `1.5.x`. Companion may keep older development builds installed alongside the public `1.0.0` release.

For a new installation, simply select the latest available stable version.

## Quick start

1. In Resolume, enable **Preferences > Webserver**.
2. In Resolume, enable **Preferences > OSC > OSC Input** if OSC functions are required.
3. In Companion, create a connection using **VBS Resolume Arena Redundant**.
4. Select **Version 1.0.0**.
5. Enter the MAIN Resolume IP address or hostname.
6. Check the REST/WebSocket port, normally **8080**.
7. Check the OSC port, normally **7000**.
8. Enable BACKUP only when a second Resolume machine is used.
9. Enter the BACKUP IP address or hostname.
10. Save the connection.

Once connected, Companion automatically retrieves the Resolume composition structure, names and available controls.

Keep the same show structure and media layout on MAIN and BACKUP whenever possible for the most reliable redundancy mapping.

## Reliability model

MAIN and BACKUP use independent REST state, WebSockets, timers and reconnection logic. A failing BACKUP must not stall MAIN.

The module uses bounded REST requests, guarded WebSocket reconnects, exponential retry, connection watchdogs and automatic state resynchronization. Live triggers are not queued for delayed replay when a machine is offline.

When Resolume is edited after connection, structural changes trigger a debounced REST resync. Dynamic names, choices, presets and feedback definitions are rebuilt only when needed.

## Redundancy mapping

For Columns, Layers and Clips, BACKUP lookup follows this order:

1. matching Resolume ID when available;
2. matching name;
3. matching position.

Layer Groups, Effects and Video Routers use the same principle with object-specific fallbacks.

The module reports states such as:

- `OK`
- `MAIN ONLY`
- `DEGRADED`
- `STRUCTURE MISMATCH`
- `STATE MISMATCH`

## Performance on large compositions

Production-safe realtime behaviour is automatic. High-cardinality Clip and Group×Column WebSocket paths are subscribed only when they are actually needed by placed feedbacks.

Recommended defaults:

- Monitor snapshots: **OFF**
- Clip preset generation: **Standard**
- Clip preset thumbnails: **OFF**
- Effect preset generation: **Bypass only**
- Detailed per-Clip variables: **OFF**

When Monitor snapshots are enabled, the module uses JPEG snapshots at a fixed 1000 ms interval.

## Diagnostics

MAIN and BACKUP expose Companion variables for:

- Resolume product and version;
- REST/API connection state;
- WebSocket connection state;
- REST/API response latency;
- WebSocket uptime;
- reconnect and disconnect counters;
- last disconnect code/reason/time;
- last connection error.

Resolume render FPS, GPU, VRAM, CPU and RAM are **not exposed by the public Resolume REST/WebSocket API** and are therefore not fabricated by this module.

## Documentation

The complete bilingual documentation is available in [`companion/HELP.md`](companion/HELP.md).

## License

MIT License. See [`LICENSE`](LICENSE).

## Issues

Repository: https://github.com/VideoBridgeSolutions/companion-module-resolume-redundant  
Issues: https://github.com/VideoBridgeSolutions/companion-module-resolume-redundant/issues
