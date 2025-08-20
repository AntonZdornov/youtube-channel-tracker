# ESP32‑C6 + LVGL: YouTube Channel Stats Display

This project uses an **ESP32‑C6** with a color display to show **YouTube channel statistics** (views, subscribers, etc.) using the **LVGL** graphics library. Wi‑Fi configuration is done via **Bluetooth (BLE)**. After setup, the client queries the **Google YouTube Data API** every minute with an API key and displays the updated statistics on screen.

---

## Features

* 📺 Displays YouTube channel metrics: views, subscribers, videos, etc.
* 🧩 User interface built with **LVGL** on a color TFT/LCD screen.
* 📶 Wi‑Fi setup via **Bluetooth provisioning** (no hardcoding credentials).
* 🔁 Automatic update every **60 seconds** (configurable).
* ♻️ Caches the last response for offline fallback.

---

## Hardware

* **MCU:** ESP32‑C6 (RISC‑V, Wi‑Fi 6, BLE 5.0)
* **Display:** Color TFT/LCD compatible with LVGL (e.g., ST7789 240×240 or ILI9341 320×240)
* **Power:** 5V USB or Li‑ion battery with power board
* **Optional:** Buttons for manual refresh/screen switching

### Example pinout (ST7789 240×240, SPI)

| Signal | ESP32‑C6 | Display  |
| ------ | -------: | -------- |
| VCC    |   5V/3V3 | VCC      |
| GND    |      GND | GND      |
| SCK    |     IO06 | SCL/SCK  |
| MOSI   |     IO07 | SDA/MOSI |
| DC     |     IO02 | DC       |
| RST    |     IO03 | RST      |
| CS     |     IO10 | CS       |
| BL     |     IO11 | LED/BL   |

---

## Software & Libraries

* **Arduino / PlatformIO**
* **LVGL** (v8.x)
* Display driver (e.g., **TFT\_eSPI** or LVGL HAL)
* **NimBLE-Arduino** (or ESP‑IDF BLE) for provisioning
* **ArduinoJSON** for parsing API responses
* **WiFi** / **HTTPClient**

---

## How It Works

1. **First boot**: ESP32‑C6 runs a BLE service for provisioning. A mobile app sends Wi‑Fi credentials and API settings.
2. **Wi‑Fi connection**: Credentials are stored in NVS and the device connects.
3. **API request**: Every minute the ESP32‑C6 queries YouTube Data API with API key & channel ID.
4. **Display update**: LVGL widgets update with new stats (views, subs, etc.).
5. **Error handling**: Retries with exponential backoff; cached data shown if no connection.

---

## YouTube Data API Setup

1. Go to \[Google Cloud Console] and create a project.
2. Enable **YouTube Data API v3**.
3. Generate an **API key**.
4. Find your **Channel ID** (in YouTube URL or via API).

**Example request:**

```
GET https://www.googleapis.com/youtube/v3/channels?part=statistics&id=<CHANNEL_ID>&key=<API_KEY>
```


### Arduino IDE

* Install **ESP32 boards** package (ESP32‑C6).
* Install libraries: LVGL, TFT\_eSPI, NimBLE‑Arduino, ArduinoJSON.
* Configure display in `lv_conf.h` / `User_Setup.h`.
* Flash sketch.
