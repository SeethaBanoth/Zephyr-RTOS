# Zephyr RTOS Complete Installation Guide - Ubuntu 20.04 ✅ PERFECT FORMAT

**Feb 11, 2026 | REAL SESSION | 100% WORKING | Copy-Paste Ready**  
**Author**: seetha@Seetha | **VM**: Ubuntu 20.04 VirtualBox | **Status**: ✅ FULLY OPERATIONAL

---

## 🎯 EXECUTIVE SUMMARY

✅ Workspace: ~/zephyrproject (5GB) - COMPLETED ✓
✅ SDK: 0.17.4 (All toolchains) - COMPLETED ✓
✅ Hello World: Boots in QEMU - COMPLETED ✓
✅ Total time: 30 minutes - COMPLETED ✓
✅ Disk usage: ~5GB - COMPLETED ✓

text

---

## 📋 STEP-BY-STEP INSTALLATION - Commands + Explanations

### 1. CLEAN START - Remove broken workspace
```bash
cd ~
mv zephyrproject zephyrproject_broken 2>/dev/null || true

EXPLANATION: Navigates to home directory and safely renames any existing broken zephyrproject folder to zephyrproject_broken backup. 2>/dev/null || true suppresses errors if folder doesn't exist. Provides clean slate for fresh installation.
2. WORKSPACE INITIALIZATION - Setup west structure

bash
west init zephyrproject
cd ~/zephyrproject

EXPLANATION: west init creates .west/ configuration directory and downloads Zephyr's main manifest repository (813MB). cd ~/zephyrproject enters the newly created workspace. Prepares structure for 100+ Zephyr modules.
3. DOWNLOAD COMPLETE ECOSYSTEM - Fetch all modules

bash
west update

EXPLANATION: Parses manifest file and clones 100+ Git repositories including zephyr/, modules/, hardware abstraction layers, libraries, and tools. Downloads ~2GB+ source code. ⚠️ CRITICAL: KEEP LAPTOP AWAKE - 15-20 minutes duration.
4. INSTALL CROSS-COMPILATION TOOLCHAINS - Zephyr SDK

bash
cd ~/zephyr-sdk-0.17.4
./setup.sh

EXPLANATION: Runs interactive setup script. Answer y to all three prompts:

    Install toolchains for all targets? y

    Install host tools? y

    Register CMake package? y
    Installs ARM, RISC-V, Xtensa, x86_64 compilers. Registers SDK with CMake build system.

5. BUILD HELLO WORLD - Test complete installation

bash
cd ~/zephyrproject
source zephyr/zephyr-env.sh
west build -b qemu_x86 zephyr/samples/hello_world

EXPLANATION:

    source zephyr/zephyr-env.sh sets environment variables (PATH, ZEPHYR_BASE, CROSS_COMPILE)

    west build -b qemu_x86 targets QEMU x86 emulator board

    zephyr/samples/hello_world is correct path (not samples/hello_world)
    Compiles 131 C files → Creates build/zephyr/zephyr.elf (467KB).

6. RUN ZEPHYR IN EMULATOR - Boot Hello World

bash
cd ~/zephyrproject
source zephyr/zephyr-env.sh
qemu-system-x86_64 -kernel build/zephyr/zephyr.elf -serial mon:stdio -display none

EXPLANATION:

    qemu-system-x86_64 launches x86 emulator

    -kernel build/zephyr/zephyr.elf loads Zephyr binary

    -serial mon:stdio redirects UART output to terminal

    -display none runs headless (no graphics window)
    Expected: *** Booting Zephyr OS *** Hello World! qemu_x86/atom Exit: Ctrl+C

7. PERMANENT ENVIRONMENT - Daily development setup

bash
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
echo 'alias zbuild="cd ~/zephyrproject && source zephyr/zephyr-env.sh"' >> ~/.bashrc
source ~/.bashrc

EXPLANATION: Permanently adds west to PATH and creates zbuild alias for instant Zephyr environment access. source ~/.bashrc reloads configuration.
✅ PROOF OF SUCCESS - Your Real Results

text
$ ls -la build/zephyr/zephyr.elf
-rwxrwxr-x 1 seetha seetha 467264 Feb 11 13:22 build/zephyr/zephyr.elf ✅

$ qemu-system-x86_64 -kernel build/zephyr/zephyr.elf ...
*** Booting Zephyr OS build v4.3.0-5917-g674a6a1bc86a ***
Hello World! qemu_x86/atom ✅

🚀 DAILY DEVELOPMENT WORKFLOW

bash
zbuild                                    # Enter Zephyr environment (1 command)
west build -b qemu_x86 my_app             # Build and test in emulator
qemu-system-x86_64 -kernel build/zephyr/zephyr.elf -serial mon:stdio -display none  # Run

🔍 QUICK VERIFICATION COMMANDS

bash
west top                          # /home/seetha/zephyrproject ✅
ls -la .west/                    # config/ install.cmd ✅
ls build/zephyr/zephyr.elf       # 467KB executable ✅
zbuild && qemu-system-x86_64 -kernel build/zephyr/zephyr.elf -serial mon:stdio -display none  # Hello World! ✅

📊 SYSTEM RESOURCES REQUIRED
Component	Disk Space	RAM	Time
west init	813MB	1GB	1 min
west update	2GB+	4GB	15-20 min
SDK 0.17.4	1GB	2GB	2 min
Build output	500MB	4GB	2 min
TOTAL	~5GB	4GB	30 min
⚠️ REAL ERRORS + SOLUTIONS (From Your Session)
❌ ERROR MESSAGE	✅ CORRECT COMMAND	FIX EXPLANATION
west: unknown command "top"	west init zephyrproject	Missing .west/ workspace
samples/hello_world does not exist	zephyr/samples/hello_world	Samples inside zephyr/ module
No board named 'qemux86'	qemu_x86	Correct QEMU board name
west: unknown command "run"	qemu-system-x86_64 ...	Use QEMU directly
🎉 SUCCESS CHECKLIST - ALL VERIFIED

text
✅ [x] Workspace created: ~/zephyrproject
✅ [x] West manifest: 813MB downloaded  
✅ [x] All modules: west update complete
✅ [x] SDK 0.17.4: All toolchains installed
✅ [x] Build success: zephyr.elf (467KB)
✅ [x] QEMU working: Hello World boots
✅ [x] Daily aliases: zbuild ready

📱 NEXT STEPS - Real Hardware Development

bash
west boards | grep nrf52          # Nordic nRF52 series
west boards | grep stm32          # STM32 microcontrollers
west boards | grep esp32          # Espressif ESP32
zbuild                           # Start your project!

