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
    x-axis ["08-11 09:05", "08-11 10:05", "08-11 11:05", "08-11 12:05", "08-11 13:05", "08-11 14:05", "08-11 15:05", "08-11 16:05", "08-11 17:05", "08-11 18:05", "08-11 19:05", "08-11 20:05", "08-11 21:05", "08-11 22:05", "08-11 23:05", "08-12 00:05", "08-12 01:05", "08-12 02:05", "08-12 03:05", "08-12 04:05", "08-12 05:05", "08-12 06:05", "08-12 07:05", "08-12 08:05", "08-12 09:05", "08-12 10:05", "08-12 11:05", "08-12 12:05", "08-12 13:05", "08-12 14:05", "08-12 15:05", "08-12 16:05", "08-12 17:05", "08-12 18:05", "08-12 19:05", "08-12 20:05", "08-12 21:05", "08-12 22:05", "08-12 23:05", "08-13 00:05", "08-13 01:05", "08-13 02:05", "08-13 03:05", "08-13 04:05", "08-13 05:05", "08-13 06:05", "08-13 07:05", "08-13 08:05"]
    y-axis "IP Count" 0 --> 20000
    line [11000, 11060, 11138, 11207, 11277, 11349, 11412, 11473, 11531, 11592, 11652, 11747, 11833, 11899, 11952, 12023, 12101, 10630, 10719, 10767, 10836, 10919, 11016, 11089, 11143, 11238, 11312, 11357, 11431, 11580, 11676, 11768, 11857, 11948, 12031, 12133, 12239, 12315, 12397, 12470, 12586, 11039, 11165, 11282, 11407, 11505, 11584, 11689]
```

> **Current count:** 11689 IPs \&nbsp;|\&nbsp; **Tracking since:** 2026-08-11 09:05 \&nbsp;|\&nbsp; **Change (period):** +689
