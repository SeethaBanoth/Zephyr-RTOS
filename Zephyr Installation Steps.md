# Zephyr RTOS Complete Installation Guide - Ubuntu 20.04 ✅ WITH COMMAND EXPLANATIONS

**Feb 11, 2026 | REAL SESSION | 100% WORKING | Copy-Paste Ready**
**Author**: seetha@Seetha | **VM**: Ubuntu 20.04 VirtualBox | **Status**: ✅ FULLY OPERATIONAL

## 🎯 EXECUTIVE SUMMARY

## 📋 COMPLETE STEP-BY-STEP COMMANDS WITH DETAILED EXPLANATIONS

### 1. **CLEAN START** - Remove broken workspace
```bash
cd ~
mv zephyrproject zephyrproject_broken 2>/dev/null || true
west init zephyrproject
cd ~/zephyrproject
Cloning into '/home/seetha/zephyrproject/.west/manifest-tmp'...
Receiving objects: 100% (1438330/1438330), 813.24 MiB ✅
west update
Scheduling all modules
--- zephyr - Downloading zephyr branch main*** Booting Zephyr OS build v4.3.0 ***
Hello World! qemu_x86/atom ✅
[Infinite loop = NORMAL - Zephyr is running!]
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
echo 'alias zbuild="cd ~/zephyrproject && source zephyr/zephyr-env.sh"' >> ~/.bashrc
source ~/.bashrc

HEAD is now at 9164bd1 tests: Format platform... ✅
  cd ~/zephyr-sdk-0.17.4
./setup.sh
Installing 'xtensa-nxp_rt600_adsp_zephyr-elf' toolchain ... 100%
All done. ✅
cd ~/zephyrproject
source zephyr/zephyr-env.sh
west build -b qemu_x86 zephyr/samples/hello_world
1. CMake generates Ninja build files
2. Parses Kconfig → Creates `.config`
3. Compiles 131 source files → Links zephyr.elf
4. Device tree generation (DTS → C headers)
cd ~/zephyrproject
source zephyr/zephyr-env.sh
qemu-system-x86_64 -kernel build/zephyr/zephyr.elf -serial mon:stdio -display none
