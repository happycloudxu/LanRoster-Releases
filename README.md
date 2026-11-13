# LanRoster — downloadable builds

**This repository hosts released builds only. It contains no source code.** The
LanRoster source lives in a private repository and is not published here, and nothing in
this repository is imported, compiled or needed by the application.

LanRoster keeps an inventory of the devices on your local network: a table of everything
it can see on the LAN, what changed since the last scan, and a detail card for each device.

**Current version: 0.4.3.** LanRoster is a native Windows desktop app — a Qt window with
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
| **`LanRoster.exe`** — start here | 58.6 MiB (61,478,072 bytes) | **The application.** Single 64-bit Windows executable with Python and Qt inside: no installer, no Python install. Double-click it, then press **Scan now** (`Ctrl+R`) |
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
3. Run the exe from any folder. It keeps `data-<machine>\` and `lanroster-<machine>.log` in
   `%LOCALAPPDATA%\LanRoster`, not beside the exe — see [Where your data
   lives](#where-your-data-lives) to keep them with the exe instead.

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
| `LanRoster.exe` (v0.4.3) | 61,478,072 | `4d4ca18e18fd6039c63671fa4b46a0475362b1c3513905a0d79170913d08a08c` |
| `npcap-1.90.exe` | 1,325,256 | `682b57e1c05ef375931975c9d6ded40888f4bfdf9f4b11571e654f962f1199b6` |

`npcap-1.90.exe` is Npcap's own installer, republished here unmodified so the download is
easy to find; its checksum is Npcap's, not a build of ours.

The same checksums are repeated in the notes for each release, so a download from any
version can be verified against its own release page.

## Where your data lives

Everything LanRoster learns stays on the machine that ran the scan. Nothing is uploaded.

| What | Where |
|---|---|
| Database — devices, change events | `%LOCALAPPDATA%\LanRoster\data-<machine>\lanroster.db` |
| Vendor cache (OUI table) | `%LOCALAPPDATA%\LanRoster\data-<machine>\oui.csv`, refreshed in place |
| Log | `%LOCALAPPDATA%\LanRoster\lanroster-<machine>.log` |

`%LOCALAPPDATA%` is usually `C:\Users\<you>\AppData\Local`. `<machine>` is the computer
name, so several copies of the app on one PC keep separate histories instead of sharing one
database. The folder that holds the executable is never written to — that is what lets the
same code run from a Microsoft Store install, where the program folder is read-only. To
start over, quit the app and delete `%LOCALAPPDATA%\LanRoster`. There is no account, no
server component and no cloud store.

To keep the data with a portable copy of the app instead of in your user profile, use any
one of, in this order of precedence: a `--data-dir <path>` argument, the
`LANROSTER_DATA_DIR` environment variable, or an empty `portable.txt` file next to
`LanRoster.exe`. On first run, 0.4.2 copies an existing `data-<machine>\` database from
beside the executable into the new location once, so upgrade history carries over.

## Privacy

LanRoster sends nothing to the developer and has no sign-in, sync or cloud storage. What
it does send on your behalf, and exactly what it keeps in the data folder, are set out in
the [privacy statement](PRIVACY.md).

## What's new

**0.4.3** rebuilds Help as a full guide with a table of contents and internal links, and adds
an **Npcap and discovery quality** section: what the driver is, what degrades without it,
numbered 1.90 install steps, licensing, and troubleshooting with `sc query npcap`. The amber
discovery-capability chip now opens a scrollable dialog instead of a cramped message box, so
the detected state and its fix can be read in full. Scanning behaviour is unchanged. The
[release notes](../../releases/latest) have the full list.

**0.4.2** stores writable state in `%LOCALAPPDATA%\LanRoster` for every install, portable or
Store, with no packaging detection at all — this fixes the packaged 0.4.0 build crashing
while trying to write inside its read-only install folder. State no longer sits beside the
executable: keep it there with an empty `portable.txt` next to `LanRoster.exe`, a
`--data-dir <path>` argument or the `LANROSTER_DATA_DIR` environment variable. A one-time
migration copies an existing adjacent `data-<machine>\` database into the new location, so
history survives the upgrade. Help/About now names the actual database file rather than its
folder, and a saved window position that would land off-screen is pulled back onto a real
monitor. The [release notes](../../releases/latest) have the full list.

**0.4.1** was the Microsoft Store packaging release. The packaged build keeps its database,
OUI cache and log in `%LOCALAPPDATA%\LanRoster` because the install folder is read-only;
0.4.2 gives the portable build that same location. The [release
notes](../../releases/latest) have the full list.

**0.4.0** renamed the application from LanScope Desktop to LanRoster: `LanRoster.exe`, the
`LANROSTER_*` environment variables, the `lanroster.db` database and the
`lanroster-<machine>.log` log. An existing pre-0.4 database is adopted on first launch, so
scan history carries over with no import step. The [release
notes](../../releases/latest) have the full list.

## Issues and support

This repository has no issue tracker for the application source. Report problems to the
author, Dr. Haiping Xu, or through whoever supplied your copy of the build.

Scan only networks you are responsible for.
