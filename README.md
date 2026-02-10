# Force / Pressure Sensor with Arduino Uno R4 WiFi

A simple PlatformIO Arduino project that reads a force-sensitive resistor (FSR) attached to an Arduino Uno R4 WiFi and prints analog readings to the serial monitor.

This repository contains a minimal example (src/main.cpp) that samples the FSR on analog pin A0 and writes lines like:

```
Analog reading = 512
```

Values range from 0 (no pressure) to ~1023 (maximum pressure) on the Uno R4 ADC.

---

## Hardware Required

- Arduino Uno R4 WiFi (or compatible Renesas RA-based Uno R4 board)
- Force Sensitive Resistor (FSR) or similar analog pressure sensor
- 10 kΩ resistor (used as pulldown)
- Breadboard and jumper wires
- USB-C (or appropriate) cable to connect the Uno R4 WiFi to your PC

Optional:
- Multimeter to verify wiring

## Wiring / Pin Mapping

This project expects the FSR wired as a simple voltage divider with a 10 kΩ pulldown resistor. Example wiring (text diagram):

- FSR one end -> +5V
- FSR other end -> A0 (analog input)
- A0 -> 10 kΩ resistor -> GND (pulldown)
- GND -> Arduino GND

ASCII wiring diagram (breadboard view):

```
+5V ---[ FSR ]--- A0 ---[10kΩ]--- GND
                     |
                    (Analog input to Arduino A0)
```

Notes:
- If you are using a different analog pin, update `fsrPin` inside `src/main.cpp` accordingly.
- If using a 3.3V sensor or running the board at 3.3V, wire the sensor to 3.3V instead of 5V and be aware the ADC range will be relative to 3.3V.

## Software Prerequisites

- PlatformIO (CLI or the PlatformIO extension for VS Code / CLion)
- Python (required by PlatformIO)
- Git (to clone the repo)
- IDE (Visual Studio Code or CLion recommended)

## Quickstart (PowerShell)

1. Clone the repository and open it in your editor:

```powershell
git clone <REPOSITORY_URL>
cd force-pressure-sensor-integrated-with-arduino-uno-r4-wifi
code .    # or open the folder in CLion
```

2. Build the project for the included PlatformIO environment (`uno_r4_wifi`):

```powershell
pio run -e uno_r4_wifi
```

3. Upload to the board (make sure the board is connected to your PC and the correct COM port is selected in `platformio.ini` or set in PlatformIO):

```powershell
pio run -e uno_r4_wifi -t upload
```

4. Open the serial monitor (PlatformIO will use settings from `platformio.ini` by default):

```powershell
pio device monitor -e uno_r4_wifi
```

Or explicitly specify port and baud:

```powershell
pio device monitor -p COM5 -b 9600
```

The example `platformio.ini` in this repo defines the environment `uno_r4_wifi` and sets `monitor_speed = 9600` and `monitor_port = COM5` by default.

## Example Output

Once the board is running and the sensor is attached you should see repeated lines like:

```
Analog reading = 73
Analog reading = 88
Analog reading = 120
```

The numeric value is the raw ADC reading (0–1023). Higher values indicate more force on the sensor.

## Configuration Tips

- To change the serial port or baud rate, edit `platformio.ini`:

```ini
[env:uno_r4_wifi]
monitor_speed = 9600
monitor_port = COM5
```

- To change the analog input pin, update `fsrPin` in `src/main.cpp` (currently `A0`).
- If uploading fails, double-check the selected board (`board = uno_r4_wifi`) and ensure any required drivers are installed on Windows.

## Troubleshooting

- No serial output:
  - Verify the board is powered and the USB cable works.
  - Confirm the correct COM port in Device Manager and update `platformio.ini` or the monitor command.
  - Make sure `Serial.begin(9600);` matches the monitor baud rate.

- Reading is always 0 or constant:
  - Check the wiring and the pulldown resistor (10 kΩ) is connected correctly.
  - Verify the FSR is functional and connected to the correct analog pin.

- Upload fails or device not found:
  - Confirm the board is set to `uno_r4_wifi` in `platformio.ini` and the board is in a state ready to receive uploads.
  - Restart your IDE/PlatformIO or reconnect the USB cable.

## Contributing

Contributions are welcome. A simple workflow:

1. Fork the repo
2. Create a feature branch
3. Open a pull request describing your changes

If you'd like, add a `docs/wiring.png` or an improved diagram for common sensors.

## License

This project is provided under the MIT License. See the `LICENSE` file for details (or add one if you prefer a different license).

---

If you'd like, I can also:
- add a `LICENSE` file (MIT) and a `docs/wiring.png` example image,
- update `src/main.cpp` to add filtered/averaged readings or units conversion,
- or open the new `README.md` in the editor for review.

Tell me which of these (if any) you'd like me to do next.
