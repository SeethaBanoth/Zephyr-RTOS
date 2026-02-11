# Steps to build a LED Blinking Project
═══════════════════════════════════════════════════════════════════════════════
## STEP 1: Create & enter project directory
═══════════════════════════════════════════════════════════════════════════════
```bash
mkdir ~/esp32_zephyr_led && cd ~/esp32_zephyr_led
```
 ↑ Creates CLEAN project folder in home directory (~/) - keeps everything organized
 ↑ cd enters the folder - all future commands work here

═══════════════════════════════════════════════════════════════════════════════
## STEP 2: Activate Zephyr environment (REQUIRED every new terminal)
═══════════════════════════════════════════════════════════════════════════════
```bash
source ~/zephyrproject/zephyr/zephyr-env.sh
```
 ↑ "sources" (runs) Zephyr's setup script
 ↑ Sets 100+ environment variables (toolchain, paths, libraries)
 ↑ WITHOUT this → west, cmake, compiler all fail
 ↑ MUST run every new terminal session

═══════════════════════════════════════════════════════════════════════════════
## STEP 3: Create main.c - YOUR LED BLINK CODE
═══════════════════════════════════════════════════════════════════════════════
```bash
mkdir src && cat > src/main.c << 'EOF'

#include <zephyr/kernel.h>      // Zephyr RTOS kernel (sleep, threads)
#include <zephyr/device.h>      // Device management
#include <zephyr/drivers/gpio.h> // GPIO driver API

#define LED_PIN 2  // ESP32 onboard LED pin

void main(void)
{
    const struct device *gpio_dev;  // GPIO controller pointer
    int ret;                        // Return value holder
    
    // Get GPIO0 controller (ESP32 GPIO0-31)
    gpio_dev = DEVICE_DT_GET(DT_NODELABEL(gpio0));
    // ↑ Device Tree gets gpio0 hardware block from ESP32
    
    if (!device_is_ready(gpio_dev)) {
        printk("Error: GPIO device not ready\n");
        return;
    }
    // ↑ Safety check - ensures hardware is accessible
    
    // Configure GPIO2 as OUTPUT (active high)
    ret = gpio_pin_configure(gpio_dev, LED_PIN, GPIO_OUTPUT_ACTIVE);
    if (ret != 0) {
        printk("Error %d: failed to configure GPIO\n", ret);
        return;
    }
    // ↑ Sets GPIO2 direction=OUTPUT, starts HIGH (LED ON)
    
    // INFINITE BLINK LOOP
    while (1) {
        printk("LED ON\n");           // Print to UART console
        gpio_pin_set(gpio_dev, LED_PIN, 1);  // LED ON
        k_msleep(1000);               // Sleep 1 second (kernel delay)
        
        printk("LED OFF\n");          // Print to UART console  
        gpio_pin_set(gpio_dev, LED_PIN, 0);  // LED OFF
        k_msleep(1000);               // Sleep 1 second
    }
}
EOF
```
 ↑ src/main.c = YOUR APPLICATION CODE
 ↑ GPIO2 = standard ESP32 onboard LED pin
 ↑ printk() = Zephyr's printf() to UART serial
 ↑ k_msleep() = Zephyr kernel sleep (non-blocking)

═══════════════════════════════════════════════════════════════════════════════
## STEP 4: CMakeLists.txt - BUILD SYSTEM CONFIG
═══════════════════════════════════════════════════════════════════════════════
```bash
cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.20.0)  # Minimum CMake version
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})  # Find Zephyr installation
project(led_blink)                    # Project name

target_sources(app PRIVATE src/main.c)  # Add main.c to app target
EOF
```
 ↑ REQUIRED Zephyr file - tells build system what to compile
 ↑ find_package(Zephyr) = links 1000+ Zephyr libraries
 ↑ target_sources = "compile src/main.c into final binary"

═══════════════════════════════════════════════════════════════════════════════
## STEP 5: prj.conf - ZEPHYR CONFIGURATION (ENABLERS)
═══════════════════════════════════════════════════════════════════════════════
```bash
cat > prj.conf << 'EOF'
CONFIG_CONSOLE=y           # Enable console subsystem
CONFIG_LOG=y               # Enable logging system  
CONFIG_UART_CONSOLE=y      # UART → serial output (your terminal)
CONFIG_GPIO=y              # GPIO driver (LED control)
CONFIG_LOG_DEFAULT_LEVEL=3 # INFO level logging
EOF
```
 ↑ Kconfig settings - enables hardware drivers
 ↑ WITHOUT CONFIG_UART_CONSOLE=y → no "LED ON" prints!
 ↑ Level 3 = INFO + WARNING + ERROR messages

═══════════════════════════════════════════════════════════════════════════════
## STEP 6: Install ESP32 flashing tool
═══════════════════════════════════════════════════════════════════════════════
```bash
pip3 install esptool==5.0.2
```
 ↑ Espressif's official ESP32 flashing utility
 ↑ Version 5.0.2 = Zephyr 4.x compatible
 ↑ Required for Step 8 flashing

═══════════════════════════════════════════════════════════════════════════════
## STEP 7: BUILD - C → BINARY FIRMWARE
═══════════════════════════════════════════════════════════════════════════════
```bash
west build -p auto -b esp32_devkitc/esp32/procpu .
```
 ↑ west = Zephyr's build tool (like make)
 ↑ -p auto = parallel build (fast)
 ↑ -b esp32_devkitc/esp32/procpu = ESP32 PRO CPU target  
 ↑ . = build current directory
 ↑ Creates: build/zephyr/zephyr.bin (your firmware!)

═══════════════════════════════════════════════════════════════════════════════
## STEP 8: FLASH - Send firmware to ESP32
═══════════════════════════════════════════════════════════════════════════════
```bash
esptool.py --chip esp32 --port /dev/ttyACM0 --baud 460800 write_flash -z 0x1000 build/zephyr/zephyr.bin
```
 ↑ --chip esp32 = target ESP32 chip
 ↑ --port /dev/ttyACM0 = your USB serial port  
 ↑ --baud 460800 = fast flashing speed
 ↑ write_flash = write binary to flash memory
 ↑ -z = erase before write
 ↑ 0x1000 = flash address (Zephyr standard)
 ↑ build/zephyr/zephyr.bin = your compiled firmware

═══════════════════════════════════════════════════════════════════════════════
## STEP 9: SERIAL MONITOR - SEE "LED ON"
═══════════════════════════════════════════════════════════════════════════════
```bash
sudo apt install minicom -y    # Install serial terminal
minicom -D /dev/ttyACM0 -b 115200  # Open serial monitor
EOF
```
## 🎯 Expected Serial Output:

text
*** Booting Zephyr OS ***
LED ON
LED OFF
LED ON
LED OFF
LED ON
...

## ✅ Visual Results:

text
🔴🟢 LED blinks every 2 seconds (GPIO2 onboard)
📱 "LED ON" prints every second to terminal
⚙️ Zephyr RTOS kernel running
💾 148KB firmware (3.5% of 4MB flash)

🚪 EXIT minicom: Ctrl+A → X → Enter
## 📁 Final Directory Structure:

text
~/esp32_zephyr_led/
├── src/
│   └── main.c          # YOUR blink code
├── CMakeLists.txt      # Build config
├── prj.conf           # Zephyr settings
└── build/             # Compiled firmware
    └── zephyr.bin     # Flash this file!


