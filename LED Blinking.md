# Steps to build a LED Blinking Project
# =====================================================
## STEP 1: Create project directory
# =====================================================
mkdir ~/esp32_zephyr_led && cd ~/esp32_zephyr_led

# =====================================================
## STEP 2: Source Zephyr environment (REQUIRED every terminal)
# =====================================================  
source ~/zephyrproject/zephyr/zephyr-env.sh

# =====================================================
# #STEP 3: Create source directory and main.c (prints "LED ON")
# =====================================================
mkdir src
cat > src/main.c << 'EOF'
#include <zephyr/kernel.h>
#include <zephyr/device.h>
#include <zephyr/drivers/gpio.h>

#define LED_PIN 2  // ESP32 onboard LED

void main(void)
{
    const struct device *gpio_dev;
    int ret;
    
    // Get GPIO0 device (controls GPIO 0-31 on ESP32)
    gpio_dev = DEVICE_DT_GET(DT_NODELABEL(gpio0));
    
    if (!device_is_ready(gpio_dev)) {
        printk("Error: GPIO device not ready\n");
        return;
    }
    
    // Configure GPIO2 as output (ESP32 onboard LED)
    ret = gpio_pin_configure(gpio_dev, LED_PIN, GPIO_OUTPUT_ACTIVE);
    if (ret != 0) {
        printk("Error %d: failed to configure GPIO\n", ret);
        return;
    }
    
    // Blink forever - prints "LED ON" every second
    while (1) {
        printk("LED ON\n");
        gpio_pin_set(gpio_dev, LED_PIN, 1);
        k_msleep(1000);
        
        printk("LED OFF\n");
        gpio_pin_set(gpio_dev, LED_PIN, 0);
        k_msleep(1000);
    }
}
EOF

# =====================================================
## STEP 4: Create CMakeLists.txt (build configuration)
# =====================================================
cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.20.0)
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(led_blink)

target_sources(app PRIVATE src/main.c)
EOF

# =====================================================
# STEP 5: Create prj.conf (enable console + GPIO)
# =====================================================
cat > prj.conf << 'EOF'
CONFIG_CONSOLE=y
CONFIG_LOG=y
CONFIG_UART_CONSOLE=y
CONFIG_GPIO=y
CONFIG_LOG_DEFAULT_LEVEL=3
EOF

# =====================================================
# STEP 6: Install ESP32 flashing tool
# =====================================================
pip3 install esptool==5.0.2

# =====================================================
# STEP 7: Build for ESP32 (takes 2-3 minutes first time)
# =====================================================
west build -p auto -b esp32_devkitc/esp32/procpu .

# =====================================================
# STEP 8: Flash to ESP32 (hold BOOT button if needed)
# =====================================================
esptool.py --chip esp32 --port /dev/ttyACM0 --baud 460800 write_flash -z 0x1000 build/zephyr/zephyr.bin

# =====================================================
# STEP 9: Install + open serial monitor (SEE "LED ON")
# =====================================================
sudo apt install minicom -y
minicom -D /dev/ttyACM0 -b 115200
