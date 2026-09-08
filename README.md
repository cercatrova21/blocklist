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
    x-axis ["09-06 15:00", "09-06 16:00", "09-06 17:00", "09-06 18:00", "09-06 18:20", "09-06 19:00", "09-06 20:00", "09-06 21:00", "09-06 22:00", "09-06 23:00", "09-07 00:00", "09-07 01:00", "09-07 02:00", "09-07 03:00", "09-07 04:00", "09-07 05:00", "09-07 06:00", "09-07 07:00", "09-07 08:00", "09-07 09:00", "09-07 10:00", "09-07 11:00", "09-07 12:00", "09-07 13:00", "09-07 14:00", "09-07 15:00", "09-07 16:00", "09-07 17:00", "09-07 18:00", "09-07 19:00", "09-07 20:00", "09-07 21:00", "09-07 22:00", "09-07 23:00", "09-08 00:00", "09-08 01:00", "09-08 02:00", "09-08 03:00", "09-08 04:00", "09-08 05:00", "09-08 06:00", "09-08 07:00", "09-08 08:00", "09-08 09:00", "09-08 10:00", "09-08 11:00", "09-08 12:00", "09-08 13:00"]
    y-axis "IP Count" 0 --> 15500
    line [14017, 14076, 14140, 14202, 14227, 14275, 14350, 14445, 14506, 14577, 14638, 14689, 13043, 13095, 13178, 13244, 13337, 13418, 13479, 13552, 13641, 13704, 13778, 13837, 13904, 13957, 14009, 14072, 14125, 14190, 14258, 14325, 14380, 14451, 14544, 14614, 13223, 13286, 13354, 13457, 13534, 13601, 13665, 13736, 13796, 13854, 13917, 13983]
```

> **Current count:** 13983 IPs &nbsp;|&nbsp; **Tracking since:** 2026-09-06 15:00 &nbsp;|&nbsp; **Change (period):** -34
