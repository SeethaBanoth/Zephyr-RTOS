ESP32 Zephyr RTOS Temperature Sensing Project - Complete GitHub Ready Commands
📁 Project Structure

text
temp_sensor/
├── CMakeLists.txt
├── prj.conf
└── src/
    └── main.c

🚀 Complete Step-by-Step Commands (Copy-Paste Ready)
1. Create Project Directory

bash
mkdir ~/temp_sensor && cd ~/temp_sensor

Why: Creates isolated project workspace for standalone Zephyr application.
2. Create CMakeLists.txt

bash
vi CMakeLists.txt

Inside vi: i → Paste → Esc → :wq

text
cmake_minimum_required(VERSION 3.20.0)
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(temp_sensor)

target_sources(app PRIVATE src/main.c)

Why: CMake build configuration - tells Zephyr where source code lives.
3. Create prj.conf

bash
vi prj.conf

Inside vi: i → Paste → Esc → :wq

text
CONFIG_SENSOR=y
CONFIG_SENSOR_LOG_LEVEL_DBG=y
CONFIG_LOG=y
CONFIG_CONSOLE=y
CONFIG_UART_CONSOLE=y

Why: Kconfig settings - enables logging, console, sensor subsystem.
4. Create Source Directory & Main Code

bash
mkdir src
vi src/main.c

Inside vi: i → Paste → Esc → :wq

c
#include <zephyr/kernel.h>
#include <zephyr/logging/log.h>

LOG_MODULE_REGISTER(temp_app, LOG_LEVEL_DBG);

int main(void)
{
    LOG_INF("=== ESP32 Zephyr Temperature Demo Started ===");
    LOG_INF("Original ESP32: No built-in temp sensor");
    LOG_INF("Use ESP32-S3/C3 for real temperature sensor");
    
    uint32_t counter = 0;
    while (1) {
        LOG_INF("Heartbeat #%u - System uptime: %ums", 
                counter++, k_uptime_get_32());
        k_sleep(K_SECONDS(2));
    }
    return 0;
}

Why: Main application - logs heartbeat messages every 2 seconds (works on original ESP32).
5. Build Project

bash
west build -p auto -b esp32_devkitc/esp32/procpu .

Why: Compiles C code → generates build/zephyr/zephyr.bin firmware. /procpu specifies ESP32 PROcessor CPU target.
6. Flash to ESP32 (Connect ESP32 via USB first)

bash
west flash

Why: Programs firmware binary to ESP32 flash memory. Hold BOOT button if download fails.
7. Monitor Serial Output (Live temperature readings)

bash
west espressif flash monitor

Expected Output:

text
*** Booting Zephyr OS ***
[00:00:00.500] <inf> temp_app: === ESP32 Zephyr Temperature Demo Started ===
[00:00:02.501] <inf> temp_app: Heartbeat #0 - System uptime: 2501ms
[00:00:04.502] <inf> temp_app: Heartbeat #1 - System uptime: 4502ms

Why: Shows live UART console output (115200 baud). Ctrl+C to exit.
🔧 REAL Temperature Sensor (ESP32-S3/C3 only)

For actual temperature readings, change board target:

bash
west build -p auto -b xiao_esp32s3 .

OR

bash
west build -p auto -b esp32s3_devkitc .

Then use this main.c for real temp sensor:

c
#include <zephyr/kernel.h>
#include <zephyr/device.h>
#include <zephyr/drivers/sensor.h>
#include <zephyr/logging/log.h>

LOG_MODULE_REGISTER(temp_app, LOG_LEVEL_DBG);

int main(void)
{
    const struct device *temp_dev = DEVICE_DT_GET_ONE(espressif_esp32_temp);
    struct sensor_value temp_val;

    if (!device_is_ready(temp_dev)) {
        LOG_ERR("Temp sensor not ready");
        return -1;
    }

    LOG_INF("ESP32-S3 Temperature Sensor Started");

    while (1) {
        sensor_sample_fetch(temp_dev);
        sensor_channel_get(temp_dev, SENSOR_CHAN_DIE_TEMP, &temp_val);
        LOG_INF("CPU Temperature: %.1f°C", sensor_value_to_double(&temp_val));
        k_sleep(K_SECONDS(2));
    }
    return 0;
}

📋 Troubleshooting Commands

bash
# Check available ESP32 boards
west boards | grep esp32

# Clean build
rm -rf build

# Check serial ports
ls /dev/tty*

# Monitor specific port
west espressif flash monitor --esp-device /dev/ttyUSB0

✅ Success Criteria

    ✅ Build completes: [225/225] Linking C executable

    ✅ Flash completes: Flash write completed successfully

    ✅ Monitor shows: Heartbeat #X - System uptime: Yms
