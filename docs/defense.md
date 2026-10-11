# Detection & Mitigation

The purpose of studying this attack is to **defend against it**. This document maps
the Evil Portal technique to concrete, layered countermeasures.

---

## Threat classification

| Framework | Mapping |
|-----------|---------|
| **MITRE ATT&CK** | [T1557 — Adversary-in-the-Middle](https://attack.mitre.org/techniques/T1557/), [T1598 — Phishing for Information](https://attack.mitre.org/techniques/T1598/) |
| **OWASP** | Social Engineering / Phishing |
| **Attack type** | Rogue AP (Evil Twin) + captive-portal credential phishing |

---

## 1. Defenses for end users

| Signal to watch | What to do |
|-----------------|------------|
| 🔓 **Open network asking for a login** | Treat as hostile. Legitimate open hotspots rarely ask for account passwords (and never for your *Google/Microsoft* password). |
| 🔒 **No HTTPS / "Not secure"** | Never submit credentials. Real sign-in pages use HTTPS with a valid cert for the correct domain. |
| 🌐 **Wrong / odd domain** | Check the address bar. `172.0.0.1` or `msftconnecttest.com/redirect` is not `accounts.google.com`. |
| 🔁 **Duplicate SSIDs** | Two networks with the same name = possible evil twin. |
| 📲 **Auto-join** | Disable auto-join for open networks; "Forget" one-off hotspots after use. |
| 🔑 **Password manager** | It won't autofill on a spoofed domain — a built-in phishing check. |
| 🛂 **MFA** | A stolen password alone can't complete login. Prefer phishing-resistant MFA (passkeys / FIDO2). |
| 🧅 **VPN** | Encrypts traffic end-to-end on untrusted networks. |

**Golden rule:** *Never type a password into a page that opened automatically after
joining open Wi-Fi.*

---

## 2. Defenses for network defenders / organizations

| Control | Benefit |
|---------|---------|
| 📡 **WIDS/WIPS** | Detects rogue BSSIDs, evil-twin SSIDs, deauthentication floods, and unauthorized APs. |
| 🧭 **WPA3-Enterprise / 802.1X** | Mutual authentication — clients verify the network, defeating simple evil twins. |
| 🏷️ **Server-cert validation on supplicants** | Prevents clients from trusting rogue RADIUS/portal endpoints. |
| 📊 **Continuous RF monitoring** | Alerts on open APs impersonating the corporate SSID. |
| 🎓 **Security-awareness training** | Phishing & rogue-AP drills measurably reduce credential submission. |
| 🚫 **Captive-portal hardening** | Legitimate corporate portals should use HTTPS + a known domain users are taught to verify. |
| 🔐 **Passkeys / FIDO2 rollout** | Phishing-resistant by design — nothing reusable to harvest. |

---

## 3. Detecting an active evil twin

- Scan for **multiple APs broadcasting the same SSID** on different BSSIDs/channels.
- Watch for a **newly appeared open AP** matching a known corporate/public name.
- Correlate **deauth bursts** (used to push victims off the real AP) with new AP appearances.
- On your own device, note if a **"login" page appears for a network that never had one**.

---

## 4. If you suspect you were phished

1. Disconnect from the network immediately.
2. From a **trusted** connection, change the exposed password.
3. Revoke active sessions and rotate any reused passwords.
4. Verify/enable **MFA** and review recent account activity.
5. Report the rogue AP to the venue / your security team.

---

## Summary

Evil Portal succeeds on **human trust**, not a software vulnerability. The most
effective controls are therefore a mix of **user vigilance** (verify HTTPS, distrust
open-network logins), **phishing-resistant authentication** (passkeys + MFA), and
**network-side monitoring** (WIDS + WPA3-Enterprise).
