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
    x-axis ["11-03 06:00", "11-03 07:00", "11-03 07:13", "11-03 08:00", "11-03 09:00", "11-03 10:00", "11-03 11:00", "11-03 12:00", "11-03 13:00", "11-03 14:00", "11-03 15:00", "11-03 16:00", "11-03 17:00", "11-03 18:00", "11-03 19:00", "11-03 20:00", "11-03 21:00", "11-03 22:00", "11-03 23:00", "11-04 00:00", "11-04 01:00", "11-04 02:00", "11-04 03:00", "11-04 04:00", "11-04 05:00", "11-04 06:00", "11-04 07:00", "11-04 08:00", "11-04 09:00", "11-04 10:00", "11-04 11:00", "11-04 12:00", "11-04 13:00", "11-04 14:00", "11-04 15:00", "11-04 16:00", "11-04 17:00", "11-04 18:00", "11-04 19:00", "11-04 20:00", "11-04 21:00", "11-04 22:00", "11-04 23:00", "11-05 00:00", "11-05 01:00", "11-05 02:00", "11-05 03:00", "11-05 04:00"]
    y-axis "IP Count" 0 --> 15500
    line [11318, 11369, 11400, 11457, 11519, 11573, 11630, 11683, 11714, 11766, 11807, 11863, 11921, 11971, 12056, 12096, 12167, 12235, 12307, 12357, 10660, 10749, 10817, 10887, 10935, 11000, 11070, 11142, 11202, 11274, 11330, 11378, 11444, 11510, 11588, 11633, 11692, 11742, 11813, 11846, 11877, 11911, 11975, 12034, 10170, 10225, 10293, 10352]
```

> **Current count:** 10352 IPs &nbsp;|&nbsp; **Tracking since:** 2026-11-03 06:00 &nbsp;|&nbsp; **Change (period):** -966
