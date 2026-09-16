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
    x-axis ["09-14 06:00", "09-14 07:00", "09-14 08:00", "09-14 09:00", "09-14 10:00", "09-14 11:00", "09-14 12:00", "09-14 13:00", "09-14 14:00", "09-14 15:00", "09-14 16:00", "09-14 17:00", "09-14 18:00", "09-14 19:00", "09-14 20:00", "09-14 21:00", "09-14 22:00", "09-14 23:00", "09-15 00:00", "09-15 01:00", "09-15 02:00", "09-15 03:00", "09-15 04:00", "09-15 05:00", "09-15 06:00", "09-15 07:00", "09-15 08:00", "09-15 09:00", "09-15 10:00", "09-15 10:18", "09-15 11:00", "09-15 12:00", "09-15 13:00", "09-15 14:00", "09-15 15:00", "09-15 16:00", "09-15 17:00", "09-15 18:00", "09-15 19:00", "09-15 19:39", "09-15 20:00", "09-15 21:00", "09-15 22:00", "09-15 23:00", "09-16 00:00", "09-16 01:00", "09-16 02:00", "09-16 03:00"]
    y-axis "IP Count" 0 --> 15500
    line [11359, 11424, 11481, 11543, 11606, 11675, 11745, 11848, 11904, 11973, 12047, 12106, 12173, 12233, 12304, 12361, 12415, 12470, 12530, 12603, 11046, 11081, 11149, 11228, 11298, 11380, 11468, 11572, 11651, 11679, 11727, 11802, 11912, 12019, 12114, 12206, 12263, 12361, 12439, 12497, 12529, 12614, 12669, 12723, 12781, 12837, 11314, 11378]
```

> **Current count:** 11378 IPs &nbsp;|&nbsp; **Tracking since:** 2026-09-14 06:00 &nbsp;|&nbsp; **Change (period):** +19
