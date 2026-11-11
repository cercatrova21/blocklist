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
    x-axis ["11-09 22:00", "11-09 23:00", "11-10 00:00", "11-10 01:00", "11-10 02:00", "11-10 03:00", "11-10 04:00", "11-10 05:00", "11-10 06:00", "11-10 07:00", "11-10 08:00", "11-10 09:00", "11-10 10:00", "11-10 11:00", "11-10 12:00", "11-10 13:00", "11-10 14:00", "11-10 15:00", "11-10 16:00", "11-10 17:00", "11-10 18:00", "11-10 19:00", "11-10 20:00", "11-10 21:00", "11-10 22:00", "11-10 23:00", "11-11 00:00", "11-11 01:00", "11-11 02:00", "11-11 03:00", "11-11 04:00", "11-11 05:00", "11-11 06:00", "11-11 07:00", "11-11 08:00", "11-11 09:00", "11-11 10:00", "11-11 11:00", "11-11 12:00", "11-11 13:00", "11-11 14:00", "11-11 15:00", "11-11 16:00", "11-11 17:00", "11-11 18:00", "11-11 19:00", "11-11 20:00", "11-11 21:00"]
    y-axis "IP Count" 0 --> 27500
    line [11992, 12024, 12090, 10743, 10813, 10870, 10952, 11029, 11082, 11150, 11203, 11248, 11297, 11340, 11387, 11434, 11478, 11540, 11599, 11649, 11702, 11784, 11859, 11912, 11950, 11992, 12039, 10732, 10788, 10861, 10928, 11023, 11118, 11173, 11236, 11289, 16065, 20558, 23159, 24688, 25579, 26166, 26590, 26839, 27007, 27139, 27238, 27298]
```

> **Current count:** 27298 IPs &nbsp;|&nbsp; **Tracking since:** 2026-11-09 22:00 &nbsp;|&nbsp; **Change (period):** +15306
