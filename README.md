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
    x-axis ["08-09 00:05", "08-09 01:05", "08-09 02:05", "08-09 03:05", "08-09 04:05", "08-09 05:05", "08-09 06:05", "08-09 07:05", "08-09 08:05", "08-09 09:05", "08-09 10:05", "08-09 11:05", "08-09 12:05", "08-09 13:05", "08-09 14:05", "08-09 15:05", "08-09 16:05", "08-09 17:05", "08-09 18:05", "08-09 19:05", "08-09 20:05", "08-09 21:05", "08-09 22:05", "08-09 23:05", "08-10 00:05", "08-10 01:05", "08-10 02:05", "08-10 03:05", "08-10 04:05", "08-10 05:05", "08-10 06:05", "08-10 07:05", "08-10 08:05", "08-10 09:05", "08-10 10:05", "08-10 11:05", "08-10 12:05", "08-10 13:05", "08-10 14:05", "08-10 15:05", "08-10 16:05", "08-10 17:05", "08-10 18:05", "08-10 19:05", "08-10 20:05", "08-10 21:05", "08-10 22:05", "08-10 23:05"]
    y-axis "IP Count" 0 --> 20000
    line [11836, 11889, 10054, 10094, 10178, 10252, 10326, 10398, 10451, 10521, 10574, 10635, 10682, 10748, 10812, 10877, 10946, 11002, 11069, 11153, 11235, 11297, 11356, 11414, 11476, 11531, 10152, 10244, 10334, 10410, 10482, 10572, 10637, 10719, 10771, 10831, 10896, 10959, 11014, 11089, 11144, 11198, 11292, 11343, 11439, 11510, 11574, 11638]
```

> **Current count:** 11638 IPs \&nbsp;|\&nbsp; **Tracking since:** 2026-08-09 00:05 \&nbsp;|\&nbsp; **Change (period):** -198
