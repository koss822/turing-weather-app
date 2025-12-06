# Weather App Smart Screen

### ⚠️ DISCLAIMER

This project is **a community fork inspired by the Turing Smart Screen project**. It is **not affiliated, associated, authorized, endorsed by, or in any way officially connected with the Turing brand**, or any of its subsidiaries, affiliates, manufacturers, or sellers of Turing products.
All names and trademarks belong to their respective owners.

***

## Overview

This project is a **Python-based weather display application** for the **Turing Smart Screen hardware**, using data fetched from a **weather API** ([openweathermap.org)](https://openweathermap.org).
It retains compatibility with the Turing Smart Screen architecture while replacing system monitoring features with real-time weather displays.

You can use this as a **standalone weather dashboard** or embed it as part of larger projects that use the screen as a micro display.

The original codebase and display hardware interface come from the open-source [turing-smart-screen-python](https://github.com/mathoudebine/turing-smart-screen-python) project, adapted here for weather visualization and simpler operation.

<img src="res/docs/weather.jpg"/> 

<img src="res/docs/aliexpress.png"/> 

***

## Features

- Displays live weather data from a configurable weather API.
- Customizable refresh interval and display brightness through `config.yaml`.
- Supports all display revisions: **A**, **B**, **flagship**, and **simulation (SIMU)** mode.
- Fully cross-platform: works on **Windows**, **Linux**, **macOS**, and **Raspberry Pi**.
- Auto-detects COM port connection (“AUTO” mode).
- Easy configuration – no modifications to source code are required.
- Compatible with **3.5" 320×480 IPS USB-C (UART)** touch display.

***

## Installation

1. Clone or download this repository:

```bash
git clone git@github.com:koss822/turing-weather-app.git
cd turing-weather-app
```

2. Install the dependencies:

```bash
pip install -r requirements.txt
```

3. Configure your settings in `config.yaml`:

```yaml
screen_brightness: 50
api_key: "your-weather-api-key"
hr24: true
degc: true
d_weather: 5
COM_PORT: "AUTO"
REVISION: "A"
```

## Raspberry Pi 4 Installation (Ubuntu)
1. Installation

```bash
git clone https://github.com/koss822/turing-weather-app.git
cd turing-weather-app/
sudo apt install libdrm-dev libdrm-amdgpu1 libdrm-common pkg-config python3-dev python3 build-essential
python3 -m venv .
. bin/activate
python3 -m pip install -r requirements.txt
python3 -m pip install geocoder
sudo dmesg | grep usbmonitor -i -A5 -B5
sudo vi /etc/udev/rules.d/99-usb-turing.rules
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo vi /etc/systemd/system/turing-weather-app.service
sudo systemctl daemon-reload
sudo systemctl enable --now turing-weather-app.service
```

2. 99-usb-turing.rules (change YOUR_USER)
```
# Turing UsbMonitor (VID:1a86 PID:5722) -> /dev/ttyACM-Turing
KERNEL=="ttyACM*", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="5722", ATTRS{serial}=="USB35INCHIPSV2", \
SYMLINK+="ttyACM-Turing", OWNER="YOUR_USER", GROUP="dialout", MODE="0666"
```

3. /etc/systemd/system/turing-weather-app.service (change YOUR_USER)
```
[Unit]
Description=Turing Weather App
After=network.target
Wants=network.target

[Service]
Type=simple
User=YOUR_USER
Group=YOUR_USER
WorkingDirectory=/home/YOUR_USER/turing-weather-app
Environment=PATH=/home/YOUR_USER/turing-weather-app/bin
ExecStart=/home/YOUR_USER/turing-weather-app/bin/python weather-app.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

4. Configure your settings in `config.yaml`:

```yaml
...
COM_PORT: "/dev/ttyACM-Turing"
...
```


***

## Usage

To start the app, simply run:

```bash
python weather-app.pyw
```

The screen will initialize automatically, connect to the weather API, and begin updating weather information at the configured interval.

If you do not have a connected display, use **SIMU mode** (set `REVISION: "SIMU"`) to simulate the output on a local image file (`screencap.png`).

***

## Configuration

All application settings are centralized in `config.yaml`.
You can adjust:

- **screen_brightness** — display brightness (1–100)
- **api_key** — your weather API key
- **hr24** — use 24-hour format (true/false)
- **degc** — use Celsius units (true) or Fahrenheit (false)
- **d_weather** — update interval in minutes
- **COM_PORT** — serial port name or `"AUTO"`
- **REVISION** — hardware revision ("A", "B", "SIMU", or "flagship")

***

## Credits

This project is a **fork** of the open-source [turing-smart-screen-python](https://github.com/mathoudebine/turing-smart-screen-python) by **mathoudebine**, modified to display weather data using modern weather APIs.

