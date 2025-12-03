🐶 Smart Dog Bowl — BLE Eating Detection System

This project implements a Bluetooth-enabled dog eating detection system using:

Arduino Nano 33 BLE Sense

Edge Impulse TinyML sound classification

Web Bluetooth UI for real-time monitoring

The system listens for dog chewing sounds, classifies them using an on-device ML model, and sends the probability over Bluetooth Low Energy (BLE) to a simple web-based UI that displays whether the dog is eating or has finished.

🚀 Features
🐕 TinyML Sound Classification

Trained using Edge Impulse with two classes:

Dog Chewing

Noise

Model performance:

96% accuracy

Strong separation between chewing vs noise

Works fully offline on the microcontroller

📡 Bluetooth Low Energy Output

Arduino exposes a BLE service:

UUID Service → Dog Eating Sound Service

Characteristic → Dog Chewing Probability (float)

Updated every inference cycle (≈ 1 second).

🌐 Web Bluetooth Interface

Included in the repo (/dog_eating_ui):

Connects to Arduino via BLE

Shows real-time chewing probability

Detects eating sessions

Logs “Dog has eaten at HH:MM:SS” when chewing drops

No Android/iOS app required — works directly in Chrome.

🧠 How It Works
1. Audio Capture

The Nano 33 BLE Sense PDM mic records 1-second windows of audio.

2. ML Inference

Edge Impulse model processes:

MFE features

1D CNN classifier

Outputs probability:

Dog Chewing: 0.95
Noise: 0.05

3. BLE Notification

Arduino sends the chewing probability (0.0–1.0) over BLE.

4. Web App Logic

Threshold-based state machine:

0.90 for several cycles → DOG IS EATING

< 0.20 for several cycles → DOG FINISHED

Logs the timestamp

📁 Project Structure
smartdogbowl/
 ├── dog_eating_ui/               # Web Bluetooth UI
 ├── ei-smart-dog-bowl-2.0.zip    # Edge Impulse exported Arduino library
 ├── .gitignore
 └── README.md                    # (this file)

🔧 Requirements

Hardware:

Arduino Nano 33 BLE Sense

USB cable

(Optional) Dog bowl enclosure

Software:

Arduino IDE 2.x

Edge Impulse CLI (optional)

Chrome browser for Web Bluetooth

📡 Setup Instructions
🟦 1. Install Arduino Dependencies

Inside Arduino IDE:

Library Manager → Add .ZIP Library → ei-smart-dog-bowl-2.0.zip

🟧 2. Upload Firmware

Open the .ino file generated from Edge Impulse and upload to your board.

🟩 3. Run the Web App

Open:

dog_eating_ui/index.html


Click:

Connect → Select "DogEatingSound" → Start Monitoring

📊 Model Performance
Metric	Value
Accuracy	96.5%
Precision	0.93
Recall	0.96
F1 Score	0.95
AUC	0.50 (binary)

Confusion matrix (validation):

True Label	Chewing	Noise
Dog Chewing	100%	0%
Noise	100%	0%
📝 Notes & Limitations

Dataset uses ASMR chewing videos, not real dog recordings → not ideal

Real dog eating sounds vary in loudness and rhythm

Additional samples from your own dog would greatly improve accuracy

Model optimized (int8 quantized) for low RAM devices

📄 License

This project uses:

Edge Impulse code under Apache 2.0

Arduino core libraries under LGPL
