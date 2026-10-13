# Aircrack-ng Suite — Command Reference

Personal reference for the Pi WiFi assessment build. Monitor interface is the
MT7612U as `wlan1`; onboard WiFi (`wlan0`) stays as the management AP, so be
careful with any command that touches network services (see the `check kill`
note below).

Add new commands under the relevant tool as you learn them.

---

## airmon-ng

Manages monitor mode on wireless interfaces.

| Command | What it does |
|---|---|
| `airmon-ng` | Lists detected wireless interfaces, their driver and chipset. Run with no args to see what's available. |
| `airmon-ng start wlan1` | Puts `wlan1` into monitor mode. Usually creates/renames to a monitor interface (e.g. `wlan1mon`) — note the name it reports, you'll use it for other tools. |
| `airmon-ng check kill` | Kills processes that interfere with monitor mode (NetworkManager, wpa_supplicant). ⚠️ **On this Pi build this also disrupts `wlan0`'s management AP / any WiFi uplink.** Prefer stopping only the specific conflicting process, or put `wlan1` into monitor mode with `iw` instead, so you don't lock yourself out. |
| `airmon-ng stop wlan1mon` | Takes the monitor interface back out of monitor mode and returns it to managed mode (reverses `start`). Note the name is `wlan1mon` — the monitor interface created by `start`, not the original `wlan1`. |

<!-- Add more airmon-ng commands here -->

---

## airodump-ng

Captures/surveys nearby networks and clients on the monitor interface.

| Command | What it does |
|---|---|
| `airodump-ng wlan1mon` | Starts a live survey on `wlan1mon` — lists nearby APs (BSSID, channel, encryption, signal) and associated clients, hopping across channels. Passive; good first look at what's in range. Ctrl-C to stop. |
| `sudo airodump-ng --channel 6 --bssid 76:AF:C5:B3:F3:59 --write wpa-test1 wlan1mon` | **Targeted capture.** Locks to one AP instead of channel-hopping: `--channel 6` stays on channel 6, `--bssid 76:AF:C5:B3:F3:59` filters to just that AP, `--write wpa-test1` saves to files prefixed `wpa-test1` (`.cap`, `.csv`, etc.). This is how you capture a specific network's traffic — e.g. to catch a WPA handshake for later analysis. `sudo` needed to write capture files. |

<!-- Add more airodump-ng commands here -->

> **Capture file location:** `--write` saves to the current directory by default.
> On the Pi, either `cd` to a dedicated captures folder first or give a full path
> (e.g. `--write /home/pi/captures/wpa-test1`) to keep evidence organised — this
> matters for the reporting/evidence side of an engagement.

---

## aireplay-ng

Injects / replays frames (test injection, deauth, fakeauth, etc.).

| Command | What it does |
|---|---|
| `aireplay-ng -9 wlan1mon` | **Injection test** (`-9` = `--test`). Checks whether the card can successfully inject frames and which nearby APs respond. Good first check after enabling monitor mode to confirm the MT7612U is working. Use the monitor interface name `wlan1mon` (the one `airmon-ng start` created), not the base `wlan1`. |
| `sudo aireplay-ng --deauth 5 -a 76:AF:C5:B3:F3:59 -c <client-MAC> wlan1mon` | **Deauthentication.** Sends deauth frames to knock a client off the AP so it reconnects — forcing the 4-way handshake you capture with airodump-ng. `--deauth 5` = 5 bursts, `-a` = AP BSSID, `-c` = target client (omit `-c` to deauth all clients, noisier). ⚠️ **Active and disruptive** — only against your own test AP. Run alongside a targeted `airodump-ng` so you catch the handshake when the client comes back. |

<!-- Add more aireplay-ng commands here -->

---

## WPA Attacks

Offline analysis of a captured WPA/WPA2 handshake (the `.cap` from a targeted
`airodump-ng` capture). These steps run against a capture file, not the live
network — but you still need authorisation to have captured the handshake in the
first place.

### Prepare a WPA-valid wordlist

```bash
cat rockyou.txt | sort | uniq | pw-inspector -m 8 -M 63 > wparockyou.txt
```

Filters `rockyou.txt` down to candidates that could actually be WPA passphrases:
`sort | uniq` removes duplicates, then `pw-inspector -m 8 -M 63` keeps only
entries between **8 and 63 characters** (the valid WPA/WPA2 passphrase length
range). Dropping impossible-length words makes the dictionary attack faster.

### Dictionary attack against the capture

```bash
aircrack-ng -w wparockyou.txt wpa_crack-01.cap
```

Runs a dictionary attack: `-w wparockyou.txt` is the wordlist, `wpa_crack-01.cap`
is the capture file containing the handshake. aircrack-ng tries each candidate
against the captured handshake and reports the passphrase if one matches. Needs a
**complete 4-way handshake** in the capture (airodump-ng shows "WPA handshake" in
the top-right when it's got one) — no handshake, nothing to test against.

### Faster cracking with hashcat (off the Pi)

The aircrack-ng path above is CPU-only and slow. For serious wordlists, convert
the capture and crack it on a machine with a GPU — **not the Pi 3B**, which has
no usable GPU for this:

```bash
hcxpcapngtool -o wpa-test1.22000 wpa-test1.cap
hashcat -m 22000 wpa-test1.22000 wparockyou.txt
```

`hcxpcapngtool` (from the `hcxtools` package) converts the capture into hashcat's
mode **22000** (WPA-PBKDF2-PMKID+EAPOL) format; `hashcat -m 22000` then runs the
attack GPU-accelerated. Same wordlist, orders of magnitude faster. The Pi's job
is capture; move the file to a workstation for cracking.

<!-- Add more WPA attack commands here -->

---

> **Scope reminder:** only run active commands (injection, deauth, replay) against
> networks you own or are authorised in writing to test.
