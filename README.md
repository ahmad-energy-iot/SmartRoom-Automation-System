# 🧠 Advanced AIoT Smart Room Automation & Security System

ESP32 • MQTT • Node-RED • Firebase • Telegram Alerts • Python Kamera Integration • Echtzeit Dashboard • Smart Security System

# 📌 Projektbeschreibung

Dieses Projekt demonstriert ein professionelles AIoT Smart Room Automation & Security System mit ESP32, MQTT-Kommunikation, Node-RED Automatisierung, Firebase Realtime Database, Telegram Bot Integration und Python-basierter Kameraüberwachung.

Das System überwacht kontinuierlich Umgebungsbedingungen in Echtzeit, darunter:

* Temperatur

* Luftfeuchtigkeit

* Bewegungserkennung

* Lichtintensität

* RGB-LED Status

* Betriebsmodi

* Lüfterstatus

* Nacht-Sicherheitsbedingungen

Die Plattform kombiniert:

* Echtzeit IoT Überwachung

* Cloud Synchronisation

* Smart Automation

* Bewegungsbasierte Sicherheit

* Telegram Benachrichtigungen

* Kamera Snapshot Warnungen

* Smart Dashboard Visualisierung

* Intelligente Nachtalarm-Logik

Das System wurde als vollständige professionelle IoT Automatisierungs- und Sicherheitsplattform entwickelt und eignet sich für:

* Smart Homes

* Smart Buildings

* Energieeffiziente Systeme

* Sicherheitsüberwachung

* IoT Ausbildung

* Cloudbasierte Überwachungslösungen

# 🚀 Funktionen

## 🔹 Echtzeitüberwachung

* Temperaturüberwachung

* Luftfeuchtigkeitsüberwachung

* PIR Bewegungserkennung

* LDR Lichterkennung

* RGB LED Statusüberwachung

* Lüfterstatusüberwachung

* Betriebsmodusüberwachung

## 🔹 Smart Automation

* AUTO MODE

* MANUAL ON MODE

* MANUAL OFF MODE

* Automatische Lüftersteuerung

* Automatische RGB LED Logik

* Bewegungsbasierte Lichtlogik

* Energieeffiziente Automatisierung

## 🔹 Sicherheitssystem

* Smart Nachtalarm-Logik

* Bewegungsbasierte Einbruchserkennung

* Telegram Sicherheitswarnungen

* Echtzeit Alarmbenachrichtigungen

* Kamera Snapshot Aufnahme

* Cooldown Schutzsystem

## 🔹 Cloud & Dashboard

* Firebase Realtime Database

* Node-RED Live Dashboard

* Echtzeit Diagramme

* Cloud Synchronisation

* Smartphone Zugriff

* Fernüberwachung

## 🔹 Professionelles Dashboard

* Modernes Dashboard Design

* Live Sensorwerte

* Interaktive Diagramme

* Systemanalysen

* Alarmüberwachung

* Gerätestatusvisualisierung

# 🖥️ Dashboard Bilder

## Hauptdashboard

(Add dashboard screenshot here)

## Sicherheitsüberwachung

(Add security system screenshot here)

## Firebase Cloud Überwachung

(Add Firebase screenshot here)

## Telegram Alarm Beispiel

(Add Telegram alert screenshot here)

# 🏗️ Systemarchitektur

Das System basiert auf einer mehrschichtigen AIoT Architektur:

1. Sensoren erfassen Umgebungsdaten

2. ESP32 verarbeitet Sensordaten lokal

3. MQTT überträgt Daten an Node-RED

4. Node-RED verarbeitet Automatisierungslogik

5. Firebase speichert Cloud-Daten

6. Telegram Bot sendet Alarmbenachrichtigungen

7. Python Kamerasystem erstellt Sicherheitsaufnahmen

8. Dashboard visualisiert Echtzeitdaten

# ☁️ Verwendete Technologien

* ESP32 Mikrocontroller

* MQTT Kommunikation

* Node-RED

* Firebase Realtime Database

* Python

* Telegram Bot API

* Embedded Systems

* IoT Automatisierung

* Smart Dashboard

* WiFi Networking

* Cloud Synchronisation

* Echtzeitüberwachung

* Bewegungserkennungssysteme

* Smart Security Systeme

# 🔧 Node-RED Flow Erklärung

Dieser Abschnitt erklärt die wichtigsten Node-RED Nodes des Projekts einschließlich Funktionalität, Zweck und interner Logik.

Der Flow kombiniert:

* Smart Automation

* Bewegungsbasierte Sicherheit

* Telegram Benachrichtigungen

* Python Kamera Integration

* Firebase Cloud Logging

* Echtzeitüberwachung

# 1️⃣ Night Alarm Logic

### 📌 Node Typ:

Function Node

### 🎯 Zweck:

Dieser Node ist die zentrale Sicherheitslogik des Systems.

Er überprüft kontinuierlich, ob die Alarmbedingungen erfüllt sind und entscheidet, ob ein Sicherheitsalarm ausgelöst werden soll.

### ✅ Überprüfte Bedingungen:

* Night Alarm Schalter ist aktiviert

* Bewegung wurde erkannt

* Raumstatus ist DARK

* Aktuelle Uhrzeit liegt zwischen 23:00 und 06:00

* Alarm Cooldown Zeit ist abgelaufen

### 🚨 Ausgelöste Aktionen:

Wenn alle Bedingungen erfüllt sind:

* Telegram Alarm wird gesendet

* Kamera Snapshot wird aufgenommen

* Alarmton wird abgespielt

* Firebase Alarmprotokoll wird erstellt

### 💻 Code:

```javascript
let motion = flow.get("motion");
let roomState = flow.get("room_state");
let alarmIsOn = flow.get("alarm_enabled") === true;

let hour = new Date().getHours();
let isNight = (hour >= 23 || hour < 6);

let motionDetected =
    motion === "YES" ||
    motion === "DETECTED" ||
    motion === "MOTION" ||
    motion === true ||
    motion === 1 ||
    motion === "1";

let isDark =
    roomState === "DARK" ||
    roomState === "Dark" ||
    roomState === "dark";

let lastAlarmTime = flow.get("last_alarm_time") || 0;
let now = Date.now();

let cooldown = 30 * 1000;

if (alarmIsOn && isNight && motionDetected && isDark) {

    if (now - lastAlarmTime < cooldown) {
        node.warn("Alarm ignored - cooldown active");
        return null;
    }

    flow.set("last_alarm_time", now);

    msg.payload = {
        type: "ALARM",
        motion: motion,
        roomState: roomState,
        time: now
    };

    return msg;
}

return null;
```

---

# 2️⃣ Build Telegram Alert

### 📌 Node Typ:

Function Node

### 🎯 Zweck:

Erstellt und formatiert Telegram Alarmnachrichten.

### 📲 Ergebnis:

Die generierte Nachricht wird direkt an die Telegram Bot API gesendet.

### 💻 Code:

```javascript
let motion = flow.get("motion") || "UNKNOWN";
let roomState = flow.get("room_state") || "UNKNOWN";
let now = new Date().toLocaleString("de-DE");

let text = `🚨 SMART ROOM ALERT

Motion detected: ${motion}
Room state: ${roomState}
Time: ${now}

System: Advanced IoT Smart Room Automation System`;

let botToken = "YOUR_TELEGRAM_BOT_TOKEN";
let chatId = "YOUR_CHAT_ID";

msg.method = "POST";
msg.url = `https://api.telegram.org/bot${botToken}/sendMessage`;
msg.headers = { "Content-Type": "application/json" };

msg.payload = {
    chat_id: chatId,
    text: text
};

return msg;
```

---

# 3️⃣ Telegram HTTP Request

### 📌 Node Typ:

HTTP Request Node

### 🎯 Zweck:

Dieser Node sendet die Telegram Alarmanfrage an die Telegram Bot API.

### ⚙️ Konfiguration:

```text
Method: use msg.method
URL: use msg.url
Return: a parsed JSON object
```

---

# 4️⃣ Camera Alert System

### 📌 Node Typ:

Exec Node

### 🎯 Zweck:

Startet das Python Kamera Sicherheitsskript, sobald das Alarmsystem ausgelöst wird.

### 📷 Funktionen:

* Webcam Snapshot aufnehmen

* Bild an Telegram senden

* Sicherheitsbeweise erstellen

### 💻 Befehl:

```bash
py C:\Users\hp\camera_alert.py
```

---

# 5️⃣ Build Firebase Alert Log

### 📌 Node Typ:

Function Node

### 🎯 Zweck:

Erstellt strukturierte Firebase Cloud Alarmprotokolle für Sicherheitsüberwachung.

### ☁️ Gespeicherte Informationen:

* Bewegungsstatus

* Raumstatus

* Temperatur

* Luftfeuchtigkeit

* Lichtwert

* RGB Status

* Lüfterstatus

* Aktueller Modus

* Zeitstempel

* Systemidentifikation

### 💻 Code:

```javascript
let motion = flow.get("motion");
let roomState = flow.get("room_state");
let temperature = flow.get("temperature");
let humidity = flow.get("humidity");
let fan = flow.get("fan_state");
let rgb = flow.get("rgb_status");
let mode = flow.get("current_mode");
let light = flow.get("light_value");

msg.method = "POST";
msg.url = "https://YOUR_FIREBASE_DATABASE_URL/alerts.json";
msg.headers = { "Content-Type": "application/json" };

msg.payload = {
    type: "MOTION_ALERT",
    motion: motion,
    room_state: roomState,
    temperature: temperature,
    humidity: humidity,
    light: light,
    fan: fan,
    rgb: rgb,
    mode: mode,
    time: new Date().toLocaleString("de-DE"),
    system: "Advanced IoT Smart Room Automation System"
};

return msg;
```

---

# 6️⃣ Firebase Alert Logger

### 📌 Node Typ:

HTTP Request Node

### 🎯 Zweck:

Sendet Sicherheitsprotokolle an Firebase Realtime Database.

### ☁️ Firebase Datenpfad:

```text
/alerts
```

### ⚙️ Konfiguration:

```text
Method: use msg.method
URL: use msg.url
Return: a parsed JSON object
```

---

# 7️⃣ Save Motion

### 📌 Node Typ:

Function Node

### 🎯 Zweck:

Speichert den letzten Bewegungswert im Node-RED Flow Speicher.

### 💻 Code:

```javascript
flow.set("motion", msg.payload);
return msg;
```

---

# 8️⃣ Save Room State

### 📌 Node Typ:

Function Node

### 🎯 Zweck:

Speichert den Raumhelligkeitsstatus:

* DARK

* BRIGHT

### 💻 Code:

```javascript
flow.set("room_state", msg.payload);
return msg;
```

---

# 9️⃣ Save Alarm State

### 📌 Node Typ:

Function Node

### 🎯 Zweck:

Speichert den aktuellen Zustand des Night Alarm Schalters.

### 💻 Code:

```javascript
flow.set("alarm_enabled", msg.payload === true);
return null;
```

---

# 🔟 Alarm Sound System

### 📌 Node Typ:

Template Node

### 🎯 Zweck:

Spielt einen Alarmton im Dashboard ab, sobald ein Eindringling erkannt wird.

### 💻 Code:

```html
<audio autoplay>
  <source src="https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg" type="audio/ogg">
</audio>
```

---

# 1️⃣1️⃣ Developer Dashboard Card

### 📌 Node Typ:

Template Node

### 🎯 Zweck:

Zeigt professionelle Entwicklerinformationen im Dashboard an.

### 💻 Code:

```html
<div style="
background:linear-gradient(90deg,#1a1a1a,#7a0015);
color:white;
padding:20px;
text-align:center;
border-radius:20px;
box-shadow:0px 0px 15px rgba(0,0,0,0.4);
">

    <div style="
font-size:34px;
font-weight:bold;
margin-bottom:10px;
">
        AHMAD AZROUN
    </div>

    <div style="
font-size:18px;
font-weight:600;
margin-bottom:18px;
line-height:1.6;
">
        Renewable Energy Manager | IoT & AI Specialist | Smart Energy Systems Developer
    </div>

    <div style="
font-size:17px;
margin-top:10px;
">
        📡 ESP32 • MQTT • Node-RED • Python
    </div>

    <div style="
font-size:17px;
margin-top:8px;
">
        🤖 AI Powered Monitoring System
    </div>

</div>
```

# ⚙️ Installation

## Benötigte Hardware-Komponenten

* ESP32 DevKit V1
* PIR-Bewegungssensor
* DHT11 / DHT22 Sensor
* LDR-Lichtsensor
* RGB-LED
* Relaismodul
* Lüfter
* OLED Display SSD1306
* Breadboard
* Jumper-Kabel

---

## Benötigte Software

* Thonny IDE
* Node-RED
* Mosquitto MQTT Broker
* Python 3.x
* Firebase Realtime Database

---

## Python-Bibliotheken

```bash
pip install opencv-python
pip install requests
```

---

## Benötigte MicroPython-Bibliotheken

```python
from machine import Pin, ADC, PWM, I2C
import network
import time
import ssd1306
from umqtt.simple import MQTTClient
```

---

# 🔒 Sicherheitshinweis

Vor der Veröffentlichung des Projekts auf GitHub:

## ❌ Entfernen oder Verbergen:

* WLAN-SSID
* WLAN-Passwort
* Telegram Bot Token
* Telegram Chat ID
* Firebase Secret Keys
* Persönliche E-Mail-Adresse
* Persönliche Telefonnummer

## ✅ Platzhalter verwenden:

```text
YOUR_WIFI_NAME
YOUR_WIFI_PASSWORD
YOUR_BOT_TOKEN
YOUR_CHAT_ID
YOUR_FIREBASE_URL
```

Dies schützt das Projekt und ermöglicht gleichzeitig anderen Entwicklern, die Systemarchitektur professionell zu verstehen und wiederzuverwenden.

---

# 👨‍💻 Entwickler

## Ahmad Azroun

Renewable Energy Manager
IoT & KI-Systementwickler
Spezialist für Smart Energy Systeme






English:

Advanced AIoT Smart Room Automation & Security System

ESP32 • MQTT • Node-RED • Firebase • Telegram Alerts • Python Camera Integration • Real-Time Dashboard • Smart Security System

---

 Project Description

This project demonstrates a professional AIoT Smart Room Automation & Security System using ESP32, MQTT communication, Node-RED automation, Firebase Realtime Database, Telegram Bot integration, and Python-based camera monitoring.

The system continuously monitors environmental conditions in real time including:

- Temperature
- Humidity
- Motion Detection
- Light Intensity
- RGB LED Status
- Operating Modes
- Fan Status
- Night Security Conditions

The platform combines:

- Real-Time IoT Monitoring
- Cloud Synchronization
- Smart Automation
- Motion-Based Security
- Telegram Notifications
- Camera Snapshot Alerts
- Smart Dashboard Visualization
- Intelligent Night Alarm Logic

The system was designed as a complete professional IoT automation and security platform suitable for:

- Smart Homes
- Smart Buildings
- Energy-Efficient Systems
- Security Monitoring
- IoT Education
- Cloud-Based Monitoring Solutions

---

 Features

🔹 Real-Time Monitoring

- Temperature Monitoring
- Humidity Monitoring
- PIR Motion Detection
- LDR Light Detection
- RGB LED Status Monitoring
- Fan Status Monitoring
- Operating Mode Monitoring

🔹 Smart Automation

- AUTO MODE
- MANUAL ON MODE
- MANUAL OFF MODE
- Automatic Fan Control
- Automatic RGB LED Logic
- Motion-Based Lighting Logic
- Energy-Efficient Automation

🔹 Security System

- Smart Night Alarm Logic
- Motion-Based Intrusion Detection
- Telegram Security Alerts
- Real-Time Alarm Notifications
- Camera Snapshot Capture
- Cooldown Protection System

🔹 Cloud & Dashboard

- Firebase Realtime Database
- Node-RED Live Dashboard
- Real-Time Charts
- Cloud Synchronization
- Smartphone Accessibility
- Remote Monitoring

🔹 Professional Dashboard

- Modern Dashboard Design
- Live Sensor Values
- Interactive Charts
- System Analytics
- Alarm Monitoring
- Device Status Visualization

---

🖥️ Dashboard Images

Main Dashboard

(Add dashboard screenshot here)

Security Monitoring

(Add security system screenshot here)

Firebase Cloud Monitoring

(Add Firebase screenshot here)

Telegram Alert Example

(Add Telegram alert screenshot here)

---

🏗️ System Architecture

The system is based on a multi-layer AIoT architecture:

1. Sensors collect environmental data
2. ESP32 processes sensor data locally
3. MQTT transfers data to Node-RED
4. Node-RED processes automation logic
5. Firebase stores cloud data
6. Telegram Bot sends alarm notifications
7. Python camera system captures security snapshots
8. Dashboard visualizes real-time system data

---

☁️ Technologies Used

- ESP32 Microcontroller
- MQTT Communication
- Node-RED
- Firebase Realtime Database
- Python
- Telegram Bot API
- Embedded Systems
- IoT Automation
- Smart Dashboard
- WiFi Networking
- Cloud Synchronization
- Real-Time Monitoring
- Motion Detection Systems
- Smart Security Systems

---

🔧 Node-RED Flow Explanation

This section explains the most important Node-RED nodes used in the project, including their functionality, purpose, and internal logic.

The flow combines:

- Smart automation
- Motion-based security
- Telegram notifications
- Python camera integration
- Firebase cloud logging
- Real-time monitoring

---

1️⃣ Night Alarm Logic

📌 Node Type:

Function Node

🎯 Purpose:

This node is the core security logic of the system.

It continuously checks whether the alarm conditions are valid and decides if the system should trigger a security alert.

✅ Conditions Checked:

- Night Alarm switch is enabled
- Motion is detected
- Room state is DARK
- Current time is between 23:00 and 06:00
- Alarm cooldown timer has expired

🚨 Actions Triggered:

If all conditions are true:

- Telegram alert is sent
- Camera snapshot is captured
- Alarm sound is played
- Firebase alert log is created

💻 Code:

let motion = flow.get("motion");
let roomState = flow.get("room_state");
let alarmIsOn = flow.get("alarm_enabled") === true;

let hour = new Date().getHours();
let isNight = (hour >= 23 || hour < 6);

let motionDetected =
    motion === "YES" ||
    motion === "DETECTED" ||
    motion === "MOTION" ||
    motion === true ||
    motion === 1 ||
    motion === "1";

let isDark =
    roomState === "DARK" ||
    roomState === "Dark" ||
    roomState === "dark";

let lastAlarmTime = flow.get("last_alarm_time") || 0;
let now = Date.now();

let cooldown = 30 * 1000;

if (alarmIsOn && isNight && motionDetected && isDark) {

    if (now - lastAlarmTime < cooldown) {
        node.warn("Alarm ignored - cooldown active");
        return null;
    }

    flow.set("last_alarm_time", now);

    msg.payload = {
        type: "ALARM",
        motion: motion,
        roomState: roomState,
        time: now
    };

    return msg;
}

return null;

---

2️⃣ Build Telegram Alert

📌 Node Type:

Function Node

🎯 Purpose:

Creates and formats Telegram alarm messages.

📲 Result:

The generated message is sent directly to the Telegram Bot API.

💻 Code:

let motion = flow.get("motion") || "UNKNOWN";
let roomState = flow.get("room_state") || "UNKNOWN";
let now = new Date().toLocaleString("de-DE");

let text = `🚨 SMART ROOM ALERT

Motion detected: ${motion}
Room state: ${roomState}
Time: ${now}

System: Advanced IoT Smart Room Automation System`;

let botToken = "YOUR_TELEGRAM_BOT_TOKEN";
let chatId = "YOUR_CHAT_ID";

msg.method = "POST";
msg.url = `https://api.telegram.org/bot${botToken}/sendMessage`;
msg.headers = { "Content-Type": "application/json" };

msg.payload = {
    chat_id: chatId,
    text: text
};

return msg;

---

3️⃣ Telegram HTTP Request

📌 Node Type:

HTTP Request Node

🎯 Purpose:

This node sends the Telegram alert request to the Telegram Bot API.

⚙️ Configuration:

Method: use msg.method
URL: use msg.url
Return: a parsed JSON object

---

4️⃣ Camera Alert System

📌 Node Type:

Exec Node

🎯 Purpose:

Runs the Python camera security script whenever the alarm system is triggered.

📷 Functions:

- Captures webcam snapshot
- Sends image to Telegram
- Creates visual security evidence

💻 Command:

py C:\Users\hp\camera_alert.py

---

5️⃣ Build Firebase Alert Log

📌 Node Type:

Function Node

🎯 Purpose:

Creates structured Firebase cloud alert logs for security monitoring.

☁️ Stored Information:

- Motion state
- Room state
- Temperature
- Humidity
- Light value
- RGB status
- Fan status
- Current mode
- Timestamp
- System identification

💻 Code:

let motion = flow.get("motion");
let roomState = flow.get("room_state");
let temperature = flow.get("temperature");
let humidity = flow.get("humidity");
let fan = flow.get("fan_state");
let rgb = flow.get("rgb_status");
let mode = flow.get("current_mode");
let light = flow.get("light_value");

msg.method = "POST";
msg.url = "https://YOUR_FIREBASE_DATABASE_URL/alerts.json";
msg.headers = { "Content-Type": "application/json" };

msg.payload = {
    type: "MOTION_ALERT",
    motion: motion,
    room_state: roomState,
    temperature: temperature,
    humidity: humidity,
    light: light,
    fan: fan,
    rgb: rgb,
    mode: mode,
    time: new Date().toLocaleString("de-DE"),
    system: "Advanced IoT Smart Room Automation System"
};

return msg;

---

6️⃣ Firebase Alert Logger

📌 Node Type:

HTTP Request Node

🎯 Purpose:

Sends security alert logs to Firebase Realtime Database.

☁️ Firebase Database Path:

/alerts

⚙️ Configuration:

Method: use msg.method
URL: use msg.url
Return: a parsed JSON object

---

7️⃣ Save Motion

📌 Node Type:

Function Node

🎯 Purpose:

Stores the latest motion sensor value inside Node-RED flow memory.

💻 Code:

flow.set("motion", msg.payload);
return msg;

---

8️⃣ Save Room State

📌 Node Type:

Function Node

🎯 Purpose:

Stores room brightness state:

- DARK
- BRIGHT

💻 Code:

flow.set("room_state", msg.payload);
return msg;

---

9️⃣ Save Alarm State

📌 Node Type:

Function Node

🎯 Purpose:

Stores the current Night Alarm switch state.

💻 Code:

flow.set("alarm_enabled", msg.payload === true);
return null;

---

🔟 Alarm Sound System

📌 Node Type:

Template Node

🎯 Purpose:

Plays an alarm sound inside the dashboard whenever an intrusion is detected.

💻 Code:

<audio autoplay>
  <source src="https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg" type="audio/ogg">
</audio>

---

1️⃣1️⃣ Developer Dashboard Card

📌 Node Type:

Template Node

🎯 Purpose:

Displays professional developer branding and project information inside the Node-RED dashboard.

💻 Code:

<div style="
background:linear-gradient(90deg,#1a1a1a,#7a0015);
color:white;
padding:20px;
text-align:center;
border-radius:20px;
box-shadow:0px 0px 15px rgba(0,0,0,0.4);
">

    <div style="
font-size:34px;
font-weight:bold;
margin-bottom:10px;
">
        AHMAD AZROUN
    </div>

    <div style="
font-size:18px;
font-weight:600;
margin-bottom:18px;
line-height:1.6;
">
        Renewable Energy Manager | IoT & AI Specialist | Smart Energy Systems Developer
    </div>

    <div style="
font-size:17px;
margin-top:10px;
">
        📡 ESP32 • MQTT • Node-RED • Python
    </div>

    <div style="
font-size:17px;
margin-top:8px;
">
        🤖 AI Powered Monitoring System
    </div>

</div>

---

⚙️ Installation

Required Components

- ESP32 DevKit V1
- PIR Sensor
- DHT11 / DHT22
- LDR Sensor
- RGB LED
- Relay Module
- Cooling Fan
- OLED Display SSD1306
- Breadboard
- Jumper Wires

---

Required Software

- Thonny IDE
- Node-RED
- Mosquitto MQTT Broker
- Python 3.x
- Firebase Realtime Database

---

Python Libraries

pip install opencv-python
pip install requests

---

Required MicroPython Libraries

from machine import Pin, ADC, PWM, I2C
import network
import time
import ssd1306
from umqtt.simple import MQTTClient

---

🔒 Security Note

Before publishing the project to GitHub:

❌ Remove or Hide:

- WiFi SSID
- WiFi Password
- Telegram Bot Token
- Telegram Chat ID
- Firebase Secret Keys
- Personal Email
- Personal Phone Number

✅ Use Placeholders Instead:

YOUR_WIFI_NAME
YOUR_WIFI_PASSWORD
YOUR_BOT_TOKEN
YOUR_CHAT_ID
YOUR_FIREBASE_URL

This keeps the project secure while still allowing others to understand and reuse the system architecture professionally.

---

Developer

Ahmad Azroun

Renewable Energy Manager
IoT & AI Systems Developer
Smart Energy Systems Specialist
