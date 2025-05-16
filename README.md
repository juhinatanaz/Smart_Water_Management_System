# Smart Water Level Monitoring and Leakage Detection System 💧

This project automates water tank filling and detects leakage using an ESP32 microcontroller, water level sensors, a relay module, and a buzzer.

---

## 🔧 Features

- Automatic motor control based on water level.
- Real-time water level monitoring.
- Leakage detection with buzzer alert.
- Efficient water usage to prevent overflow and wastage.

---

## 🧰 Components

- ESP32 microcontroller
- Water level sensors
- Relay module
- Buzzer
- Submersible pump (motor)
- Water tanks (buckets)
- Power supply
- Connecting wires and pipes

---

## ⚙️ How It Works

1. Water level sensors monitor the tank water level.
2. When water is below a set level, the ESP32 activates the relay to turn on the water pump.
3. Once the tank is full, the pump automatically turns off.
4. Leakage detection logic triggers the buzzer if water flow is detected without an increase in tank water level.

---

## 📸 Setup Images

### 🛠️ Full setup including ESP32, sensors, relay, and motor
![Setup](./images/Set_up.png)

### 📍 Sensor placement inside the water tank
![Sensor](./images/Sensor.png)

### ⚡ Power supply connected to the system
![Power Supply](./images/power_supply.png)

### 🔌 ESP32 with relay module and buzzer wiring
![Control Board](./images/Control_board.png)

### 💧 Water flow during motor operation
![Automatic Tank Refilling](./images/automatic_tank_refilling.png)

### 🚨 Leakage detection triggered (buzzer ON)
![Leakage Detection](./images/Leakage_detection.png)

---

## 📌 Project Status

✅ Functional and tested with basic components  
📈 Ready for further improvements like mobile alerts and analytics.

