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
    x-axis ["08-24 05:00", "08-24 06:00", "08-24 07:00", "08-24 08:00", "08-24 09:00", "08-24 10:00", "08-24 11:00", "08-24 12:00", "08-24 13:00", "08-24 14:00", "08-24 15:00", "08-24 16:00", "08-24 17:00", "08-24 18:00", "08-24 19:00", "08-24 20:00", "08-24 21:00", "08-24 22:00", "08-24 23:00", "08-25 00:00", "08-25 01:00", "08-25 02:00", "08-25 03:00", "08-25 04:00", "08-25 05:00", "08-25 06:00", "08-25 07:00", "08-25 08:00", "08-25 09:00", "08-25 10:00", "08-25 11:00", "08-25 12:00", "08-25 13:00", "08-25 14:00", "08-25 15:00", "08-25 16:00", "08-25 17:00", "08-25 18:00", "08-25 19:00", "08-25 20:00", "08-25 21:00", "08-25 22:00", "08-25 23:00", "08-26 00:00", "08-26 01:00", "08-26 02:00", "08-26 03:00", "08-26 04:00"]
    y-axis "IP Count" 0 --> 15000
    line [13400, 13469, 13548, 13614, 13674, 13742, 13816, 13877, 13926, 13991, 14048, 14103, 14139, 14190, 14227, 14299, 14357, 14414, 14465, 14507, 14560, 12783, 12870, 12962, 13060, 13143, 13215, 13289, 13404, 13498, 13573, 13667, 13783, 13883, 13974, 14058, 14152, 14221, 14291, 14380, 14478, 14541, 14619, 14688, 14758, 12086, 12185, 12304]
```

> **Current count:** 12304 IPs &nbsp;|&nbsp; **Tracking since:** 2026-08-24 05:00 &nbsp;|&nbsp; **Change (period):** -1096
