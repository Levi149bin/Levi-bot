# 🤖 LEVIBOT - Project Bible
### By Levii | AI Desk Buddy Robot

---

## 📋 Project Overview
Levibot is an AI-powered desk buddy robot that:
- Unlocks via RFID card
- Responds to voice and interacts using Gemini AI
- Connects to WiFi for time, date, and AI capabilities
- Shows emotions on OLED display
- Tracks hands and avoids obstacles
- Roams autonomously or via web control
- Has personality — gets bored, sleepy, excited!

---

## ✅ Components

### Already Owned:
| Component | Details |
|---|---|
| ESP32 | Konductor WROOM-32, CP2102, 30-pin, 240MHz |
| ESP32 Expansion Board | 30-pin, USB-C port |
| OLED Display | SSD1306 0.96 inch I2C |
| Servo Motor | 1x SG90 (currently owned) |
| L298N Motor Driver | For BO motors |
| Arduino Uno | For motor control |
| Li-ion Batteries | 2x 3.7V (connect in series = 7.4V) |

### Need To Buy:
| Component | Purpose | Est. Cost |
|---|---|---|
| RC522 RFID + cards | Door unlock | ₹83 |
| 2WD Chassis + BO motors | Movement base | ₹350 |
| 2x Caster wheels | Balance | ₹50 |
| SG90 Servo x2 | Pan-tilt head | ₹120 |
| HC-SR04 x2 | Hand tracking + obstacle avoid | ₹80 |
| IR sensor x2 | Table edge detection | ₹40 |
| TTP223 Touch sensor x2 | Head pat + side touch | ₹60 |
| INMP441 Mic | Voice input | ₹150-200 |
| MAX98357 Amp | Audio output | ₹200-250 |
| Small speaker 3W 4ohm | Sound output | ₹50-80 |
| Passive Buzzer | Notifications | ₹20 |
| 18650 battery holder | Power | ₹30 |

**Total Estimated Remaining: ₹800-900**

---

## 🔌 Pin Assignments (ESP32)

### OLED Display (I2C):
```
VCC → 3.3V
GND → GND
SDA → GPIO 21
SCL → GPIO 22
```

### RFID RC522 (SPI):
```
VCC  → 3.3V
GND  → GND
RST  → GPIO 4
SDA  → GPIO 5
MOSI → GPIO 23
MISO → GPIO 19
SCK  → GPIO 18
IRQ  → Not connected
```

### INMP441 Microphone (I2S):
```
VDD → 3.3V
GND → GND
SD  → GPIO 32
SCK → GPIO 33
WS  → GPIO 25
L/R → GND
```

### MAX98357 Amplifier (I2S):
```
VIN  → VIN (5V)
GND  → GND
DIN  → GPIO 26
BCLK → GPIO 27
LRC  → GPIO 14
```

### Servos (Pan-Tilt Head):
```
Pan Servo  → GPIO 13
Tilt Servo → GPIO 12
```

### HC-SR04 Ultrasonic Sensors:
```
Sensor 1 (Fixed front - obstacle):
TRIG → GPIO 16
ECHO → GPIO 17

Sensor 2 (On head - hand tracking):
TRIG → GPIO 1 (TX) -- TBD
ECHO → GPIO 3 (RX) -- TBD
```

### IR Sensors (Table edge):
```
IR Front → GPIO 36
IR Back  → GPIO 39
```

### Touch Sensors (TTP223):
```
Head touch → GPIO 15
Side touch → GPIO 2
```

### Button (Time display):
```
Button → GPIO 34
Button → GND
```

### Arduino Uno Communication:
```
ESP32 TX2 (GPIO 17) → Arduino Uno RX
ESP32 RX2 (GPIO 16) → Arduino Uno TX
GND → GND (common)
```

---

## 🔌 Pin Assignments (Arduino Uno)

### L298N Motor Driver:
```
IN1 → Pin 2
IN2 → Pin 3
IN3 → Pin 4
IN4 → Pin 5
ENA → Pin 9 (PWM)
ENB → Pin 10 (PWM)
```

### IR Sensors (Table edge):
```
IR Front → Pin 6
IR Back  → Pin 7
```

### ESP32 Communication:
```
RX → ESP32 TX2
TX → ESP32 RX2
GND → GND (common)
```

---

## ⚡ Power Setup:
```
2x 3.7V Li-ion in SERIES = 7.4V
7.4V → L298N motor driver (motors)
7.4V → ESP32 VIN via buck converter (5V)
3.3V → All sensors (from ESP32 3V3 pin)
```

---

## 🧠 Architecture:
```
ESP32 (Master Brain)
  │
  ├── OLED display (emotions/time/info)
  ├── Pan-tilt servos (head movement)
  ├── RFID RC522 (unlock)
  ├── Touch sensors x2 (interaction)
  ├── INMP441 mic (voice input)
  ├── MAX98357 speaker (voice output)
  ├── HC-SR04 x2 (hand track + obstacle)
  ├── IR sensors x2 (table edge)
  ├── WiFi → Gemini AI
  ├── Web server → phone control
  │
  └── Serial → Arduino Uno
                    │
                    ├── L298N motor driver
                    ├── BO motors x2
                    └── Caster wheels x2
```

---

## 🤖 Features:

### 1. RFID Unlock:
```
Power on → waits for RFID scan
Wrong card → angry face + buzzer
Right card → happy face + welcome message
```

### 2. Voice AI:
```
Wake word detected
→ INMP441 captures voice
→ Google Speech to Text
→ Send to Gemini AI
→ Reply via MAX98357 speaker
→ Emotion shown on OLED
```

### 3. Touch Interactions:
```
Pat on head   → happy face + wiggle
Side touch    → shows time/date/info
Double tap    → excited face
Long press    → settings
```

### 4. Emotions (Time Based):
```
6AM - 9AM  → Sleepy 😴
9AM - 6PM  → Happy 😊
6PM - 9PM  → Excited 🤩
9PM+       → Sleepy 😴
```

### 5. Hand Tracking:
```
HC-SR04 on pan-tilt head
→ detects hand distance
→ pan servo follows hand
→ OLED eyes follow direction
→ too close → surprised face
```

### 6. Obstacle Avoidance:
```
HC-SR04 fixed on front
→ detects obstacle < 20cm
→ stops + turns away
→ ESP32 shows worried face
```

### 7. Table Edge Protection:
```
IR sensors on bottom
→ detects table edge
→ Arduino stops motors immediately
→ reverses away
→ ESP32 shows scared face
```

### 8. Idle/Bored Mode:
```
No interaction for 10 mins
→ random bored movements
→ spin + wiggle + jerk
2 more mins no interaction
→ head droops (tilt servo)
→ sleepy face 😴
Touch/voice/RFID → wakes up!
```

### 9. Web Control Panel:
```
Phone browser → ESP32 IP address
Control panel:
[⬆️ Forward]
[⬅️ Left][⏹️ Stop][➡️ Right]
[⬇️ Backward]
[🔄 Spin][🎉 Wiggle]
Mode: [Auto 🤖][Manual 🎮]
```

### 10. Movement Sequences:
```
FORWARD  → both motors forward
BACKWARD → both motors backward
LEFT     → right motor forward,
            left motor backward
RIGHT    → left motor forward, 
            right motor backward
SPIN     → motors opposite directions
WIGGLE   → alternate left right quickly
JERK     → rapid forward backward
STOP     → all motors stop
```

---

## 📦 ESP32 → Arduino Commands (Serial):
```
"FORWARD"  → move forward
"BACKWARD" → move backward
"LEFT"     → turn left
"RIGHT"    → turn right
"SPIN"     → spin 360
"WIGGLE"   → wiggle left right
"JERK"     → rapid jerk
"STOP"     → stop all motors
"EDGE_F"   → front edge detected
"EDGE_B"   → back edge detected
```

---

## 🛠️ Build Phases:
```
Phase 1  → OLED emotions ✅ DONE!
Phase 2  → RFID unlock 🔜
Phase 3  → Touch sensors
Phase 4  → WiFi + time + Gemini AI
Phase 5  → Web server + control panel
Phase 6  → Chassis + motors + L298N
Phase 7  → Table edge IR sensors
Phase 8  → Pan-tilt head + HC-SR04
Phase 9  → Hand tracking logic
Phase 10 → Voice mic + speaker
Phase 11 → Bored/idle mode
Phase 12 → Final combine + polish 🤖🔥
```

---

## 🔧 Development Setup:
```
Code editor    → Acode (Android)
Compiler       → GitHub Actions
Flash method   → Konduktor OTA dashboard
Serial monitor → Termux arduino-cli monitor
```

## GitHub Workflow:
```
Edit code in GitHub → commit
→ GitHub Actions compiles automatically
→ Download Levibot.ino.bin (322KB)
→ Connect to KONDUKTOR-B4694C hotspot
→ Go to 192.168.4.1
→ Upload via OTA
→ Done! 🔥
```

---

## 📝 Notes:
- OLED and RFID run on 3.3V only — never 5V!
- Servos run on 5V (VIN pin)
- Motors run on 7.4V via L298N
- All GNDs must be connected together (common ground)
- ESP32 and Arduino Uno share common GND
- Li-ion batteries connected in SERIES for 7.4V
- Use Levibot.ino.bin (322KB) for OTA — NOT merged.bin (too big)

---

## 💬 How To Continue This Project With Claude:
Paste this at the start of every new conversation:
```
"Hey Claude! Continuing my Levibot project.
Here is my project bible: [paste this README]
Current phase completed: Phase X
Continue from: Phase X+1"
```

---

*Levibot — Built by Levii 🤖🔥*

