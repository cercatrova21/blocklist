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
    x-axis ["09-29 07:00", "09-29 08:00", "09-29 09:00", "09-29 10:00", "09-29 11:00", "09-29 12:00", "09-29 13:00", "09-29 14:00", "09-29 15:00", "09-29 16:00", "09-29 17:00", "09-29 18:00", "09-29 19:00", "09-29 20:00", "09-29 21:00", "09-29 22:00", "09-29 23:00", "09-30 00:00", "09-30 01:00", "09-30 02:00", "09-30 03:00", "09-30 04:00", "09-30 05:00", "09-30 06:00", "09-30 07:00", "09-30 08:00", "09-30 08:39", "09-30 08:46", "09-30 09:00", "09-30 10:00", "09-30 11:00", "09-30 12:00", "09-30 13:00", "09-30 14:00", "09-30 15:00", "09-30 16:00", "09-30 17:00", "09-30 18:00", "09-30 19:00", "09-30 20:00", "09-30 21:00", "09-30 22:00", "09-30 23:00", "10-01 00:00", "10-01 01:00", "10-01 02:00", "10-01 03:00", "10-01 04:00"]
    y-axis "IP Count" 0 --> 15500
    line [12242, 12320, 12401, 12461, 12538, 12611, 12677, 12720, 12785, 12837, 12897, 12984, 13054, 13155, 13209, 13264, 13320, 13370, 13412, 11326, 11407, 11480, 11583, 11674, 11776, 11860, 11897, 11910, 11924, 12015, 12105, 12174, 12232, 12296, 12353, 12397, 12446, 12495, 12547, 12623, 12670, 12724, 12761, 12806, 12858, 9410, 9464, 9562]
```

> **Current count:** 9562 IPs &nbsp;|&nbsp; **Tracking since:** 2026-09-29 07:00 &nbsp;|&nbsp; **Change (period):** -2680
