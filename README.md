# ESP32-C3 mini with 0.42-OLED
Verschiedene Anwendungen mit dem **ABrobot ESP32-C3 mini** Module mit **0.42"-OLED**

### 1) Daten
**ESP32-C3** (ESP32C3FH4), 4MB-Flash, WLAN, Bluetooth, Keramikantenne, 0,42"-OLED.  
Abmessung mit USB-C Buchse 27 x 20 mm.

<img width="609" height="330" alt="grafik" src="https://github.com/user-attachments/assets/981b1f32-de2a-4133-b237-8e4ff96f1e25" />  

von: https://emalliab.wordpress.com/2025/02/12/esp32-c3-0-42-oled/ (gute Beschreibung mit Beispielen)

**OLED**: SCL = `GPIO6`, SDA = `GPIO5`  
Spezielle Anschlüsse: ADC, IIC=I²C, SPI, Serielle Schnittstelle  

Die Schaltung sollte dem entsprechen: https://github.com/zhuhai-esp/ESP32-C3-ABrobot-OLED/blob/main/Document/ESP32C3%20OLED%E5%8E%9F%E7%90%86%E5%9B%BE.pdf

Das 0,42"-OLED ist ein Display mit 128x64 Pixel Buffer und **angezeigten 73 x 64 Pixel**.  
Der Pixel-Offset ist: **x+27** und **y+24** (SSD1306-Treiber)  
Mit dem **SSD1306.py** Treiber können 9 Zeichen á 8 Pixel Breite + 1 Pixel = 73 Pixel dargestellt werden.
Der Zeichensatz ist aus 8x8 Pixel Zeichen.  
Der Zeilenabstand = 10 ist optimal für diesen Treiber.  
**=> 4 Zeilen á 9 Zeichen**

### 2) MicroPython
FW https://micropython.org/resources/firmware/LOLIN_C3_MINI-20250415-v1.25.0.bin  
```
esptool -p COM11 erase_flash
esptool -p COM11 write_flash 0 LOLIN_C3_MINI-20250415-v1.25.0.bin
```
!!! COM11 entsprechend anpassen !!!

**MicroPython** meldet sich mit **115200 Baud**. Mit Thonny getestet und programmiert.  

**SSD1306.py** https://raw.githubusercontent.com/stlehmann/micropython-ssd1306/refs/heads/master/ssd1306.py

**main.py**
```
import machine
import ssd1306

i2c = machine.SoftI2C(scl=machine.Pin(6), sda=machine.Pin(5))

oled = ssd1306.SSD1306_I2C(128, 64, i2c)
oled.fill(0)
oled.text("Micro", 27, 24)
oled.text("   Python", 27, 34)
oled.text("123456789", 27, 44)
oled.text("M_._,_._._", 27, 56)
oled.show()
```
<img width="1280" height="1207" alt="grafik" src="https://github.com/user-attachments/assets/f4f256df-722f-4844-9a55-3372a8faacb7" />

29.7.2025 - OE3WAS

