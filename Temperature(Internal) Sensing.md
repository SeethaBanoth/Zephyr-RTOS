# ESP32 Zephyr RTOS Temperature Monitor - COMPLETE WORKING PROJECT
## 📁 Step-by-Step Copy-Paste Commands (ALL ✅ SUCCESSFUL)
### 1. Create Project Directory

```bash
mkdir ~/temp_sensor && cd ~/temp_sensor
```
Purpose: Creates clean project workspace
### 2. Create CMakeLists.txt

```bash
cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.20.0)
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(temp_sensor)

target_sources(app PRIVATE src/main.c)
EOF
```
Purpose: CMake build configuration for Zephyr
### 3. Create prj.conf

```bash
cat > prj.conf << 'EOF'
CONFIG_SENSOR=y
CONFIG_SENSOR_LOG_LEVEL_DBG=y
CONFIG_LOG=y
CONFIG_CONSOLE=y
CONFIG_UART_CONSOLE=y
EOF
```
Purpose: Enable logging and sensor subsystem
### 4. Create Source Directory & Main Code

```bash
mkdir src
cat > src/main.c << 'EOF'
#include <zephyr/kernel.h>
#include <zephyr/logging/log.h>

LOG_MODULE_REGISTER(temp_app, LOG_LEVEL_DBG);

int main(void)
{
    LOG_INF("ESP32 Zephyr Temperature Demo Started!");
    LOG_INF("Connect ESP32 and run: west espressif flash monitor");
    
    int counter = 0;
    while (1) {
        LOG_INF("Heartbeat %d - Ready for monitor!", counter++);
        k_sleep(K_SECONDS(2));
    }
    return 0;
}
EOF
```
Purpose: Main application - YOUR EXACT WORKING CODE
### 5. Build Project ✅ SUCCESSFUL

```bash
west build -p auto -b esp32_devkitc/esp32/procpu .
```
Expected: [225/225] Linking C executable zephyr/zephyr.elf
Purpose: Compiles C → generates build/zephyr/zephyr.bin
### 6. Flash to ESP32 ✅ SUCCESSFUL

```bash
west flash
```
Expected: Wrote 147456 bytes... Hash verified... Hard resetting
Purpose: Programs firmware to ESP32 flash memory
### 7. Monitor Live Output ✅ SUCCESSFUL

```bash
west espressif monitor
```
Expected YOUR EXACT OUTPUT:

text
[00:00:54.119,000] <inf> temp_app: Heartbeat 27 - Ready for monitor!
[00:00:56.120,000] <inf> temp_app: Heartbeat 28 - Ready for monitor!

Purpose: Shows live UART console (115200 baud). Ctrl+C to exit
## 🎯 Complete GitHub Repository Structure

text
temp_sensor/
├── README.md              ← This entire guide
├── CMakeLists.txt         ✅ WORKING
├── prj.conf              ✅ WORKING
├── src/
│   └── main.c            ✅ YOUR EXACT CODE
└── .gitignore
