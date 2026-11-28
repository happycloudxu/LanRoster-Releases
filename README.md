# LanRoster

LanRoster keeps an inventory of the devices on your local network: one row per device, what
changed since the last scan, and a detail card for each device. It reads the Windows ARP cache
and, when the Npcap driver is installed, sweeps the subnet for a straight answer from every
device.

It is a native Windows desktop app — a Qt window with the scan history in SQLite — with no
account, no cloud service and no telemetry, and nothing about your network leaves the machine.

- **Device table** — name, IP, MAC, vendor, type, open ports and last seen, one row per
  device.
- **Detail card** — LAN address (and the mesh-VPN address, when a mesh VPN client on this PC
  can verify it), MAC, vendor, model, TTL, open ports, services and first/last seen, plus a
  presence chart and the device's change log (`new device`, `joined`, `left`,
  `ip changed`).
- **Activity feed** — the network-wide changes, shown when nothing is selected.
- **Tray icon** — left-click restores the window; right-click offers *Open LanRoster /
  Scan now / Quit*.
- **Background scanning** — the scan runs on a worker thread, so the window stays
  responsive and shows *Scanning…* until it finishes.

## Install

Install LanRoster from the
[Microsoft Store](https://apps.microsoft.com/detail/9N81MRGPCKWG). The package carries Python
and Qt inside, so nothing else has to be installed first: no runtime, no installer, no
unsigned-executable prompt, and the Store keeps it updated.

The Store edition is whichever build last cleared certification; **About** shows the exact
version you are running. Requires Windows 10/11 x64.

Press **Scan now** (`Ctrl+R`) to inventory the networks the machine is attached to, select a
row for its detail card, and use **Help** for the full guide. Run it as Administrator for the
most complete discovery.

## Raw ARP and Npcap

Raw ARP is the only way to read a device's MAC address straight from its reply and to get TTL
hints, and it needs the Npcap driver. The Store package cannot install it: Npcap is a kernel
driver that needs administrator rights outside a package.

Install the current Npcap from [npcap.com](https://npcap.com), keeping **WinPcap
API-compatible mode** checked, then restart LanRoster. `sc query npcap` reporting
`STATE : 4 RUNNING` is how you check the driver is up, and the header chip reads **Raw ARP via
Npcap** once it is working.

Without Npcap, LanRoster still runs, but lists only devices your PC already has an ARP entry
for, the header shows an amber **Limited discovery — install Npcap** chip, and the log says
`scapy ARP sweep unavailable`.

## Where your data lives

Everything LanRoster learns stays on the machine that ran the scan. Nothing is uploaded.

| What | Where |
|---|---|
| Database — devices, change events | `%LOCALAPPDATA%\LanRoster\data-<machine>\lanroster.db` |
| Vendor cache (OUI table) | `%LOCALAPPDATA%\LanRoster\data-<machine>\oui.csv`, refreshed in place |
| Log | `%LOCALAPPDATA%\LanRoster\lanroster-<machine>.log` |

`%LOCALAPPDATA%` is usually `C:\Users\<you>\AppData\Local`. `<machine>` is the computer name,
so several copies on one PC keep separate histories instead of sharing one database. The
folder that holds the executable is never written to — that is what lets the same code run
from a Store install, where the program folder is read-only. To start over, quit the app and
delete `%LOCALAPPDATA%\LanRoster`. There is no account, no server component and no cloud store.

To point the data somewhere else — a portable copy, a second disk — use any one of these, in
this order of precedence: a `--data-dir <path>` argument, the `LANROSTER_DATA_DIR` environment
variable, or an empty `portable.txt` file next to `LanRoster.exe`, which keeps the data beside
the executable. A one-time migration adopts a `data-<machine>\` database found beside an older
executable, so upgrade history carries over.

## What's new

**0.4.4** shows the version number on the main window: the title reads `LanRoster 0.4.4`
instead of just `LanRoster`, and the header shows `0.4.4` in small muted type beside the app
title, with a `LanRoster version 0.4.4` tooltip. The running build is now identifiable without
opening About.

**0.4.3** rebuilds Help as a full guide with a table of contents and internal links, and adds
an **Npcap and discovery quality** section: what the driver is, what degrades without it,
numbered install steps, licensing, and troubleshooting. The amber discovery chip now opens a
scrollable dialog instead of a cramped message box, so the detected state and its fix can be
read in full.

**0.4.2** stores writable state in `%LOCALAPPDATA%\LanRoster` for every install, portable or
Store, with no packaging detection: it fixes the packaged 0.4.0 build crashing while trying to
write inside its read-only install folder. A one-time migration copies an existing adjacent
`data-<machine>\` database into the new location, so history survives the upgrade.

**0.4.1** was the Microsoft Store packaging release. **0.4.0** renamed the application from
LanScope Desktop to LanRoster: `LanRoster.exe`, the `LANROSTER_*` environment variables, the
`lanroster.db` database and the `lanroster-<machine>.log` log. An existing pre-0.4 database is
adopted on first launch, so scan history carries over with no import step.

## Privacy

LanRoster sends nothing to the developer and has no sign-in, sync or cloud storage. What it
does send on your behalf, and exactly what it keeps in the data folder, are set out in the
[privacy statement](PRIVACY.md).

## Issues and support

Report a problem in the [issue tracker](../../issues) — that is the support link on the Store
listing — or to the author, Dr. Haiping Xu.

Scan only networks you are responsible for.
