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
    x-axis ["05-04 21:05", "05-04 22:05", "05-04 23:05", "05-05 00:05", "05-05 01:05", "05-05 02:05", "05-05 03:05", "05-05 04:05", "05-05 05:05", "05-05 06:05", "05-05 07:05", "05-05 08:05", "05-05 09:05", "05-05 10:05", "05-05 11:05", "05-05 12:05", "05-05 13:05", "05-05 14:05", "05-05 15:05", "05-05 16:05", "05-05 17:05", "05-05 18:05", "05-05 19:05", "05-05 20:05", "05-05 21:05", "05-05 22:05", "05-05 23:05", "05-06 00:05", "05-06 01:05", "05-06 02:05", "05-06 03:05", "05-06 04:05", "05-06 05:05", "05-06 06:05", "05-06 07:05", "05-06 08:05", "05-06 09:05", "05-06 10:05", "05-06 11:05", "05-06 12:05", "05-06 13:05", "05-06 14:05", "05-06 15:05", "05-06 16:05", "05-06 17:05", "05-06 18:05", "05-06 19:05", "05-06 20:05"]
    y-axis "IP Count" 0 --> 20000
    line [19283, 19360, 19423, 19477, 19535, 18107, 18164, 18233, 18333, 18394, 18461, 18542, 18612, 18649, 18707, 18774, 18840, 18911, 18983, 19060, 19144, 19217, 19301, 19376, 19420, 19466, 19520, 19568, 19619, 16605, 16648, 16715, 16793, 16907, 16987, 17058, 17143, 17231, 17294, 17368, 17427, 17517, 17578, 17630, 17701, 17782, 17875, 17984]
```

> **Current count:** 17984 IPs \&nbsp;|\&nbsp; **Tracking since:** 2026-05-04 21:05 \&nbsp;|\&nbsp; **Change (period):** -1299
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
