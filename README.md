# LanRoster — downloadable builds

**This repository hosts released builds only. It contains no source code.** The
LanRoster source lives in a private repository and is not published here, and nothing in
this repository is imported, compiled or needed by the application.

LanRoster keeps an inventory of the devices on your local network: a table of everything
it can see on the LAN, what changed since the last scan, and a detail card for each device.

**Current version: 0.4.1.** LanRoster is a native Windows desktop app — a Qt window with
the scan history in SQLite — shipped as a single Windows executable with Python and Qt
inside, so there is no runtime to install.

- **Device table** — name, IP, MAC, vendor, type, open ports and last seen, one row per
  device.
- **Detail card** — LAN address (and the Tailscale address, when the Tailscale client can
  verify it), MAC, vendor, model, TTL, open ports, services and first/last seen, plus a
  presence chart and the device's change log (`new device`, `joined`, `left`,
  `ip changed`).
- **Activity feed** — the network-wide changes, shown when nothing is selected.
- **Tray icon** — left-click restores the window; right-click offers *Open LanRoster /
  Scan now / Quit*.
- **Background scanning** — the scan runs on a worker thread, so the window stays
  responsive and shows *Scanning…* until it finishes.

## Which file do I download?

Get the executable from the [latest release](../../releases/latest).

| Asset | Size | What it is |
|---|---|---|
| **`LanRoster.exe`** — start here | 58.6 MiB (61,470,785 bytes) | **The application.** Single 64-bit Windows executable with Python and Qt inside: no installer, no Python install. Double-click it, then press **Scan now** (`Ctrl+R`) |
| **`npcap-1.90.exe`** — optional | 1.3 MiB (1,325,256 bytes) | **The Npcap driver installer.** Run it once for raw ARP scans, which are the only way to read a device's MAC address straight from its reply and to get TTL hints. Without it LanRoster falls back to the Windows ARP cache |

Version 0.4.0 renamed the application: it was **LanScope Desktop** before 0.4.0, and the
executable has been `LanRoster.exe` since. When you replace a pre-0.4 build, keep a single
executable in the folder and repoint any shortcut, Send-To entry or script that still
names the old one.

Requires Windows 10/11 x64. Run it as Administrator for the most complete discovery.

`LanRoster.exe` is unsigned (there is no code-signing certificate), so SmartScreen may
show **"Windows protected your PC"** on first run: choose **More info → Run anyway**.
A matching SHA-256 proves the file left GitHub intact; it does not prove who built it
and does not suppress that prompt.

## Install

1. Download `LanRoster.exe` — single file, no Python needed.
2. Run `npcap-1.90.exe` once if you want raw ARP scans (MAC addresses and TTL hints).
   Without it the app falls back to the ARP cache and the log says
   `scapy ARP sweep unavailable`.
3. Run the exe from any folder; it creates `data-<machine>\` and `lanroster-<machine>.log`
   beside itself.

## Checksums

Every asset is byte-identical to the file that was verified before release. Confirm
after downloading:

```powershell
Get-FileHash .\LanRoster.exe, .\npcap-1.90.exe -Algorithm SHA256
```

```cmd
certutil -hashfile LanRoster.exe SHA256
certutil -hashfile npcap-1.90.exe SHA256
```

| Asset | Bytes | SHA-256 |
|---|---|---|
| `LanRoster.exe` (v0.4.1) | 61,470,785 | `157a229989217cb3cf1ed3ac190ea17402bc68f9a9cc35a9c6dee8b8dda1db26` |
| `npcap-1.90.exe` | 1,325,256 | `682b57e1c05ef375931975c9d6ded40888f4bfdf9f4b11571e654f962f1199b6` |

`npcap-1.90.exe` is Npcap's own installer, republished here unmodified so the download is
easy to find; its checksum is Npcap's, not a build of ours.

The same checksums are repeated in the notes for each release, so a download from any
version can be verified against its own release page.

## Where your data lives

Everything LanRoster learns stays on the machine that ran the scan. Nothing is uploaded.

| What | Where |
|---|---|
| Database — devices, change events | `data-<machine>\lanroster.db`, beside the exe |
| Vendor cache (OUI table) | `data-<machine>\oui.csv`, refreshed in place |
| Log | `lanroster-<machine>.log`, beside the exe |

`<machine>` is the computer name, so several copies of the app in one folder keep separate
histories instead of sharing one database. To start over, quit the app and delete
`data-<machine>\`. There is no account, no server component and no cloud store.

## Privacy

LanRoster sends nothing to the developer and has no sign-in, sync or cloud storage. What
it does send on your behalf, and exactly what it keeps in the data folder, are set out in
the [privacy statement](PRIVACY.md).

## What's new

**0.4.1** is the Microsoft Store packaging release. The portable executable is unchanged:
the same window, the same scanning, and the same `data-<machine>\` folder beside it. A
Store build keeps its database, OUI cache and log in `%LOCALAPPDATA%\LanRoster` instead,
because the packaged install folder is read-only; the [privacy statement](PRIVACY.md)
covers both locations. The [release notes](../../releases/latest) have the full list.

**0.4.0** renamed the application from LanScope Desktop to LanRoster: `LanRoster.exe`, the
`LANROSTER_*` environment variables, the `lanroster.db` database and the
`lanroster-<machine>.log` log. An existing pre-0.4 database is adopted on first launch, so
scan history carries over with no import step. The [release
notes](../../releases/latest) have the full list.

## Issues and support

This repository has no issue tracker for the application source. Report problems to the
author, [Dr. Haiping Xu](https://www.cis.umassd.edu/~hxu/), or through whoever supplied
your copy of the build.

Scan only networks you are responsible for.
