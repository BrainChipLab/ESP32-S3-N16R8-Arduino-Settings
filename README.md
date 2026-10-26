# ESP32-S3 N16R8 (and N8R2) Arduino IDE Settings & Test Sketch

Complete `Tools` menu settings and hardware verification sketch for **ESP32-S3-WROOM-1-N16R8** (16 MB Quad Flash + 8 MB Octal PSRAM) and **N8R2** boards on `arduino-esp32` core 3.x.

---

### 1. Arduino IDE `Tools` Menu Settings (`N16R8` vs `N8R2`)

| Setting (`Tools` Menu) | **ESP32-S3 N16R8** | **ESP32-S3 N8R2** |
| :--- | :--- | :--- |
| **Board** | `ESP32S3 Dev Module` | `ESP32S3 Dev Module` |
| **Flash Size** | `16MB (128Mb)` | `8MB (64Mb)` |
| **Flash Mode** | `QIO 80MHz` *(do NOT pick OPI for flash)* | `QIO 80MHz` |
| **PSRAM** | `OPI PSRAM` *(Octal)* | `QSPI PSRAM` *(Quad)* |
| **Partition Scheme** | `16M Flash (3MB APP/9.9MB FATFS)` | `8M with spiffs (3MB APP/1.5MB SPIFFS)` |
| **USB CDC On Boot** | `Enabled` (on `USB` port) / `Disabled` (on `COM`/`UART` port) | `Enabled` (on `USB` port) / `Disabled` (on `COM`/`UART` port) |
| **USB Mode** | `Hardware CDC and JTAG` (default) | `Hardware CDC and JTAG` (default) |
| **Upload Mode** | `UART0 / Hardware CDC` (default) | `UART0 / Hardware CDC` (default) |

---

### 2. Flash & PSRAM Verification Sketch
Upload this sketch and open the Serial Monitor at `115200 baud` to verify your physical Flash chip and PSRAM configuration:

```cpp
void setup() {
  Serial.begin(115200);
  delay(2000);  // give the USB port time to connect
  Serial.print("Flash: "); Serial.println(ESP.getFlashChipSize());
  Serial.print("PSRAM: "); Serial.println(ESP.getPsramSize());
}

void loop() {}
```

**Expected Output on a Genuine `N16R8`:**
* `Flash: 16777216` (16 MB — read directly from the flash chip)
* `PSRAM: ~8388608` (close to 8 MB)
* *Troubleshooting:* If `PSRAM` prints `0` with `OPI PSRAM` selected, switch `Tools -> PSRAM` to `QSPI PSRAM` (your board may have an `R2` module instead of `R8`).

---

### 3. Important ESP32-S3 Pin & Port Notes
* **Manual Download Mode (if upload fails on native `USB` port):** Hold **BOOT**, tap **RESET**, then release **BOOT**. After uploading, press **RESET** once to start your sketch.
* **Reserved Octal PSRAM Pins (Do NOT use on `R8` boards):** `GPIO 35`, `GPIO 36`, `GPIO 37`
* **Native USB Pins:** `GPIO 19` (D-), `GPIO 20` (D+)
* **Strapping Pins:** `GPIO 0`, `GPIO 3`, `GPIO 45`, `GPIO 46`
* **Onboard RGB LED (`WS2812`):** `GPIO 48` (DevKitC-1 v1.0 & most clones) or `GPIO 38` (DevKitC-1 v1.1)
