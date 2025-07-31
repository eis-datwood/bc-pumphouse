# Briar Court Pumphouse Monitoring

## Overview
This projects aim is to provide memembers of the Briar Court Association with real-time insights into conditions inside the pump house remotley at any time. 
The current sensed data points are:
* Temperature
* Humidity

## 🌡️ LoRa Weather Sensor Node (DHT22 + Arduino + RYLR896)

This system uses an **Arduino Uno R3**, **DHT22 temperature/humidity sensor**, and a **RYLR896 LoRa module** to transmit environmental data over long distances. A USB-connected LoRa receiver collects the data, which is then surfaced at **[www.briarct.com](http://www.briarct.com)** for remote monitoring.

---

## 🧰 Components Used

| Component               | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| **Arduino Uno R3**     | Main microcontroller unit                                                   |
| **DHT22**              | Digital humidity and temperature sensor                                     |
| **RYLR896 (LoRa)**     | LoRa transceiver module using Semtech SX1276 chipset                        |
| **USB to Serial UART** | CP2102N USB to UART bridge for connecting LoRa receiver to PC               |
| **Breadboard & Wires** | For physical connections and prototyping                                    |
| **PC**                 | Runs the receiver script and forwards data for public web display           |

---

## ⚙️ How It Works

### 🛰️ Transmitter (Arduino)

1. The **DHT22** sensor reads humidity and temperature every 5 seconds.
2. The data is formatted as a JSON string:  
   `{"t": temperature, "h": humidity}`
3. The payload is sent to the **RYLR896 LoRa** module via `SoftwareSerial` using the `AT+SEND` command.
4. LoRa transmits the packet to a paired receiver module.

### 📥 Receiver (LoRa + Python)

1. The **receiver LoRa module** is connected to a PC via a **CP2102N USB UART bridge**.
2. A Python script listens on the serial port at **115200 baud**.
3. It parses the LoRa message (e.g., `+RCV=0,24,{"t":67.64,"h":63.70},-37,33`) and extracts just the JSON.
4. *TODO*: Sensor data stored for web server to access

> 📡 The data from the receiver is surfaced on a **public website**, allowing remote real-time monitoring.

---

## 🔌 Wiring Summary

### Transmitter Side (Arduino)

| LoRa Pin     | Arduino Pin |
|--------------|-------------|
| VDD          | 3.3V        |
| GND          | GND         |
| TXD (LoRa)   | Pin 8 (Arduino RX via SoftwareSerial) |
| RXD (LoRa)   | Pin 7 (Arduino TX via SoftwareSerial) |
| DHT22 Data   | Pin 5       |

### Receiver Side (LoRa + USB Serial)

| LoRa Pin     | USB UART Pin (CP2102N) |
|--------------|------------------------|
| VDD          | 3.3V                   |
| GND          | GND                    |
| TXD (LoRa)   | RXD                    |
| RXD (LoRa)   | TXD                    |

---

## 💸 Electricity Cost Estimate

This project is designed to run continuously (24/7). Below is an estimate of the operating cost:

| Item             | Value              |
|------------------|--------------------|
| **Estimated Power** | ~0.5 watts         |
| **Daily Use**       | 0.012 kWh          |
| **Monthly Use**     | 0.36 kWh           |
| **Yearly Use**      | 4.38 kWh           |
| **Electricity Rate**| $0.112 / kWh       |
| **Monthly Cost**    | ~$0.04             |
| **Yearly Cost**     | **~$0.49**         |

This makes it extremely affordable to keep running continuously.

---

## 🌍 Public Dashboard

The data collected by the receiver is uploaded to a **public-facing web interface**, allowing users to view live temperature and humidity readings.

> 📌 Details and link to the dashboard will be added in a future update.

---

## 📁 Repository Contents

- `transmitter/`: Arduino sketch for DHT22 + LoRa transmission
- `README.md`: Project overview and documentation
