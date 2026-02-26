# 🤖 QUBIT — The Desk Robot

> **Machines, memes, and magic.** Qubit is a dual-arm desktop robot powered by a Raspberry Pi, expressive LED eyes, and fully open-source Python control software. Built to demo, stream, and inspire.

<p align="center">
  <img src="docs/qubit-banner.png" alt="Qubit Desk Robot" width="100%" />
</p>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Hardware](#hardware)
  - [Bill of Materials — Per SO-100 Arm](#bill-of-materials--per-so-100-arm)
  - [Full System BOM](#full-system-bom)
  - [Wiring Diagram](#wiring-diagram)
- [3D Printing](#3d-printing)
- [Software Setup](#software-setup)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
- [Python Scripts](#python-scripts)
- [Emotes & LED Animations](#emotes--led-animations)
- [Project Structure](#project-structure)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**Qubit** is a fully functional dual-arm desk robot designed for robotics demonstrations, content creation, and educational use. It combines two SO-100 robotic arms, expressive 16×16 LED matrix eyes, ambient LED tube lighting, and a Raspberry Pi brain — all running on modular Python scripts.

**Key capabilities:**
- Bimanual manipulation with two independent SO-100 arms (12 servos total)
- Expressive face via 16×16 iDotMatrix LED display
- Ambient RGB lighting via dual LED tubes
- Fully scriptable behaviors and animations in Python
- Teleoperation support via leader/follower arm pairing
- Camera integration for computer vision tasks

---

## Hardware

### Bill of Materials — Per SO-100 Arm

> **Note:** Each Qubit uses **2× SO-100 arms**, so multiply arm quantities by 2. The SO-101 leader arm (optional, for teleoperation) requires 6× STS3215 servos in a specific mix: 3× C046 (1/147 gear), 2× C044 (1/191 gear), 1× C001 (1/345 gear) — available as a bundle on Alibaba.

| Part | Qty | 🇺🇸 USD | 🇺🇸 Buy | 🇪🇺 EUR | 🇪🇺 Buy | 🇨🇳 RMB | 🇨🇳 Buy |
|---|:---:|---:|---|---:|---|---:|---|
| STS3215 Servo (7.4V, 1/345 gear C001) | 6 | $14 | [Alibaba](https://www.alibaba.com/product-detail/Top-Seller-Low-Cost-Feetech-STS3215_1600999461525.html) | €13 | [Alibaba](https://www.alibaba.com/product-detail/Top-Seller-Low-Cost-Feetech-STS3215_1600999461525.html) | ￥97.72 | [TaoBao](https://item.taobao.com/item.htm?id=712179366565&skuId=5268252241438) |
| Motor Control Board | 1 | $11 | [Amazon](https://www.amazon.com/Waveshare-Integrates-Control-Circuit-Supports/dp/B0CTMM4LWK/) | €12 | [Amazon](https://www.amazon.fr/-/en/dp/B0CJ6TP3TP/) | ￥27 | [TaoBao](https://detail.tmall.com/item.htm?id=738817173460) |
| USB-C Cable 2-pack | 1 | $7 | [Amazon](https://www.amazon.com/Charging-etguuds-Charger-Braided-Compatible/dp/B0B8NWLLW2/) | €7 | [Amazon](https://www.amazon.fr/dp/B07BNF842T/) | ￥23.90 | [TaoBao](https://detail.tmall.com/item.htm?id=44425281296) |
| Power Supply (7.5V DC) | 1 | $10 | [Amazon](https://www.amazon.com/Facmogu-Switching-Transformer-Compatible-5-5x2-1mm/dp/B087LY41PV/) | €13 | [Amazon](https://www.amazon.fr/-/en/dp/B01HRR9GY4/) | ￥22.31 | [TaoBao](https://item.taobao.com/item.htm?id=544824248494) |
| Table Clamp 2-pack | 1 | $5 | [Amazon](https://www.amazon.com/Mr-Pen-Carpenter-Clamp-6inch/dp/B092L925J4/) | €8 | [Amazon](https://www.amazon.fr/-/en/dp/B08HZ1QRBF/) | ￥7.80 | [TaoBao](https://detail.tmall.com/item.htm?id=738636473238) |
| Screwdriver Set | 1 | $6 | [Amazon](https://www.amazon.com/Precision-Phillips-Screwdriver-Electronics-Computer/dp/B0DB227RTH) | €10 | [Amazon](https://www.amazon.fr/dp/B08ZXVMVYD/) | ￥14.90 | [TaoBao](https://detail.tmall.com/item.htm?id=675684600845) |
| **Total (per arm)** | | **$123** | | **€128** | | **￥682.23** | |
| **Total (2 arms)** | | **$246** | | **€256** | | **￥1,364.46** | |

> [1] STS3215 servos are Feetech serial bus servos. Verify gear ratio before ordering — the follower arm uses **all 1/345 (C001)**.  
> [2] Power supply must output **7.4–7.5V DC** with at least **5A** capacity per arm.  
> [3] A JIS screwdriver set is recommended for Japanese-market hardware if ordering in JP.

---

### Full System BOM

These are the additional components needed for the full Qubit build beyond the two arms.

| Component | Details | Est. Cost |
|---|---|---:|
| **Raspberry Pi 5** (4GB or 8GB) | Main compute brain | ~$60–$80 |
| **16×16 LED Matrix Display** | iDotMatrix or equivalent, Bluetooth/BLE controlled | ~$20–$35 |
| **LED Tubes** (×2) | WS2812B addressable RGB, 30–60cm | ~$15–$25 |
| **Bambu Lab A1 3D Printer** | For printing all structural chassis parts | ~$299 |
| **MicroSD Card** (32GB+) | For Raspberry Pi OS | ~$10 |
| **USB Hub** (powered, 4-port) | For connecting both arm control boards | ~$15 |
| **Pi Camera Module v3** (optional) | Computer vision / face tracking | ~$25 |
| **5V/5A USB-C Power Supply** | For Raspberry Pi 5 | ~$12 |

> **Total estimated full build cost (US):** ~$700–$800 depending on sourcing and arm count.

---

### Wiring Diagram

```
┌─────────────────────────────────────────────────┐
│                  RASPBERRY PI 5                 │
│                                                 │
│  USB-A ──────────► Motor Board (Left Arm)       │
│  USB-A ──────────► Motor Board (Right Arm)      │
│  USB-C (power) ◄── 5V/5A Supply                │
│  GPIO / BLE ─────► 16×16 LED Matrix             │
│  GPIO PWM ───────► LED Tube Left (WS2812B)      │
│  GPIO PWM ───────► LED Tube Right (WS2812B)     │
│  CSI / USB ──────► Pi Camera (optional)         │
└─────────────────────────────────────────────────┘

Motor Board (×2, one per arm)
  └── USB-C ◄── 7.5V DC Power Supply (per arm)
  └── Serial bus ──► 6× STS3215 Servos (daisy-chained)
```

> ⚠️ **Do not power servo boards from the Raspberry Pi USB.** Always use a dedicated 7.4–7.5V supply per arm to avoid brownouts and servo damage.

---

## 3D Printing

All structural parts are printed on a **Bambu Lab A1** using PLA or PETG.

**Recommended Print Settings:**
- Material: PLA+ or PETG
- Layer height: 0.2mm
- Infill: 40% (Gyroid recommended for strength)
- Supports: Yes, for arm joint brackets
- Print time per arm: ~8–12 hours total across all parts

All STL files are located in the [`/stl`](./stl) folder. See the table below:

| File | Description | Material | Qty |
|---|---|---|:---:|
| `qubit_head_shell.stl` | Main head enclosure housing LED matrix | PLA+ | 1 |
| `qubit_head_back.stl` | Rear panel with cable routing slots | PLA+ | 1 |
| `qubit_neck_bracket.stl` | Neck-to-body mounting bracket | PETG | 1 |
| `qubit_body_frame.stl` | Central torso and electronics mount | PETG | 1 |
| `qubit_led_tube_mount_left.stl` | Left LED tube arm mount | PLA+ | 1 |
| `qubit_led_tube_mount_right.stl` | Right LED tube arm mount | PLA+ | 1 |
| `qubit_arm_shoulder_left.stl` | Left SO-100 shoulder attachment | PETG | 1 |
| `qubit_arm_shoulder_right.stl` | Right SO-100 shoulder attachment | PETG | 1 |
| `qubit_base_plate.stl` | Weighted desk base | PETG | 1 |
| `qubit_cable_cover.stl` | Rear cable management cover | PLA+ | 1 |

> 💡 **Tip:** Print the body frame and shoulder brackets in PETG for added durability under servo torque. PLA+ is fine for cosmetic shell pieces.

---

## Software Setup

### Prerequisites

- Raspberry Pi OS (Bookworm 64-bit) — [Download](https://www.raspberrypi.com/software/)
- Python 3.10+
- Git

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/0xaiwhisperer/qubit.git
cd qubit

# 2. Create a virtual environment
python3 -m venv .venv
source .venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. (Optional) Install LeRobot for teleoperation support
pip install lerobot
```

### Configuration

Copy the example config and edit it to match your hardware setup:

```bash
cp config/config.example.yaml config/config.yaml
nano config/config.yaml
```

**Key config options:**

```yaml
# config/config.yaml

arms:
  left:
    port: /dev/ttyUSB0       # Motor board serial port for left arm
    servo_ids: [1, 2, 3, 4, 5, 6]
  right:
    port: /dev/ttyUSB1       # Motor board serial port for right arm
    servo_ids: [7, 8, 9, 10, 11, 12]

led_matrix:
  type: idotmatrix            # Display type
  size: 16                    # 16x16
  connection: bluetooth       # bluetooth or serial
  mac_address: "AA:BB:CC:DD:EE:FF"  # Replace with your device MAC

led_tubes:
  left_pin: 18               # GPIO BCM pin for left tube
  right_pin: 19              # GPIO BCM pin for right tube
  num_pixels: 30             # LEDs per tube

camera:
  enabled: false
  device: 0                  # /dev/video0
  resolution: [640, 480]
```

To find your motor board serial ports:

```bash
ls /dev/ttyUSB*
# Plug/unplug each arm to identify which port maps to which arm
```

---

## Python Scripts

All scripts live in the root `/scripts` directory and are run from the project root.

| Script | Description | Usage |
|---|---|---|
| `scripts/run_arm.py` | Manual single-arm servo control via keyboard or gamepad | `python scripts/run_arm.py --arm left` |
| `scripts/teleop.py` | Teleoperation — mirror leader arm to follower arm | `python scripts/teleop.py` |
| `scripts/record_demo.py` | Record an arm movement sequence to JSON | `python scripts/record_demo.py --arm left --output demos/wave.json` |
| `scripts/play_demo.py` | Replay a recorded movement sequence | `python scripts/play_demo.py --file demos/wave.json` |
| `scripts/calibrate.py` | Interactive servo calibration wizard | `python scripts/calibrate.py --arm left` |
| `scripts/led_matrix.py` | Push an emote or animation to the LED matrix | `python scripts/led_matrix.py --emote happy` |
| `scripts/led_tubes.py` | Control LED tube lighting effects | `python scripts/led_tubes.py --mode pulse --color 00f5c4` |
| `scripts/face_track.py` | Enable face tracking with camera + arm movement | `python scripts/face_track.py` |
| `scripts/idle_behavior.py` | Run ambient idle animations on arms + LEDs | `python scripts/idle_behavior.py` |
| `scripts/diagnostics.py` | Check servo health, temperatures, and load | `python scripts/diagnostics.py --arm all` |
| `scripts/dashboard.py` | Launch the local web control dashboard | `python scripts/dashboard.py` |

### Example Workflows

**Run idle loop on startup:**
```bash
# Add to /etc/rc.local or systemd service
python /home/pi/qubit/scripts/idle_behavior.py &
```

**Record and replay a wave:**
```bash
python scripts/record_demo.py --arm right --output demos/wave.json
# Move the arm manually while recording, then Ctrl+C to stop
python scripts/play_demo.py --file demos/wave.json --loop 3
```

**Launch the web dashboard:**
```bash
python scripts/dashboard.py
# Open http://qubit.local:5000 in a browser
```

---

## Emotes & LED Animations

All emote files live in the [`/emotes`](./emotes) folder. Each emote is a JSON file describing a 16×16 pixel frame or sequence of frames to display on the LED matrix.

### Emote Format

```json
{
  "name": "happy",
  "fps": 8,
  "loop": true,
  "frames": [
    {
      "pixels": [
        [0,0,0, ...],  // Row 0: 16 RGB values
        [0,0,0, ...],  // Row 1
        ...            // Rows 2–15
      ]
    }
  ]
}
```

### Included Emotes

| File | Expression | Animated |
|---|---|:---:|
| `emotes/happy.json` | 😊 Happy eyes | ✅ |
| `emotes/blink.json` | 😐 Slow blink | ✅ |
| `emotes/sad.json` | 😢 Sad eyes | ✅ |
| `emotes/angry.json` | 😠 Angry brow | ✅ |
| `emotes/surprised.json` | 😲 Wide eyes | ✅ |
| `emotes/wink.json` | 😉 Left eye wink | ✅ |
| `emotes/sleep.json` | 😴 Closed eyes + zzz | ✅ |
| `emotes/loading.json` | 🔄 Spinning loader | ✅ |
| `emotes/startup.json` | 💡 Boot sequence flash | ✅ |
| `emotes/eye_track_left.json` | 👀 Eyes shift left | ✅ |
| `emotes/eye_track_right.json` | 👀 Eyes shift right | ✅ |
| `emotes/heart.json` | ❤️ Heart pulse | ✅ |
| `emotes/glitch.json` | ⚡ Glitch effect | ✅ |
| `emotes/off.json` | ⬛ All pixels off | ❌ |

### Playing an Emote

```bash
# Play a single emote
python scripts/led_matrix.py --emote happy

# Play an emote sequence
python scripts/led_matrix.py --sequence startup,happy,blink

# Loop an emote indefinitely
python scripts/led_matrix.py --emote sleep --loop
```

### Creating Custom Emotes

Use the built-in emote editor in the web dashboard, or create JSON files manually following the format above. The `tools/emote_preview.py` script lets you preview emotes in terminal before uploading.

---

## Project Structure

```
qubit/
│
├── README.md                    # This file
├── requirements.txt             # Python dependencies
├── config/
│   ├── config.example.yaml      # Template config — copy to config.yaml
│   └── config.yaml              # Your local config (git-ignored)
│
├── scripts/                     # All runnable Python scripts
│   ├── run_arm.py
│   ├── teleop.py
│   ├── record_demo.py
│   ├── play_demo.py
│   ├── calibrate.py
│   ├── led_matrix.py
│   ├── led_tubes.py
│   ├── face_track.py
│   ├── idle_behavior.py
│   ├── diagnostics.py
│   └── dashboard.py
│
├── emotes/                      # LED matrix emote animations (JSON)
│   ├── happy.json
│   ├── blink.json
│   ├── sad.json
│   ├── angry.json
│   ├── surprised.json
│   ├── wink.json
│   ├── sleep.json
│   ├── loading.json
│   ├── startup.json
│   ├── eye_track_left.json
│   ├── eye_track_right.json
│   ├── heart.json
│   ├── glitch.json
│   └── off.json
│
├── stl/                         # 3D printable parts (Bambu Lab A1)
│   ├── qubit_head_shell.stl
│   ├── qubit_head_back.stl
│   ├── qubit_neck_bracket.stl
│   ├── qubit_body_frame.stl
│   ├── qubit_led_tube_mount_left.stl
│   ├── qubit_led_tube_mount_right.stl
│   ├── qubit_arm_shoulder_left.stl
│   ├── qubit_arm_shoulder_right.stl
│   ├── qubit_base_plate.stl
│   └── qubit_cable_cover.stl
│
├── demos/                       # Saved arm movement sequences (JSON)
│   ├── wave.json
│   └── idle_sway.json
│
├── qubit/                       # Core Python library
│   ├── __init__.py
│   ├── arm.py                   # SO-100 arm control class
│   ├── servo.py                 # STS3215 serial bus servo driver
│   ├── led_matrix.py            # iDotMatrix LED display driver
│   ├── led_tubes.py             # WS2812B LED tube driver (rpi_ws281x)
│   ├── camera.py                # Camera and CV utilities
│   └── utils.py                 # Shared helpers
│
├── tools/                       # Developer utilities
│   ├── emote_preview.py         # Terminal preview for emote JSON files
│   └── scan_servos.py           # Scan and identify connected servo IDs
│
└── docs/                        # Documentation assets
    ├── wiring_diagram.png
    └── qubit-banner.png
```

---

## Troubleshooting

**Servos not responding:**
- Check USB serial port assignment with `ls /dev/ttyUSB*`
- Verify power supply is 7.4–7.5V and has sufficient current (5A min per arm)
- Run `python scripts/diagnostics.py --arm all` to check servo status
- Ensure servo IDs are set correctly — run `python tools/scan_servos.py`

**LED matrix not connecting:**
- Confirm Bluetooth is enabled on the Pi: `sudo systemctl enable bluetooth`
- Pair the matrix first: `bluetoothctl` → `scan on` → `pair <MAC>`
- Double-check the MAC address in `config.yaml`

**Arm moving to wrong position:**
- Re-run calibration: `python scripts/calibrate.py --arm left`
- Check for mechanical binding in joints (common with first-print tolerances)

**Camera/face tracking not working:**
- Install OpenCV: `pip install opencv-python-headless`
- Enable camera in Pi config: `sudo raspi-config` → Interface Options → Camera

**Low FPS in teleoperation:**
- Reduce servo polling rate in `config.yaml`: `teleop.hz: 50`
- Ensure you're using Python 3.10+ and running from a virtual environment

---

## Contributing

Pull requests welcome. For major changes, open an issue first to discuss what you'd like to change.

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/new-emote`
3. Commit your changes
4. Push and open a PR

Custom emotes especially encouraged — drop your `.json` files in `/emotes` and submit a PR!

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

<p align="center">
  Built by <a href="https://twitter.com/0xaiwhisperer">@0xaiwhisperer</a> · The A.I. Whisperer · <em>machines, memes, and magic</em>
</p>
