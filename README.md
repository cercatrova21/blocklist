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
    x-axis ["11-21 08:00", "11-21 09:00", "11-21 10:00", "11-21 11:00", "11-21 12:00", "11-21 13:00", "11-21 14:00", "11-21 15:00", "11-21 16:00", "11-21 17:00", "11-21 18:00", "11-21 19:00", "11-21 20:00", "11-21 21:00", "11-21 22:00", "11-21 23:00", "11-22 00:00", "11-22 01:00", "11-22 02:00", "11-22 03:00", "11-22 04:00", "11-22 05:00", "11-22 06:00", "11-22 07:00", "11-22 08:00", "11-22 09:00", "11-22 10:00", "11-22 11:00", "11-22 12:00", "11-22 13:00", "11-22 14:00", "11-22 15:00", "11-22 16:00", "11-22 17:00", "11-22 18:00", "11-22 19:00", "11-22 20:00", "11-22 21:00", "11-22 22:00", "11-22 23:00", "11-23 00:00", "11-23 01:00", "11-23 02:00", "11-23 03:00", "11-23 04:00", "11-23 05:00", "11-23 06:00", "11-23 07:00"]
    y-axis "IP Count" 0 --> 29500
    line [27450, 27552, 27641, 27702, 27785, 27854, 27905, 27970, 28030, 28090, 28152, 28238, 28289, 28347, 28401, 28447, 28504, 27161, 27217, 27269, 27338, 27421, 27503, 27599, 27675, 27749, 27825, 27898, 27970, 28026, 28076, 28143, 28209, 28266, 28320, 28398, 28506, 29299, 29363, 29431, 29491, 27721, 27797, 27866, 27928, 27977, 28049, 28109]
```

> **Current count:** 28109 IPs &nbsp;|&nbsp; **Tracking since:** 2026-11-21 08:00 &nbsp;|&nbsp; **Change (period):** +659
