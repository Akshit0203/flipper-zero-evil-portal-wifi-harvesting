# Setup — Firmware Flashing & Hardware

This guide covers preparing the Flipper Zero and its ESP32 Wi-Fi Dev Board to run
the Marauder Evil Portal module.

> ⚠️ Perform every step only on hardware you own. See [`../DISCLAIMER.md`](../DISCLAIMER.md).

---

## 1. Hardware

| Item | Notes |
|------|-------|
| Flipper Zero | Host device; provides the UI, GPIO power, and SD-card storage |
| Wi-Fi Dev Board (ESP32-WROOM) | Official Flipper Zero Wi-Fi Module v1 or a compatible ESP32 board |
| USB-C cable | For flashing the ESP32 directly and for serial access |
| microSD card | Stores the Evil Portal HTML payload and the capture logs |

The ESP32 board mounts onto the Flipper Zero's GPIO header. It is the component that
actually performs all Wi-Fi operations — the Flipper itself has no Wi-Fi radio.

![Flipper Zero + ESP32 Wi-Fi board](../assets/images/04-flipper-evil-portal-menu.jpg)

---

## 2. Flashing Marauder onto the ESP32

Marauder is an ESP32 firmware; it is flashed to the **Wi-Fi board**, not to the
Flipper's own firmware.

### Option A — Web flasher (easiest)
1. Connect the Wi-Fi board to your PC via USB-C.
2. Open the [Marauder Web Flasher](https://sfbserbia.github.io/Flipper-Marauder-flasher/) in a Chromium-based browser.
3. Select the correct board (e.g., *Flipper Zero Wi-Fi Dev Board / ESP32-WROOM*).
4. Hold **BOOT**, tap **RESET**, release **BOOT** to enter bootloader mode.
5. Flash the latest Marauder release and wait for completion.

### Option B — qFlipper / esptool
- qFlipper can be used to manage the companion app and SD assets.
- Advanced users can flash with `esptool.py` using the official Marauder `.bin`
  files and partition offsets from the Marauder wiki.

![Flashing with qFlipper](../assets/images/01-qflipper-firmware-flash.png)

> Always flash **official Marauder releases** from
> <https://github.com/justcallmekoko/ESP32Marauder/releases>.

---

## 3. Deploying the companion app on the Flipper

Install the **ESP32 Marauder** companion app on the Flipper Zero (available via the
Flipper app catalog / qFlipper). It exposes the firmware's features through the menu:

```
Apps → GPIO → [ESP32] Marauder  (or)  Apps → GPIO → ESP → Evil Portal
```

---

## 4. Preparing the SD card

The Evil Portal module reads its HTML payload and writes logs on the SD card:

```
SD Card/apps_data/evil_portal/
├── index.html         # (optional) default portal page
└── logs/
    ├── log_0.txt
    ├── log_1.txt
    └── ...
```

Copy the captive-portal payload ([`../src/portal/google_login.html`](../src/portal/google_login.html))
into the Evil Portal data folder, or load it over serial with `sethtmlstr`
(see [`methodology.md`](methodology.md)).

---

Next: [`methodology.md`](methodology.md) — the full step-by-step attack walkthrough.
