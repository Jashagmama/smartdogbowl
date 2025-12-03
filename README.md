# 🐶 Smart Dog Bowl — BLE Eating Detection System

This project implements a **Bluetooth-enabled dog eating detection system** using:

- **Arduino Nano 33 BLE Sense**
- **Edge Impulse TinyML sound classification**
- **Web Bluetooth UI** for real-time monitoring

The system listens for dog chewing sounds, classifies them using an on-device ML model, and sends the chewing probability over **Bluetooth Low Energy (BLE)** to a simple web-based UI that displays whether the dog is currently eating or has finished.

---

## 🚀 Features

### 🐕 TinyML Sound Classification
Trained using Edge Impulse with two classes:

- `Dog Chewing`
- `Noise`

Model highlights:

- ~96% validation accuracy
- Good separation between chewing vs. noise
- Runs fully on-device on the Nano 33 BLE Sense (no internet required)

### 📡 Bluetooth Low Energy Output

The Arduino exposes a BLE service that sends:

- A single `float` value = **Dog Chewing probability** (0.0–1.0)

The value is updated after each inference window (about once per second).

### 🌐 Web Bluetooth Interface

The web UI (in the `dog_eating_ui` folder):

- Connects to the Arduino via Web Bluetooth
- Shows the current chewing probability
- Uses a simple state machine to detect start/end of an eating session
- Logs messages like:  
  `Eating finished – dog has eaten at 19:42:13`

---

## 🧠 How It Works

1. **Audio Capture**  
   The Nano 33 BLE Sense PDM microphone records 1-second audio windows.

2. **Feature Extraction & Inference**  
   Edge Impulse MFE (Mel-filterbank energy) features feed a 1D CNN model.  
   The model outputs probabilities for `Dog Chewing` and `Noise`.

3. **BLE Notification**  
   After each inference, the Arduino writes the chewing probability to a BLE characteristic and notifies any subscribed client.

4. **Browser Logic**  
   The web app reads the probability stream.  
   - If probability stays **> 0.90** for several windows → state = `EATING`  
   - If it then stays **< 0.90** for several windows → state = `NOT EATING`, and the time is logged as “dog has eaten”.

---

## 📁 Project Structure

```text
smartdogbowl/
 ├── dog_eating_ui/              # Web Bluetooth UI (HTML + JS)
 ├── ei-smart-dog-bowl-2.0.zip   # Edge Impulse exported Arduino library
 ├── .gitignore
 └── README.md

