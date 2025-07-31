# 🌡️ Briar Court Pumphouse Monitoring

## Overview

This project provides members of the Briar Court Association with **real-time insights into conditions inside the pumphouse**, accessible remotely at any time.

### Current sensed data points:

- **Temperature**
- **Humidity**

---

## 🧰 Components Used

| Component                   | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| **Arduino Uno R3**         | Main microcontroller unit                                                   |
| **DHT22**                  | Digital humidity and temperature sensor                                     |
| **RYLR896 (LoRa)**         | LoRa transceiver module using Semtech SX1276 chipset                        |
| **Logic Level Converter**  | 4-channel I²C-safe logic level shifter for 5V ↔ 3.3V signal translation      |
| **USB to Serial UART**     | CP2102N USB to UART bridge for connecting LoRa receiver to PC               |
| **Breadboard & Wires**     | For prototyping and interconnection                                         |
| **PC**                     | Runs the receiver script and forwards data for public web display           |

---

## ⚙️ How It Works

### 🛰️ Transmitter (Arduino)

1. The **DHT22** sensor reads humidity and temperature every 5 seconds.
2. Data is formatted as a compact JSON string:  
   `{"t": temperature, "h": humidity}`
3. This string is sent via `SoftwareSerial` to the **RYLR896 LoRa module** using the `AT+SEND` command.
4. The **LoRa module** transmits the packet over RF to a paired receiver unit.

### 📥 Receiver (LoRa + Python)

1. A second **LoRa module** is connected to a PC via **CP2102N USB-to-Serial**.
2. A Python script listens at **115200 baud**, parses LoRa messages, and extracts JSON payloads.
3. These values are stored or forwarded to a web server.

TODO:
> ✅ The data is made available on **[www.briarct.com](http://www.briarct.com)** for public viewing.

---

## 🔌 Wiring Summary

### Transmitter Side (Arduino Uno R3 + Logic Level Shifter)

| LoRa Pin (3.3V logic) | Logic Level Shifter | Arduino Pin (5V logic)        |
|------------------------|---------------------|--------------------------------|
| TXD                    | LV1 → HV1           | Pin 8 (Arduino RX via SoftwareSerial) |
| RXD                    | HV2 → LV2           | Pin 7 (Arduino TX via SoftwareSerial) |
| VDD                    | —                   | 3.3V (direct, not through shifter)    |
| GND                    | —                   | GND                             |
| DHT22 Data             | —                   | Pin 5                            |

> ⚠️ The **RYLR896 LoRa module operates at 3.3V logic**, while the Arduino Uno uses 5V. A logic level converter is used to safely shift signals between them.

### Receiver Side (LoRa + USB Serial)

| LoRa Pin     | USB UART Pin (CP2102N) |
|--------------|------------------------|
| TXD          | RXD                    |
| RXD          | TXD                    |
| VDD          | 3.3V                   |
| GND          | GND                    |

---

## 💸 Electricity Cost Estimate

The transmitter is designed to run continuously (24/7). Here's a breakdown of the estimated electricity cost:

| Item                | Value              |
|---------------------|--------------------|
| **Estimated Power** | ~0.5 watts         |
| **Daily Usage**     | 0.012 kWh          |
| **Monthly Usage**   | 0.36 kWh           |
| **Yearly Usage**    | 4.38 kWh           |
| **Electricity Rate**| $0.112 / kWh       |
| **Monthly Cost**    | ~$0.04             |
| **Yearly Cost**     | **~$0.49**         |

This makes it cost-effective to run the monitoring station year-round.

---

## 🌍 Public Dashboard

All collected data is surfaced via a **public dashboard** hosted at:

🔗 [www.briarct.com](http://www.briarct.com)

> 📌 Real-time readings are updated every 5 seconds when the transmitter is active.

---

## 📁 Repository Contents

- `transmitter/`: Arduino sketch for DHT22 + LoRa transmission  
- `README.md`: This documentation file
