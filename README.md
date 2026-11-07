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
    x-axis ["11-05 06:00", "11-05 07:00", "11-05 08:00", "11-05 09:00", "11-05 10:00", "11-05 11:00", "11-05 12:00", "11-05 13:00", "11-05 14:00", "11-05 15:00", "11-05 16:00", "11-05 17:00", "11-05 18:00", "11-05 19:00", "11-05 20:00", "11-05 21:00", "11-05 22:00", "11-05 23:00", "11-06 00:00", "11-06 01:00", "11-06 02:00", "11-06 03:00", "11-06 04:00", "11-06 05:00", "11-06 06:00", "11-06 07:00", "11-06 08:00", "11-06 09:00", "11-06 10:00", "11-06 11:00", "11-06 12:00", "11-06 13:00", "11-06 14:00", "11-06 15:00", "11-06 15:15", "11-06 15:17", "11-06 16:00", "11-06 17:00", "11-06 18:00", "11-06 19:00", "11-06 20:00", "11-06 21:00", "11-06 22:00", "11-06 23:00", "11-07 00:00", "11-07 01:00", "11-07 02:00", "11-07 03:00"]
    y-axis "IP Count" 0 --> 15500
    line [10488, 10550, 10617, 10699, 10745, 10819, 10891, 10953, 11014, 11076, 11160, 11245, 11310, 11408, 11467, 11519, 11556, 11625, 11695, 10076, 10136, 10227, 10299, 10369, 10438, 10509, 10606, 10690, 10767, 10983, 11040, 11080, 11113, 11149, 11169, 11170, 11212, 11259, 11303, 11382, 11443, 11498, 11547, 11594, 11642, 10288, 10347, 10407]
```

> **Current count:** 10407 IPs &nbsp;|&nbsp; **Tracking since:** 2026-11-05 06:00 &nbsp;|&nbsp; **Change (period):** -81
