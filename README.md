<div align="center">

```
██████╗ ██████╗ ██╗   ██╗ ██████╗███████╗
██╔══██╗██╔══██╗██║   ██║██╔════╝██╔════╝
██████╔╝██████╔╝██║   ██║██║     █████╗  
██╔══██╗██╔══██╗██║   ██║██║     ██╔══╝  
██████╔╝██║  ██║╚██████╔╝╚██████╗███████╗
╚═════╝ ╚═╝  ╚═╝ ╚═════╝  ╚═════╝╚══════╝
         ESP32 // CYD Edition
```

[![ESP32](https://img.shields.io/badge/ESP32-WROOM--32-red?style=for-the-badge&logo=espressif&logoColor=white)](https://www.espressif.com/)
[![Bruce Firmware](https://img.shields.io/badge/Firmware-Bruce-black?style=for-the-badge&logo=github&logoColor=white)](https://github.com/BruceDevices/firmware)
[![License](https://img.shields.io/badge/License-AGPL--3.0-blue?style=for-the-badge)](LICENSE)
[![Educational](https://img.shields.io/badge/Purpose-Educational%20Only-orange?style=for-the-badge)](README.md)
[![WiFi](https://img.shields.io/badge/WiFi-2.4GHz-green?style=for-the-badge&logo=wifi&logoColor=white)]()
[![Bluetooth](https://img.shields.io/badge/Bluetooth-4.2%20%2B%20BLE-blue?style=for-the-badge&logo=bluetooth&logoColor=white)]()

> **Birkaç yüz TL'lik ESP32 ile Flipper Zero benzeri offensive security araçları.**
> Kendi ağlarında, yetkili testler için.

![Evil Portal Demo](images/evil_portal.jpg)

</div>

---

## 📋 İçindekiler

- [Donanım](#-donanım)
- [Firmware Kurulumu](#-firmware-kurulumu)
- [Özellikler](#-özellikler)
- [Demo](#-demo)
- [Korunma Yöntemleri](#️-korunma-yöntemleri)
- [Yasal Uyarı](#-yasal-uyarı)

---

## 🔧 Donanım

<div align="center">

| Özellik | Detay |
|:---:|:---:|
| **Board** | ESP32-2432S028R (Cheap Yellow Display) |
| **MCU** | ESP32-WROOM-32, Dual-core 240 MHz |
| **Ekran** | 2.8" TFT ILI9341, 320×240 px |
| **Dokunmatik** | Resistive (XPT2046) |
| **WiFi** | 802.11 b/g/n · 2.4 GHz |
| **Bluetooth** | BT 4.2 + BLE |
| **Depolama** | MicroSD kart slotu |
| **Güç** | USB-C / Micro-USB |
| **Fiyat** | ~$10–15 (AliExpress) |

</div>

---

## 💾 Firmware Kurulumu

[![Flash Now](https://img.shields.io/badge/🔥%20Flash%20Now-bruce.computer%2Fflasher-red?style=for-the-badge)](https://bruce.computer/flasher)

<details>
<summary><b>🌐 Web Flasher ile (Önerilen)</b></summary>

```
1. Chrome'da bruce.computer/flasher aç
2. CYD'yi USB ile bağla
3. "Connect" → cihazı seç → "Flash"
4. Bitti! Cihaz otomatik açılır.
```
</details>

<details>
<summary><b>💻 esptool ile (Manuel)</b></summary>

```bash
# esptool kur
pip install esptool

# Firmware yükle
esptool.py --port /dev/ttyUSB0 write_flash 0x00000 Bruce-CYD.bin
```
</details>

<details>
<summary><b>🔧 OTA (Kablosuz Güncelleme)</b></summary>

```
Settings → OTA Update → WiFi'a bağlan → Güncelle
```
Bruce kendi içinde OTA desteğiyle geliyor, USB'ye gerek yok.
</details>

---

## ⚡ Özellikler

### 📶 WiFi

<details open>
<summary><b>🎣 Evil Portal — Captive Portal Attack</b></summary>

Sahte bir WiFi ağı kurarak bağlanan kullanıcıları login sayfasına yönlendirir.

```
┌─────────────────────────────┐
│        EVIL PORTAL          │
│      AP: Free Wifi          │
│                             │
│ → 172.0.0.1/creds → creds  │
│ → 172.0.0.1/ssid  → ssid   │
│                             │
│ Pwd mode: Full              │
│ ema: [kullanıcı adı]        │
│ pas: [şifre]                │
└─────────────────────────────┘
```

**Akış:**
```
Kurban → "Free Wifi"'a bağlanır
       → Otomatik captive portal sayfası açılır
       → Credentials girer
       → Cihaz ekranında görünür ✓
```

</details>

<details>
<summary><b>📡 Deauth Attack</b></summary>

WiFi istemcilerini ağdan düşürür. IEEE 802.11 yönetim frame zafiyetini kullanır.

```
Hedef AP → Deauth frame gönder → İstemci bağlantısı kesilir
```

</details>

<details>
<summary><b>📢 Beacon Spam</b></summary>

Çevrede onlarca sahte WiFi ağı adı yayınlar.

</details>

<details>
<summary><b>🗺 Wardriving</b></summary>

Çevredeki WiFi ağlarını tarar ve SD karta kaydeder.

```csv
SSID,BSSID,RSSI,Channel,Security
HomeNetwork,AA:BB:CC:DD:EE:FF,-65,6,WPA2
```

</details>

---

### 📡 Bluetooth / BLE

<details>
<summary><b>🔍 BLE Scanner</b></summary>

Çevredeki tüm BLE cihazları listeler: MAC adresi, sinyal gücü, cihaz adı.

</details>

<details>
<summary><b>📨 BLE Spam</b></summary>

Sahte BLE cihaz bildirimleri gönderir:

- 📱 Sahte iPhone / AirPods bağlantı isteği
- 📍 Sahte AirTag yakınlık uyarısı
- 🪟 Sahte Windows Swift Pair bildirimi
- 🎮 Sahte Nintendo Switch bildirimi

</details>

<details>
<summary><b>🐬 Flipper Zero Detector</b></summary>

Çevredeki Flipper Zero cihazlarını BLE üzerinden tespit eder.

</details>

---

### ⌨️ Bad USB (HID)

Cihazı USB klavye olarak taklit eder, Duckyscript çalıştırır.

```ducky
DELAY 1000
GUI r
DELAY 500
STRING cmd
ENTER
```

Script'ler SD karttan yüklenir: `/badusb/script.txt`

---

### 🌐 Web Arayüzü

| Endpoint | İşlev |
|---|---|
| `172.0.0.1/creds` | Yakalanan credential'ları görüntüle |
| `172.0.0.1/ssid` | Evil Portal SSID değiştir |

---

## 🗂 Menü Yapısı

```
Bruce
├── 📶 WiFi
│   ├── Evil Portal
│   ├── Deauth Attack
│   ├── Beacon Spam
│   ├── Wardriving
│   └── Packet Monitor
├── 📡 Bluetooth
│   ├── BLE Scanner
│   ├── BLE Spam
│   └── Flipper Detector
├── ⌨️  Bad USB
│   └── Script Runner (SD'den)
├── ⚙️  Settings
│   ├── WiFi Config
│   ├── Display Brightness
│   └── OTA Update
└── ℹ️  About
```

---

## 🛡️ Korunma Yöntemleri

| Saldırı | ✅ Korunma |
|---|---|
| Evil Portal | URL'e bak, HTTPS yoksa girme. VPN kullan. Tanımadığın ağa bağlanma. |
| Deauth | WPA3 destekli router al. Kritik işler için mobil data kullan. |
| BLE Spam | Kullanmadığında Bluetooth'u kapat. |
| Bad USB | Tanımadığın USB cihazını takma. |

---

## 🔗 Kaynaklar

[![Bruce GitHub](https://img.shields.io/badge/Bruce-GitHub-black?style=flat-square&logo=github)](https://github.com/BruceDevices/firmware)
[![Bruce Web](https://img.shields.io/badge/Bruce-Website-orange?style=flat-square)](https://bruce.computer)
[![CYD Community](https://img.shields.io/badge/CYD-Community-yellow?style=flat-square&logo=github)](https://github.com/witnessmenow/ESP32-Cheap-Yellow-Display)
[![Discord](https://img.shields.io/badge/Bruce-Discord-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.gg/bruce)

---

## ⚠️ Yasal Uyarı

<div align="center">

```
Bu proje yalnızca EĞİTİM AMAÇLIDIR.

✅  Kendi cihaz ve ağlarında kullanabilirsin
✅  Lab ortamında test edebilirsin
❌  İzinsiz ağlara saldırmak TCK 243-244 kapsamında suçtur
❌  Başkalarının credential'larını çalmak yasaktır
```

*Bu README'deki tüm testler kendi cihazlarımda, kendi ağımda yapılmıştır.*

</div>
