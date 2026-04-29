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
    x-axis ["04-28 00:05", "04-28 01:05", "04-28 02:05", "04-28 03:05", "04-28 04:05", "04-28 05:05", "04-28 06:05", "04-28 07:05", "04-28 08:05", "04-28 09:05", "04-28 10:05", "04-28 11:05", "04-28 12:05", "04-28 13:05", "04-28 14:05", "04-28 15:05", "04-28 16:05", "04-28 17:05", "04-28 18:05", "04-28 19:05", "04-28 20:05", "04-28 21:05", "04-28 22:05", "04-28 23:05", "04-29 00:05", "04-29 01:05", "04-29 02:05", "04-29 03:05", "04-29 04:05", "04-29 05:05", "04-29 06:05", "04-29 07:05", "04-29 08:05", "04-29 09:05", "04-29 10:05", "04-29 11:05", "04-29 12:05", "04-29 13:05", "04-29 14:05", "04-29 15:05", "04-29 16:05", "04-29 17:05", "04-29 18:05", "04-29 19:05", "04-29 20:05", "04-29 21:05", "04-29 22:05", "04-29 23:05"]
    y-axis "IP Count" 0 --> 19500
    line [18901, 18963, 16581, 16629, 16719, 16817, 16936, 17043, 17141, 17259, 17352, 17446, 17515, 17588, 17668, 17744, 17819, 17899, 18000, 18198, 18365, 18519, 18676, 18829, 18975, 19085, 15784, 16025, 16210, 16455, 16646, 16822, 17010, 17150, 17324, 17487, 17561, 17632, 17714, 17818, 17898, 17972, 18060, 18206, 18305, 18383, 18509, 18634]
```

> **Current count:** 18634 IPs \&nbsp;|\&nbsp; **Tracking since:** 2026-04-28 00:05 \&nbsp;|\&nbsp; **Change (period):** -267
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
