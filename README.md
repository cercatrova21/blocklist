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
    x-axis ["11-19 20:00", "11-19 21:00", "11-19 22:00", "11-19 23:00", "11-20 00:00", "11-20 01:00", "11-20 02:00", "11-20 03:00", "11-20 04:00", "11-20 05:00", "11-20 06:00", "11-20 07:00", "11-20 08:00", "11-20 09:00", "11-20 10:00", "11-20 11:00", "11-20 12:00", "11-20 13:00", "11-20 14:00", "11-20 15:00", "11-20 16:00", "11-20 17:00", "11-20 18:00", "11-20 19:00", "11-20 19:25", "11-20 20:00", "11-20 21:00", "11-20 22:00", "11-20 23:00", "11-21 00:00", "11-21 01:00", "11-21 02:00", "11-21 03:00", "11-21 04:00", "11-21 05:00", "11-21 06:00", "11-21 07:00", "11-21 08:00", "11-21 09:00", "11-21 10:00", "11-21 11:00", "11-21 12:00", "11-21 13:00", "11-21 14:00", "11-21 15:00", "11-21 16:00", "11-21 17:00", "11-21 18:00"]
    y-axis "IP Count" 0 --> 29000
    line [28121, 28178, 28225, 28257, 28320, 20689, 21932, 23533, 24715, 25567, 26172, 26583, 26894, 27129, 27301, 27421, 27539, 27629, 27718, 27788, 27872, 27937, 28006, 28079, 28105, 28149, 28211, 28263, 28302, 28360, 26956, 27006, 27066, 27145, 27212, 27287, 27367, 27450, 27552, 27641, 27702, 27785, 27854, 27905, 27970, 28030, 28090, 28152]
```

> **Current count:** 28152 IPs &nbsp;|&nbsp; **Tracking since:** 2026-11-19 20:00 &nbsp;|&nbsp; **Change (period):** +31
