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
    x-axis ["06-11 03:05", "06-11 04:05", "06-11 05:05", "06-11 06:05", "06-11 07:05", "06-11 08:05", "06-11 09:05", "06-11 10:05", "06-11 11:05", "06-11 12:05", "06-11 13:05", "06-11 14:05", "06-11 15:05", "06-11 16:05", "06-11 17:05", "06-11 18:05", "06-11 19:05", "06-11 20:05", "06-11 21:05", "06-11 22:05", "06-11 23:05", "06-12 00:05", "06-12 01:05", "06-12 02:05", "06-12 03:05", "06-12 04:05", "06-12 05:05", "06-12 06:05", "06-12 07:05", "06-12 08:05", "06-12 09:05", "06-12 10:05", "06-12 11:05", "06-12 12:05", "06-12 13:05", "06-12 14:05", "06-12 15:05", "06-12 16:05", "06-12 17:05", "06-12 18:05", "06-12 19:05", "06-12 20:05", "06-12 21:05", "06-12 22:05", "06-12 23:05", "06-13 00:05", "06-13 01:05", "06-13 02:05"]
    y-axis "IP Count" 0 --> 20000
    line [7908, 7989, 8053, 8107, 8161, 8203, 8266, 8303, 8337, 8467, 8562, 8682, 8722, 8764, 8813, 8857, 8883, 8912, 8962, 9012, 9088, 9120, 9175, 8071, 8095, 8166, 8220, 8301, 8354, 8406, 8444, 8492, 8521, 8570, 8613, 8654, 8681, 8718, 8762, 8801, 8855, 8907, 8938, 8989, 9034, 9100, 9136, 8055]
```

> **Current count:** 8055 IPs \&nbsp;|\&nbsp; **Tracking since:** 2026-06-11 03:05 \&nbsp;|\&nbsp; **Change (period):** +147
<!-- CHART_END -->

---

## Statistics (roughly)

- 🇨🇳 China: *Yes*
- 🇷🇺 Russia: *Also yes*
- 🇺🇸 USA (various VPS providers): *Surprisingly also yes*
- 🏠 My neighbour: *Hopefully no*

---

## Legal

This list contains exclusively IPs that have **actively attempted** to break into my systems. If you want your IP removed, stop attacking honeypots.

---

*Automatically updated by a bash script that works harder than most people.*
