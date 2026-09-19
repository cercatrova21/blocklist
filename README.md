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
    x-axis ["09-17 09:00", "09-17 10:00", "09-17 11:00", "09-17 12:00", "09-17 13:00", "09-17 14:00", "09-17 15:00", "09-17 16:00", "09-17 17:00", "09-17 18:00", "09-17 19:00", "09-17 20:00", "09-17 21:00", "09-17 22:00", "09-17 23:00", "09-18 00:00", "09-18 01:00", "09-18 02:00", "09-18 03:00", "09-18 04:00", "09-18 05:00", "09-18 06:00", "09-18 07:00", "09-18 08:00", "09-18 09:00", "09-18 10:00", "09-18 11:00", "09-18 12:00", "09-18 13:00", "09-18 14:00", "09-18 15:00", "09-18 16:00", "09-18 17:00", "09-18 18:00", "09-18 19:00", "09-18 20:00", "09-18 21:00", "09-18 22:00", "09-18 23:00", "09-19 00:00", "09-19 01:00", "09-19 02:00", "09-19 03:00", "09-19 04:00", "09-19 05:00", "09-19 06:00", "09-19 07:00", "09-19 08:00"]
    y-axis "IP Count" 0 --> 15500
    line [11535, 11606, 11667, 11736, 11794, 11857, 11915, 11969, 12024, 12081, 12125, 12180, 12233, 12270, 12322, 12368, 12414, 10779, 10824, 10892, 10961, 11013, 11073, 11121, 11177, 11226, 11271, 11322, 11372, 11421, 11479, 11531, 11581, 11637, 11704, 11776, 11840, 11881, 11918, 11956, 11993, 10687, 10731, 10780, 10834, 10894, 10938, 10991]
```

> **Current count:** 10991 IPs &nbsp;|&nbsp; **Tracking since:** 2026-09-17 09:00 &nbsp;|&nbsp; **Change (period):** -544
