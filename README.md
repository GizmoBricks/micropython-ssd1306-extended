# MicroPython SSD1306 OLED driver (I2C and SPI)

A MicroPython driver for SSD1306 OLED displays supporting both I2C and SPI interfaces.
This is a fork of adafruit/micropython-adafruit-ssd1306 with additional drawing methods.

---

## Features

- Display support: SSD1306 OLED modules (commonly 128x64 and 128x32).
- Interfaces: I2C and SPI implementations.
- Framebuffer API: Fast drawing via MicroPython’s framebuf module.
- Extended drawing: Added `line`, `hline`, `vline`, `rect`, `ellipse`, `poly` methods for richer graphics.
- Utilities: `text`, `scroll`, `invert`, `contrast`, and full-screen `fill`.

---

# Installation

- Copy file: Place the driver file (e.g., `ssd1306.py`) on your MicroPython board (ESP8266, ESP32, PyBoard).
- Dependencies: Ensure the built-in MicroPython module framebuf is available.
- Import: Use SSD1306_I2C or SSD1306_SPI classes depending on your wiring.

---

## Usage

### I2C example
```python
from machine import Pin, I2C
from ssd1306 import SSD1306_I2C

# Adjust bus ID and pins for your board
i2c = I2C(0, scl=Pin(5), sda=Pin(4))
oled = SSD1306_I2C(128, 64, i2c)  # width, height

oled.text("Hello, I2C!", 0, 0)
oled.show()
```

### SPI example

```python
from machine import Pin, SPI
from ssd1306 import SSD1306_SPI

spi = SPI(1)           # adjust bus
dc = Pin(2)            # data/command
res = Pin(16)          # reset
cs = Pin(0)            # chip select

oled = SSD1306_SPI(128, 64, spi, dc, res, cs)

oled.text("Hello, SPI!", 0, 0)
oled.show()
```

---

## Methods in this library

These methods wrap or extend MicroPython’s [`framebuf` API](https://docs.micropython.org/en/latest/library/framebuf.html) for use with SSD1306 displays.

### Display control
- `show()`  
  Flush the framebuffer to the OLED display. Handles column shift for 64‑pixel wide displays.
  
- `poweroff()`  
  Turn the display off.
- `poweron()`  
  (SPI only) Reset and turn the display on.
- `contrast(level)`  
  Set display brightness (0–255).
- `invert(invert)`  
  Invert display colors (`True` or `False`).
- `fill(col)`  
  Fill the entire screen with color `c` (0=black, 1=white).

### Drawing primitives
- `pixel(x, y, col=1)`  
  Set or get a single pixel at `(x, y)`.
  
- `line(x1, y1, x2, y2, col=1)`  
  Draw a line from `(x1, y1)` to `(x2, y2)`.
- `hline(x, y, w, col=1)`  
  Draw a horizontal line starting at `(x, y)` with width `w`.
- `vline(x, y, h, col=1)`  
  Draw a vertical line starting at `(x, y)` with height `h`.
- `rect(x, y, w, h, col=1, fill=False)`  
  Draw a rectangle outline or filled if `fill=True`.
- `ellipse(x, y, xr, yr, col=1, fill=False, quadrant=15)`  
  Draw an ellipse centered at `(x, y)` with radii `xr`, `yr`.  
  `quadrant` is a bitmask:  
  - 1  = Q1
  - 2  = Q2
  - 4  = Q3
  - 8  = Q4  
  - 3  = Q1+Q2
  - 6  = Q2+Q3
  - 12 = Q3+Q4
  - 9  = Q4+Q1
  - 15 = Q1+Q2+Q3+Q4
- `poly(x, y, coords, col=1, fill=False)`  
  Draw a polygon with vertices defined in `coords` (array of integers).  
  Example: `array('h', [x0, y0, x1, y1, ...])`.

### Text and scrolling
- `text(s, x, y, col=1)`  
  Draw text string `s` at position `(x, y)` using built‑in 8×8 font.
  
- `scroll(dx, dy)`  
  Shift the framebuffer contents by `dx` horizontally and `dy` vertically.

---

## Notes
- All drawing methods operate on the framebuffer; call `show()` to update the OLED.
- Colors are monochrome: `0` = black, `1` = white or your specific display color.
- `SSD1306_I2C` and `SSD1306_SPI` subclasses handle communication specifics; drawing API is identical.

---

## License

Upstream: Based on Adafruit’s MicroPython SSD1306 driver.

This fork uses the same MIT License as the original project.

---

### Changelog

Added new drawing methods: `line`, `hline`, `vline`, `rect`, `ellipse`, `poly`.
