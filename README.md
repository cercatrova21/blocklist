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
    x-axis ["10-15 14:00", "10-15 15:00", "10-15 16:00", "10-15 17:00", "10-15 18:00", "10-15 19:00", "10-15 20:00", "10-15 21:00", "10-15 22:00", "10-15 23:00", "10-16 00:00", "10-16 01:00", "10-16 02:00", "10-16 03:00", "10-16 04:00", "10-16 05:00", "10-16 06:00", "10-16 07:00", "10-16 08:00", "10-16 09:00", "10-16 10:00", "10-16 11:00", "10-16 12:00", "10-16 13:00", "10-16 14:00", "10-16 15:00", "10-16 16:00", "10-16 17:00", "10-16 18:00", "10-16 19:00", "10-16 20:00", "10-16 21:00", "10-16 22:00", "10-16 23:00", "10-17 00:00", "10-17 01:00", "10-17 02:00", "10-17 03:00", "10-17 04:00", "10-17 05:00", "10-17 06:00", "10-17 07:00", "10-17 08:00", "10-17 09:00", "10-17 10:00", "10-17 11:00", "10-17 12:00", "10-17 13:00"]
    y-axis "IP Count" 0 --> 15500
    line [11799, 11863, 11922, 11967, 12013, 12074, 12134, 12176, 12215, 12261, 12313, 12348, 11034, 11080, 11162, 11226, 11292, 11343, 11417, 11483, 11548, 11616, 11659, 11710, 11781, 11833, 11884, 11932, 11986, 12014, 12066, 12120, 12168, 12212, 12260, 12307, 10958, 11032, 11103, 11190, 11305, 11991, 12047, 12117, 12308, 12478, 12601, 12668]
```

> **Current count:** 12668 IPs &nbsp;|&nbsp; **Tracking since:** 2026-10-15 14:00 &nbsp;|&nbsp; **Change (period):** +869
