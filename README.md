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
    x-axis ["05-10 02:05", "05-10 03:05", "05-10 04:05", "05-10 05:05", "05-10 06:05", "05-10 07:05", "05-10 08:05", "05-10 09:05", "05-10 10:05", "05-10 11:05", "05-10 12:05", "05-10 13:05", "05-10 14:05", "05-10 15:05", "05-10 16:05", "05-10 17:05", "05-10 18:05", "05-10 19:05", "05-10 20:05", "05-10 21:05", "05-10 22:05", "05-10 23:05", "05-11 00:05", "05-11 01:05", "05-11 02:05", "05-11 03:05", "05-11 04:05", "05-11 05:05", "05-11 06:05", "05-11 07:05", "05-11 08:05", "05-11 09:05", "05-11 10:05", "05-11 11:05", "05-11 12:05", "05-11 13:05", "05-11 14:05", "05-11 15:05", "05-11 16:05", "05-11 17:05", "05-11 18:05", "05-11 19:05", "05-11 20:05", "05-11 21:05", "05-11 22:05", "05-11 23:05", "05-12 00:05", "05-12 01:05"]
    y-axis "IP Count" 0 --> 20000
    line [13142, 13189, 13250, 13307, 13349, 13398, 13449, 13502, 13550, 13595, 13639, 13689, 13736, 13788, 13830, 13868, 13933, 13985, 14003, 14048, 14096, 14143, 14208, 14300, 12510, 12531, 12563, 12620, 12684, 12728, 12771, 12824, 12856, 12916, 12960, 13011, 13062, 13112, 13159, 13215, 13308, 13385, 13457, 13518, 13568, 13618, 13660, 13704]
```

> **Current count:** 13704 IPs \&nbsp;|\&nbsp; **Tracking since:** 2026-05-10 02:05 \&nbsp;|\&nbsp; **Change (period):** +562
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
