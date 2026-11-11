# Privacy statement — LanRoster

*Last updated: 11 November 2026 — version 0.4.0*

This statement covers every way you can get LanRoster: the Windows executable published in
this repository and, when it is published there, the Microsoft Store package.

## The short version

LanRoster reports nothing to the developer. There is no account, no telemetry, no
analytics, no advertising, no crash reporting and no update check. Everything the app
learns about your network stays on the PC that ran the scan.

What it does do is talk to the devices on your own local network. That is the whole point
of the app, and those devices can see it.

## What LanRoster sends over the network

1. **Probes to the devices on your subnet.** To build the inventory the app sends ARP
   requests, ICMP echo and TTL probes, and TCP connection attempts to a fixed list of
   common service ports. It also queries mDNS, SSDP/UPnP and NetBIOS, does reverse DNS
   lookups, and — when port 3389 answers — reads a Windows PC's computer name out of its
   RDP certificate. Each device sees an incoming request from your computer and may
   record it in its own logs. The app stays inside the IPv4 subnet it found on your
   machine; it never sweeps the internet or an address outside that subnet.
2. **Your DNS resolver.** The reverse lookups go through whichever DNS server your PC is
   configured to use, so that resolver sees requests for the addresses inside your own
   subnet.
3. **The device's own description page.** When a device answers SSDP, the app fetches the
   URL that device advertised and reads its friendly name, model and manufacturer
   strings. That request goes only to the address the device itself supplied.
4. **Optional hardware details, for one device, on your command.** Selecting a device and
   asking for its CPU, RAM and GPU makes the app query that machine over Tailscale SSH
   (which has to be enabled on it) or over WinRM with a username and password you type.
   The password is handed to a local PowerShell process on stdin: it is not stored, not
   written to the log, and not sent anywhere except that device's own WinRM service. What
   comes back is shown in the detail card and held in memory for that session only; it is
   not written to the database.
5. **Nothing else.** No update check, no licence check, no heartbeat, no remote
   configuration, and no contact with the developer's servers. The vendor-name table the
   app uses ships with it; refreshing it from the IEEE registry is a separate command-line
   step you run yourself, and the app never does it on its own.

The app has no sign-in, no sync, no cloud storage and no social features. It cannot see,
and never asks for, any account you may have anywhere.

## What LanRoster stores on your PC

All of it stays on your machine, and none of it is transmitted. The portable build keeps
it in the folder that holds `LanRoster.exe`; a Microsoft Store install cannot write to its
program folder, so it keeps the same files, under the same names, in
`%LOCALAPPDATA%\LanRoster` (usually `C:\Users\<you>\AppData\Local\LanRoster`).

| File | What | Why |
|---|---|---|
| `data-<machine>\lanroster.db` | the devices it found — MAC, IP, vendor, hostname, model, type, OS guess, open ports, services, TTL, first and last seen — plus each scan, each sighting and each change event | that is the inventory and its history |
| `data-<machine>\oui.csv` | the IEEE vendor-prefix table, copied out of the app on first run | so a MAC address can be turned into a manufacturer |
| `lanroster-<machine>.log` | scan start and finish, host counts and errors | so a failure can be diagnosed in a windowed build that has no console |

`<machine>` is your computer name, so several copies of the app in one folder keep separate
histories. In the portable build the data folder and the log sit beside the executable and
nothing is written to your user profile; in the Store build they sit in
`%LOCALAPPDATA%\LanRoster` and nothing is written to the read-only install folder. Neither
build writes to the registry or to system folders, and nothing is uploaded.

Quit the app and delete the `data-<machine>` folder and the log file to erase everything it
knows. Uninstalling the portable build removes the executable and leaves those files alone;
uninstalling the Store app removes the package, and Windows leaves
`%LOCALAPPDATA%\LanRoster` in place, so delete that folder too if you want the history
gone.

A portable install and a Store install do not share history: the Store build starts with an
empty inventory and never reads or deletes a `data-<machine>` folder left beside an old copy
of the portable executable.

Two more things happen entirely on your own machine: Windows CIM is asked for this PC's
OS, CPU, RAM and GPU so the detail card can describe the computer you are sitting at, and
the installed Tailscale client is asked for its local status so a device's tailnet address
can be shown. Neither sends anything out.

## What LanRoster never does

* It does not collect, transmit or sell personal data, device identifiers, usage
  statistics or crash dumps. Nothing reaches the developer at all.
* It does not record, store or analyse the content of your network traffic. It reads the
  ARP replies to its own requests and the responses to its own probes, and discards
  everything else it sees on the wire.
* It does not read your browser history, cookies, saved passwords or clipboard.
* It does not ask for a password except the optional WinRM pair for one device you chose,
  and that pair is used once and then dropped.
* It does not install itself at startup, keep a process running after the window closes,
  or scan unless you start a scan.
* It does not include third-party advertising, analytics or telemetry SDKs.

## Other people's devices

The inventory records the addresses, hardware names and open ports of devices that may
belong to other people — family, guests, colleagues. That is what a LAN inventory is, and
it stays on your PC, but it is still information about them. Scan only networks you are
responsible for, and think before sharing a database or a screenshot of it.

## Children

The app has no user accounts, no user-generated content and no data collection of any
kind, so it collects nothing from anyone, including children under 13.

## Third-party components

The executable is built with **Python**, **PySide6 / Qt** (LGPL-3.0), **scapy** and
**PyInstaller**, all bundled inside the app.

**Npcap** is not bundled. It is an optional packet-capture driver with its own licence,
installed separately from the download page or from <https://npcap.com>. Without it
LanRoster falls back to reading the Windows ARP cache, so hosts and names still appear but
MAC addresses are less reliable.

If you install LanRoster from the Microsoft Store, Microsoft handles the download and may
collect its own Store telemetry under the Microsoft Privacy Statement. That is Microsoft's
data, not the app's, and it does not include what LanRoster found on your network.

## Your rights

Because no personal data is collected or transmitted to the developer, there is nothing to
access, correct, export or delete on the developer's side, and no request to file. To
remove what the app has stored locally, quit it and delete the `data-<machine>` folder and
the `lanroster-<machine>.log` file, which for the portable build are beside the executable
and for the Store build are in `%LOCALAPPDATA%\LanRoster`. Devices on your network keep
their own logs of the probes they received; those are theirs to manage.

## Changes

If a future version changes any of the above — for example by adding an optional update
check — this statement will be updated before that version ships, and the change will be
described in the release notes.

## Contact

LanRoster is developed by **Dr. Haiping Xu** at <https://github.com/hxu-umassd>. Questions
and problem reports about the app, including privacy questions, can be sent there.
