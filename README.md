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
    x-axis ["05-07 06:05", "05-07 07:05", "05-07 08:05", "05-07 08:14", "05-07 09:05", "05-07 10:05", "05-07 11:05", "05-07 12:05", "05-07 13:05", "05-07 14:05", "05-07 15:05", "05-07 16:05", "05-07 17:05", "05-07 18:05", "05-07 19:05", "05-07 20:05", "05-07 21:05", "05-07 22:05", "05-07 23:05", "05-08 00:05", "05-08 01:05", "05-08 02:05", "05-08 03:05", "05-08 04:05", "05-08 05:05", "05-08 06:05", "05-08 07:05", "05-08 08:05", "05-08 09:05", "05-08 09:58", "05-08 10:05", "05-08 11:05", "05-08 12:05", "05-08 13:05", "05-08 14:05", "05-08 15:05", "05-08 16:05", "05-08 17:05", "05-08 18:05", "05-08 19:05", "05-08 20:05", "05-08 20:55", "05-08 21:05", "05-08 22:05", "05-08 23:05", "05-09 00:05", "05-09 01:05", "05-09 02:05"]
    y-axis "IP Count" 0 --> 20000
    line [14085, 14327, 14478, 14478, 14670, 14863, 15009, 15194, 15320, 15444, 15555, 15669, 15773, 15878, 16009, 16117, 16207, 16279, 16350, 16432, 16515, 14476, 14557, 14652, 14748, 14846, 14934, 15022, 15071, 15118, 15120, 15153, 15185, 15226, 15262, 15327, 15384, 15429, 15489, 15524, 15553, 15599, 15608, 15657, 15699, 15740, 15782, 13803]
```

> **Current count:** 13803 IPs \&nbsp;|\&nbsp; **Tracking since:** 2026-05-07 06:05 \&nbsp;|\&nbsp; **Change (period):** -282
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
