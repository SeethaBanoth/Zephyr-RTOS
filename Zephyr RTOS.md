##### &#x20;                            **Zephyr RTOS Project**

###### **Abstract:**



This project presents the design and implementation of a BLE-based hardware tracking and proximity monitoring system using ESP32 microcontrollers and MQTT communication. The system is developed to detect and monitor the presence of specific Bluetooth Low Energy (BLE) beacons and estimate their proximity using Received Signal Strength Indicator (RSSI) values.



In this implementation, two ESP32 DevKit v1 boards are configured as BLE beacons that periodically broadcast advertising packets containing their device identity. A third ESP32 DevKit v1 board operates as a central scanner or base station, continuously scanning for nearby BLE advertisements. The base station filters and identifies only the predefined beacons using device name or MAC address and extracts the corresponding RSSI values to determine signal strength.



The scanned RSSI data is transmitted over a Wi-Fi network using the lightweight MQTT messaging protocol. The ESP32 base station acts as an MQTT publisher and sends real-time RSSI information to an MQTT broker. A Python-based client application subscribes to the MQTT topics and processes the incoming data, displaying the signal strength values in a structured table format for monitoring and analysis.



The system demonstrates an integrated approach combining embedded systems, wireless communication, IoT messaging protocols, and software-based data visualization. The use of BLE advertising allows low-power communication, while MQTT provides efficient and scalable data transfer between devices and applications.



This project highlights practical implementation of BLE scanning, RSSI-based proximity estimation, IoT communication architecture, and real-time data monitoring. The developed system can be extended for applications such as indoor asset tracking, smart warehouse management, proximity detection systems, and IoT-based monitoring solutions.   

&#x20;

###### **Brief Explanation:**



👉

“This project implements a wireless tracking system using ESP32 and MQTT.



Two ESP32 boards are configured as BLE beacons that continuously broadcast signals. Another ESP32 acts as a scanner, which detects these signals and measures their strength using RSSI to estimate proximity.



The scanner then sends this data over Wi-Fi to an MQTT broker. A Python-based application subscribes to the MQTT topic, receives the data, and displays it in real time.



👉 Final result: The system allows real-time tracking of devices and estimation of how close they are based on signal strength.”



###### **Detailed Explanation:**



**1. System Overview:**



Your system consists of 4 main parts (not just 3 — include Python client also):



Beacon ESP32s → Scanner ESP32 → MQTT Broker → Python Client



🔹 1. Beacon ESP32s (Transmitter)



Uses ESP32

Configured as BLE beacons

Continuously broadcast BLE signals (advertising packets)

What they do:

Send device identity (name / MAC)

Do NOT receive data

Work in low power mode



👉 Role: Signal source (tracking device)



🔹 2. Scanner ESP32 (Receiver + Processor)



Another ESP32

Works as BLE scanner + Wi-Fi device

What it does:

Scans nearby BLE devices

Filters only your beacons

Reads RSSI (signal strength)

Estimates proximity

Sends data using MQTT



👉 Role: Brain of the system



🔹 3. MQTT Broker (Communication Hub)



Central server for data exchange

Example: Mosquitto

What it does:

Receives data from scanner (publisher)

Sends data to clients (subscribers)



👉 Role: Message manager



🔹 4. Python Client (Monitoring System)



Runs on laptop/PC

Uses Python MQTT library

What it does:

Subscribes to MQTT topic

Receives RSSI data

Displays in table format



Example:



Device      RSSI

Beacon\_1    -60

Beacon\_2    -75



👉 Role: User interface / visualization



✅ Complete Data Flow

Step 1: Beacon → broadcasts BLE signal  

Step 2: Scanner → detects signal  

Step 3: Scanner → measures RSSI  

Step 4: Scanner → sends data via MQTT  

Step 5: Broker → forwards message  

Step 6: Python → displays data  





**2. BLE Beacon (Transmitter):**



In your project, the BLE beacon is implemented using ESP32.



🔹 What is a BLE Beacon?



A BLE beacon is a device that:



👉 Continuously broadcasts small data packets using Bluetooth Low Energy (BLE)



It does not establish a connection

It simply advertises its presence

🔹 Your Implementation

You used 2 ESP32 boards

Configured them as BLE beacons

They send signals at regular intervals (e.g., every few milliseconds)

🔹 What is an Advertising Packet?



An advertising packet is a small data message broadcast over BLE.



It contains:



Device Name / ID

MAC Address

Optional data (UUID, Tx power, etc.)



Example:



Device: Beacon\_1

MAC: 24:6F:28:AA:12:34



🔹 Key Behavior



👉 As you said (correct 👍):



Beacons only transmit

They do not receive or process incoming data



This is called:



👉 Connectionless communication



🔹 Why Only Transmit?



Because:



Reduces power consumption

Simplifies design

Faster broadcasting

No pairing or connection required



🔹 How It Works in Your Project



Beacon ESP32

&#x20;    ↓

Broadcast BLE signal (advertising)

&#x20;    ↓

Scanner ESP32 detects it



🔹 Important Characteristics



✔ Low power

✔ Short range (5–30 meters)

✔ Small data packets

✔ Continuous broadcasting





**3. BLE Scanner (Receiver):**



In your project, the scanner is implemented using an ESP32.



🔹 What is a BLE Scanner?



A BLE scanner is a device that:



👉 Continuously listens for BLE advertising signals from nearby devices



Unlike beacons:



Beacon → Transmits

Scanner → Receives + Processes



🔹 Your Implementation

You used one ESP32

Configured it as a central scanner

It runs in continuous scanning mode



🔹 How It Works

Beacon → broadcasts signal  

Scanner → detects signal  

Scanner → reads data (RSSI, MAC, name)



🔹 Filtering Mechanism (Very Important ⭐)



The scanner does not process all devices.



It filters only your specific beacons using:



✔ Device Name



Example:



Beacon\_1

Beacon\_2

✔ MAC Address



Example:



24:6F:28:AA:12:34



👉 This ensures:



No unwanted devices are considered

Accurate tracking

Reduced processing load



🔹 What Data Scanner Extracts



From each detected beacon, the scanner reads:



Device name

MAC address

RSSI (signal strength)



Example:



Beacon\_1 → RSSI: -60 dBm

Beacon\_2 → RSSI: -75 dBm



🔹 Role of RSSI



RSSI helps estimate proximity:



\-50 dBm → very close  

\-70 dBm → moderate distance  

\-90 dBm → far away  



👉 This is the core of your proximity monitoring system



🔹 What Happens After Scanning?



After processing:



👉 Scanner sends data using MQTT



Scanner → Wi-Fi → MQTT Broker → Python Client



🔹 Key Responsibilities of Scanner



✔ Detect BLE signals

✔ Filter required devices

✔ Measure RSSI

✔ Estimate proximity

✔ Send data to cloud/system



👉 It acts as the brain of your system





**4. RSSI-Based Proximity Detection:**



🔹 What is RSSI?



RSSI (Received Signal Strength Indicator) is:



👉 A measure of how strong a received wireless signal is



Unit: dBm (decibels relative to 1 milliwatt)

It is always a negative value



Example:



\-40 dBm → very strong signal  

\-60 dBm → medium signal  

\-90 dBm → very weak signal  



🔹 Basic Idea



👉 The closer the device, the stronger the signal



High RSSI (less negative)  → closer  

Low RSSI (more negative) → farther  



✔ Your statement is correct:



\-50 dBm → closer  

\-80 dBm → farther  



🔹 How Your System Uses RSSI



In your project using ESP32:



Beacon sends BLE signal

Scanner receives it

Scanner reads RSSI value

Uses it to estimate proximity



🔹 Mathematical Relation (Important ⭐)



Distance is estimated using this formula:



𝑑 = 10(𝑇𝑥𝑃𝑜𝑤𝑒𝑟 − 𝑅𝑆𝑆𝐼)

&#x20;      \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

&#x20;         10𝑛



Where:



d = distance

TxPower = signal strength at 1 meter

RSSI = measured signal strength

n = environmental factor (usually 2–4)



🔹 Simple Understanding of Formula

If RSSI decreases, distance increases

If RSSI increases, distance decreases



Example:



RSSI	Distance

\-50 dBm	Very close (\~1–2 m)

\-70 dBm	Medium (\~5 m)

\-90 dBm	Far (\~10+ m)



🔹 Why It Is “Indirect Measurement”



👉 Because:



RSSI does not give exact distance

It only gives an approximation



🔹 Factors Affecting RSSI (Very Important ⭐)



RSSI is not always accurate due to:



Walls and obstacles

Human body interference

Signal reflections

Noise and interference



👉 So distance estimation is approximate, not exact



🔹 Why We Still Use RSSI?



Because it is:



✔ Simple

✔ No extra hardware required

✔ Works with BLE directly

✔ Low cost





**5. MQTT Communication:**



In your project, communication between devices is done using

MQTT.



🔹 What is MQTT?



MQTT is a publish–subscribe based communication protocol used in IoT.



👉 Instead of direct communication:



Device → Server → Client



It works like:



Publisher → Broker → Subscriber

🔹 Components in Your Project



1️⃣ Publisher (Scanner ESP32)



The ESP32 scanner acts as a publisher

It sends RSSI data



Example:



Topic: ble/rssi

Message: Beacon\_1 → -60 dBm



2️⃣ Broker (Server)



Acts as a middleman

Example: Mosquitto



👉 Responsibilities:



Receives data from publisher

Sends data to all subscribers



3️⃣ Subscriber (Python Client)



Your Python program acts as subscriber

It listens to the topic:

ble/rssi

Displays data in real-time



🔹 Data Flow in Your System



Step 1: Scanner reads RSSI  

Step 2: Scanner publishes data  

Step 3: MQTT broker receives it  

Step 4: Python client subscribes  

Step 5: Data displayed on screen  



🔹 Why MQTT is Used (Very Important ⭐)



✔ Lightweight

Uses very small data packets

Perfect for embedded systems

✔ Efficient

Low bandwidth usage

✔ Scalable

Many devices can communicate easily

✔ Decoupled Communication

Publisher and subscriber don’t know each other



🔹 Example from Your Project



Topic: ble/rssi



Messages:

Beacon\_1 → -58 dBm

Beacon\_2 → -72 dBm



👉 Python client receives and prints:



Device      RSSI

Beacon\_1    -58

Beacon\_2    -72



**6. Python Monitoring Application:**



In your project, the monitoring system is implemented using Python.



🔹 What is the Role of Python?



👉 Python acts as an MQTT subscriber.



It connects to the MQTT broker

Subscribes to a topic (e.g., ble/rssi)

Receives real-time data from the scanner ESP32



🔹 How It Works



Scanner ESP32 → MQTT Broker → Python Client

Step-by-step:

Scanner publishes RSSI data

MQTT broker receives it

Python subscribes to the topic

Python receives the message

Displays output on screen



🔹 Example Data Flow



Topic: ble/rssi

Message: Beacon\_1 → -58 dBm



Python receives and prints:



Device      RSSI

Beacon\_1    -58

Beacon\_2    -72



🔹 What Python Program Does



✔ Connects to MQTT broker

✔ Subscribes to topic

✔ Receives messages continuously

✔ Parses data

✔ Displays in structured format



🔹 Why Python is Used



✔ Easy to implement

Simple syntax

Fast development

✔ MQTT support

Libraries like paho-mqtt

✔ Data visualization

Can extend to graphs, dashboards



🔹 Importance in Your Project



👉 Without Python:



Data is only inside ESP32

No user visibility



👉 With Python:



You get real-time monitoring

Easy to analyze signal strength





**7. Technologies Used:**



Your project integrates multiple domains, making it a complete IoT system.



🔹 1. Embedded Systems



You programmed the ESP32

It acts as:

BLE beacon (transmitter)

BLE scanner (receiver + processor)



👉 This is the core hardware platform



🔹 2. Wireless Communication (BLE)



Uses Bluetooth Low Energy

Enables:

Beacon broadcasting

Scanner detection



👉 Provides low-power short-range communication



🔹 3. Networking (Wi-Fi)



ESP32 connects to Wi-Fi network

Used to send data from scanner to broker



👉 Enables internet/cloud communication



🔹 4. IoT Communication Protocol



Uses MQTT

Handles:

Data transmission

Publisher–subscriber model



👉 Ensures efficient and real-time data transfer



🔹 5. Software (Python Application)



Implemented using Python

Used for:

Receiving MQTT data

Displaying RSSI values



👉 Provides user interface and monitoring



✅ How All Technologies Work Together



BLE (Beacon) → ESP32 Scanner → Wi-Fi → MQTT → Python Application



👉 Each technology handles a specific layer:



Layer	Technology

Hardware	ESP32

Communication	BLE

Network	Wi-Fi

Protocol	MQTT

Application	Python



✅ Why This is a Complete IoT System



Your project includes all IoT layers:



✔ Sensing (BLE beacons)

✔ Processing (ESP32 scanner)

✔ Communication (Wi-Fi + MQTT)

✔ Application (Python UI)



👉 That’s why it is called a full-stack IoT system





**8. Advantages of Your System:**



Your BLE-based tracking system has several important advantages:



🔹 1. Low Power Consumption



Uses Bluetooth Low Energy

BLE is designed for minimal power usage



👉 Benefit:



Beacons can run longer on battery

Suitable for continuous monitoring systems



🔹 2. Real-Time Monitoring



Scanner continuously detects beacons

Data is instantly sent via MQTT



👉 Benefit:



You get live updates of device location/proximity

Useful in tracking and monitoring applications



🔹 3. Scalability



You can easily add more beacons

Scanner can detect multiple devices simultaneously



👉 Benefit:



System can expand without major changes

Suitable for large environments (warehouse, office, etc.)



🔹 4. Wireless Communication



No cables required

Uses BLE + Wi-Fi



👉 Benefit:



Easy installation

Flexible deployment

Works in dynamic environments



🔹 5. Lightweight Communication (MQTT)



Uses MQTT

Sends small data packets



👉 Benefit:



Low bandwidth usage

Faster communication

Efficient for embedded systems



✅ Additional Advantages (Good to Mention ⭐)



You can also add these if needed:



🔹 6. Cost-Effective



Uses low-cost ESP32



🔹 7. Easy Integration



Can be integrated with cloud, dashboards, mobile apps



🔹 8. Modular Design



Features can be added/removed easily (especially using Zephyr RTOS)





**9. Applications of Your System:**



Your BLE-based tracking system can be used in many real-world scenarios:



🔹 1. Asset Tracking 📦



Attach BLE beacons to assets (devices, packages, tools)

Scanner detects their presence and proximity



👉 Example:



Tracking laptops, equipment, or parcels in an office or lab



👉 Benefit:



Prevents loss and improves asset management



🔹 2. Smart Warehouses 🏭



Beacons attached to goods or containers

Scanner monitors their movement



👉 Example:



Detect if a package enters or leaves a zone



👉 Benefit:



Improves inventory tracking

Reduces manual work



🔹 3. Office Tracking Systems 🏢



Track movement of employees or devices

Monitor entry/exit in specific areas



👉 Example:



Attendance or workspace monitoring



👉 Benefit:



Better resource utilization and security



🔹 4. Hospital Equipment Tracking 🏥



Attach beacons to medical equipment

Easily locate devices like wheelchairs, monitors, etc.



👉 Example:



Find nearest available equipment quickly



👉 Benefit:



Saves time in emergencies

Improves hospital efficiency



🔹 5. Indoor Positioning Systems 📍



Helps estimate location inside buildings

Works where GPS is not available



👉 Example:



Navigation inside malls, airports, offices



👉 Benefit:



Provides location-based services



✅ Why Your System is Suitable for These Applications



Because it provides:



✔ Real-time monitoring

✔ Low power operation

✔ Wireless communication

✔ Scalable architecture

