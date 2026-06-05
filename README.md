# rfid-door-lock-arduino
RFID Based Door Lock System Using Arduino Nano with LCD Display


# RFID Based Door Lock System
### Using Arduino Nano with LCD Display

**Author:** Hussain Abdulah Saleh Shimanto  
**Roll No:** 1815052 | **Reg No:** 1189 | **Session:** 18-19  
**Degree:** B.Sc. in Electrical and Electronic Engineering  
**Institution:** Islamic University, Kushtia-7003, Bangladesh  
**Supervisor:** Professor Dr. Md. Shahjahan Ali  
**Submitted:** June 2024

---

## Abstract

This project presents the design and implementation of an RFID-based door lock system using an Arduino Nano and a 16×2 LCD display. The system reads RFID tags and allows access only to authorized users. The LCD display provides real-time feedback about the status of each access attempt. A servo motor automates the locking and unlocking mechanism, and an active buzzer provides audible confirmation. The system is cost-effective, scalable, and suitable for residential, office, and institutional security applications.

---

## Hardware Components

| No. | Component | Specification | Purpose |
|-----|-----------|--------------|---------|
| 1 | Arduino Nano | ATmega328P, 5V | Main controller |
| 2 | RC522 RFID Reader | 13.56 MHz, SPI | Reads RFID tags |
| 3 | RFID Tags / Cards | ISO 14443A | User authentication |
| 4 | LCD Display (16×2) | HD44780 controller | Visual user feedback |
| 5 | I2C LCD Adapter | PCF8574, address 0x27 | Reduces wiring to 2 pins |
| 6 | SG90 Servo Motor | 0–180° rotation | Door lock/unlock |
| 7 | Active Buzzer | 5V DC | Audible confirmation |
| 8 | Breadboard & Wires | — | Prototyping |
| 9 | USB / 5V Power Supply | — | System power |

---

## System Architecture

```
┌─────────────┐     SPI      ┌──────────────┐    PWM    ┌───────────────┐
│ RC522 RFID  │ ──────────── │              │ ───────── │  SG90 Servo   │
│   Reader    │              │  Arduino     │           │  (Lock/Unlock)│
└─────────────┘              │    Nano      │           └───────────────┘
                             │ (ATmega328P) │
┌─────────────┐     I2C      │              │  Digital  ┌───────────────┐
│  LCD 16×2   │ ──────────── │              │ ───────── │ Active Buzzer │
│ + I2C Module│              │              │           └───────────────┘
└─────────────┘              └──────────────┘
```

---

## Pin Connections

### RC522 RFID Reader → Arduino Nano

| RC522 Pin | Arduino Nano Pin |
|-----------|-----------------|
| VCC | 3.3V |
| GND | GND |
| SDA (SS) | D10 |
| SCK | D13 |
| MOSI | D11 |
| MISO | D12 |
| RST | D9 |

### I2C LCD Adapter Module → Arduino Nano

| I2C Module Pin | Arduino Nano Pin |
|----------------|-----------------|
| VCC | 5V |
| GND | GND |
| SDA | A4 |
| SCL | A5 |

### Other Components → Arduino Nano

| Component | Pin | Arduino Nano Pin |
|-----------|-----|-----------------|
| Servo Motor | Signal | D3 |
| Servo Motor | VCC | 5V |
| Servo Motor | GND | GND |
| Active Buzzer | (+) | D2 |
| Active Buzzer | (−) | GND |

---

## How It Works

| Step | Action |
|------|--------|
| 1 | System powers on — servo locks at 0°, LCD shows `RFID Lock System` |
| 2 | RC522 module continuously scans for RFID tags in range (~5 cm) |
| 3 | Tag detected — UID is read and printed to Serial Monitor |
| 4 | UID compared against authorized list stored in Arduino memory |
| 5 ✅ | **Authorized:** buzzer beeps once, servo rotates to 90° (unlock), LCD shows `Access Granted` |
| 5 ❌ | **Unauthorized:** buzzer beeps 3 times, servo stays at 0°, LCD shows `Access Denied` |
| 6 | After 2 seconds, servo re-locks and LCD resets to idle message |

---

## LCD Feedback Summary

| Event | LCD Line 1 | LCD Line 2 | Buzzer |
|-------|-----------|-----------|--------|
| Idle / Startup | `RFID Lock System` | `Scan your card..` | Silent |
| Access Granted | `Access Granted` | `Unblocked!` | 1 long beep |
| Access Denied | `Access Denied` | `Unauthorized!` | 3 short beeps |

---

## Required Libraries

Install from **Arduino IDE → Sketch → Include Library → Manage Libraries**:

| Library | Author | Purpose |
|---------|--------|---------|
| MFRC522 | GithubCommunity | RC522 RFID reader communication |
| LiquidCrystal_I2C | Frank de Brabander | I2C LCD control |
| Servo | Arduino (built-in) | Servo motor control |
| SPI | Arduino (built-in) | SPI bus for RFID |

---

## How to Upload the Code

1. Connect Arduino Nano to PC via USB
2. Open Arduino IDE
3. **Tools → Board → Arduino Nano**
4. **Tools → Processor → ATmega328P (Old Bootloader)** *(try this if upload fails)*
5. **Tools → Port → [your COM port]**
6. Open `rfid_door_lock.ino`
7. Click **Verify (✓)** then **Upload (→)**
8. Open **Serial Monitor** at 9600 baud

---

## How to Add Your Own RFID Tag

1. Upload the code and open Serial Monitor (9600 baud)
2. Hold your card/tag near the RC522 reader
3. Your UID will appear, for example: `UID tag: B3 B4 D6 2F`
4. In `rfid_door_lock.ino`, find this line and replace the UID:

```cpp
const String authorizedUIDs[] = {
  "B3 B4 D6 2F"   // ← replace with your card's UID
};
```

5. To authorize **multiple cards**, add more entries:

```cpp
const String authorizedUIDs[] = {
  "B3 B4 D6 2F",
  "AA BB CC DD",
  "11 22 33 44"
};
```

6. Re-upload the code.

---

## Repository Structure

```
├── rfid_door_lock.ino       # Complete Arduino source code
├── docs/
│   └── project_paper.docx  # Full B.Sc. project report
├── images/
│   ├── circuit_diagram.png  # Wiring diagram
│   └── system_photo.jpg    # Physical prototype photo
└── README.md
```

---

## Applications

- Residential door security
- Office and corporate access control
- Educational institution labs and server rooms
- Healthcare facility restricted areas
- Hotel room key systems
- Event management entry points

---

## Future Scope

**Network & Remote Access:**
- ESP8266/ESP32 Wi-Fi for remote monitoring and cloud access logs
- GSM module (SIM800L) for SMS alerts on access attempts
- Web dashboard for real-time monitoring

**Mobile Integration:**
- Bluetooth/NFC to use a smartphone as an access token
- Mobile app for managing authorized users and viewing logs

**Enhanced Security:**
- Fingerprint scanner for two-factor authentication
- Encryption of RFID communication to prevent cloning
- Camera integration (ESP32-CAM) to photograph each access attempt

**Advanced Management:**
- Time-based access (allow entry only during certain hours)
- SD card or cloud logging of all entry/exit events
- Battery backup to maintain operation during power outages

---

## References

1. Banzi, M. & Shiloh, M. (2014). *Getting Started with Arduino* (3rd ed.). O'Reilly Media.
2. Monk, S. (2016). *Programming Arduino: Getting Started with Sketches* (2nd ed.). McGraw-Hill Education.
3. Meyers, J. (2018). *RFID Programming Made Simple and Fun!* CreateSpace.
4. Banerjee, M. (2016). *Arduino for Beginners: Step-by-Step Guide.*
5. Blum, J. (2013). *Exploring Arduino: Tools and Techniques for Engineering Wizardry.*
6. https://srituhobby.com/how-to-make-an-rfid-door-lock-system-using-an-arduino-nano-board/
