# 🍯 Blocklist – Fresh From the Honeypot

---

## What is this?

An automatically updated list of IP addresses that apparently have nothing better to do than attack my honeypot.

These addresses have voluntarily – and with impressive consistency – decided to attack a server that **exists solely to be attacked**. Congratulations. 🎉

---

## How does it work?

```
Attacker:   "I'm gonna hack this server!"
Honeypot:   *notes down IP address*
Attacker:   *feels like a hacker*
Honeypot:   "Thank you for your submission. Have a lovely day."
```

This list is automatically updated every 60 minutes. The entries come from an Elasticsearch server that diligently logs all connection attempts from the past 7 days. Somewhere out there, a Russian port scanner is unknowingly contributing to an open-source project.

---

## What do I do with this list?

I block all these IPs on my production servers via nginx. But the list is also perfect for use in firewall solutions like **pfSense** or **OPNsense** – see below.

---

## Files

| File | Contents |
|---|---|
| `attacker_ips.txt` | The Hall of Shame – updated fresh daily |

---

## Using this list in pfSense

pfSense can automatically fetch and block IP lists using the **pfBlockerNG** package.

**1. Install pfBlockerNG**
Navigate to `System → Package Manager → Available Packages`, search for `pfBlockerNG` and install it.

**2. Add a new IP feed**
Go to `Firewall → pfBlockerNG → IP → IPv4` and click **Add**.

**3. Configure the feed**

| Field | Value |
|---|---|
| Name | `HoneypotBlocklist` |
| Description | `Fresh honeypot attacker IPs` |
| Source URL | `https://raw.githubusercontent.com/cercatrova21/blocklist/main/attacker_ips.txt` |
| Format | `Auto` |
| Action | `Deny Both` (or `Deny Inbound`) |

**4. Apply**
Go to `Firewall → pfBlockerNG → Update` and run a forced update. The IPs will now be blocked automatically and refreshed on your chosen schedule.

---

## Using this list in OPNsense

OPNsense handles IP blocklists natively through its built-in **Alias** and **Firewall Rule** system – no extra package needed.

**1. Create an Alias**
Go to `Firewall → Aliases` and click **+Add**.

| Field | Value |
|---|---|
| Name | `HoneypotBlocklist` |
| Type | `URL Table (IPs)` |
| URL | `https://raw.githubusercontent.com/cercatrova21/blocklist/main/attacker_ips.txt` |
| Refresh Interval | `1` (in days, or set to your preference) |
| Description | `Fresh honeypot attacker IPs` |

Click **Save** and then **Apply**.

**2. Create a Firewall Rule**
Go to `Firewall → Rules → WAN` and click **+Add**.

| Field | Value |
|---|---|
| Action | `Block` |
| Interface | `WAN` |
| Source | `HoneypotBlocklist` |
| Description | `Block honeypot attacker IPs` |

Click **Save** and **Apply Changes**. OPNsense will now automatically fetch the latest list and block all listed IPs at the firewall level.

---

## Frequently Asked Questions

**Is my IP in here?**
If you're asking, probably not. If you're *not* asking but still want to know: `grep "your.ip.here" attacker_ips.txt`

**Can I use this list?**
Please do. The more people block these IPs, the more frustrated the operators of these port scanners will be. That's the whole point.

**Are real humans being blocked?**
Possibly. But anyone with a legitimate reason to scan my honeypot is welcome to get in touch.

**How often is the list updated?**
Every 60 minutes. The attackers never sleep, so neither does the cronjob.

---

## IP Count Over Time

<!-- CHART_START -->
```mermaid
xychart-beta
    title "Blocklist IP Count Over Time"
    x-axis ["05-14 17:05", "05-14 18:05", "05-14 19:05", "05-14 20:05", "05-14 21:05", "05-14 22:05", "05-14 23:05", "05-15 00:05", "05-15 01:05", "05-15 02:05", "05-15 03:05", "05-15 04:05", "05-15 05:05", "05-15 06:05", "05-15 07:05", "05-15 08:05", "05-15 09:05", "05-15 10:05", "05-15 11:05", "05-15 12:05", "05-15 13:05", "05-15 14:05", "05-15 15:05", "05-15 16:05", "05-15 17:05", "05-15 18:05", "05-15 19:05", "05-15 20:05", "05-15 21:05", "05-15 22:05", "05-15 23:05", "05-16 00:05", "05-16 01:05", "05-16 02:05", "05-16 03:05", "05-16 04:05", "05-16 05:05", "05-16 06:05", "05-16 07:05", "05-16 08:05", "05-16 09:05", "05-16 10:05", "05-16 11:05", "05-16 12:05", "05-16 13:05", "05-16 14:05", "05-16 15:05", "05-16 16:05"]
    y-axis "IP Count" 0 --> 20000
    line [11981, 12056, 12160, 12232, 12292, 12327, 12414, 12475, 12516, 9184, 9240, 9296, 9357, 9399, 9476, 9533, 9590, 9631, 9709, 9762, 9828, 9879, 9910, 9952, 9984, 10050, 10079, 10116, 10153, 10195, 10233, 10274, 10323, 8921, 8971, 9058, 9126, 9196, 9239, 9271, 9320, 9393, 9467, 9547, 9603, 9657, 9691, 9731]
```

> **Current count:** 9731 IPs \&nbsp;|\&nbsp; **Tracking since:** 2026-05-14 17:05 \&nbsp;|\&nbsp; **Change (period):** -2250
<!-- CHART_END -->

---

## Statistics (roughly)

- 🇨🇳 China: *Yes*
- 🇷🇺 Russia: *Also yes*
- 🇺🇸 USA (various VPS providers): *Surprisingly also yes*
- 🏠 My neighbour: *Hopefully no*

---

## Legal

This list contains exclusively IPs that have **actively attempted** to break into my systems. If you want your IP removed, stop attacking honeypots.

---

*Automatically updated by a bash script that works harder than most people.*
