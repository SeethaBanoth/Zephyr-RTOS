# Zephyr RTOS Complete Installation Guide - Ubuntu 20.04 LTS ✅

**Feb 11, 2026 | Ubuntu 20.04 LTS (Noble) | 100% Working**

---

## 🧹 Step 0: Update your system
```bash
sudo apt update
sudo apt upgrade -y
```
Updates package lists and upgrades all system packages to latest versions. Ensures compatibility with Zephyr dependencies.
## 🛠️ Step 1: Install required system packages

```bash
sudo apt install -y \
  git cmake ninja-build gperf ccache \
  dfu-util device-tree-compiler \
  python3-venv python3-pip python3-setuptools \
  wget unzip xz-utils file make \
  gcc g++ \
  libssl-dev openssl \
  flex bison \
  openocd libsdl2-dev libmagic1
```
Installs all mandatory build tools, Python dependencies, device flashing utilities, and QEMU support required for Zephyr development.
## 📁 Step 2: Create Zephyr workspace

```bash
mkdir -p ~/zephyrproject
cd ~/zephyrproject

python3 -m venv zephyr-venv
source zephyr-venv/bin/activate
```
Creates dedicated workspace directory and isolated Python virtual environment to avoid conflicts with system packages.
## 📦 Step 3: Upgrade pip in virtual environment

```bash
pip install --upgrade pip setuptools wheel
```
Updates Python packaging tools inside virtual environment for compatibility with latest west and Zephyr requirements.
## 🔧 Step 4: Install west (Zephyr meta-tool)

```bash
pip install west
```
West is Zephyr's official command-line tool that manages multi-repository workspaces, builds, flashing, and debugging.
## 📥 Step 5: Initialize Zephyr workspace

```bash
west init
```
Creates .west/ configuration directory and downloads Zephyr manifest repository (813MB). Prepares structure for 100+ modules.
## ⬇️ Step 6: Download all Zephyr modules

```bash
west update
```
Clones complete Zephyr ecosystem - main repo + 100+ modules, HAL drivers, libraries (2GB+ download). ⚠️ Keep laptop awake 15-20 mins.
## 🛠️ Step 7: Setup Zephyr SDK 0.17.4

```bash
cd ~
wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.17.4/zephyr-sdk-0.17.4_linux-x86_64.tar.xz
tar xvf zephyr-sdk-0.17.4_linux-x86_64.tar.xz
cd zephyr-sdk-0.17.4
./setup.sh
```
Downloads and installs cross-compilation toolchains (ARM, RISC-V, Xtensa). Answer y to all prompts during setup.sh.
## 🧪 Step 8: Test Hello World build

```bash
cd ~/zephyrproject
source zephyr/zephyr-env.sh
west build -b qemu_x86 zephyr/samples/hello_world
```
Compiles Hello World sample for QEMU x86 emulator. Creates build/zephyr/zephyr.elf (467KB). Tests complete toolchain.
## ▶️ Step 9: Run Hello World in QEMU

```bash
qemu-system-x86_64 -kernel build/zephyr/zephyr.elf -serial mon:stdio -display none
```
Boots Zephyr in x86 emulator. Expected output: "*** Booting Zephyr OS *** Hello World! qemu_x86". Exit: Ctrl+C.
## ⚙️ Step 10: Permanent environment setup

```bash
echo 'alias zbuild="cd ~/zephyrproject && source zephyr-venv/bin/activate && source zephyr/zephyr-env.sh"' >> ~/.bashrc
source ~/.bashrc
```
Creates zbuild alias for instant Zephyr environment access. Run zbuild daily to start development.
## ✅ Step 11: Verify installation success

```bash
zbuild
west top
ls build/zephyr/zephyr.elf
```
Confirms workspace, build artifacts, and environment are working correctly.
## 🚀 Daily Development Commands

```bash
zbuild                           # Start Zephyr environment
west build -b qemu_x86 my_app    # Build your app
qemu-system-x86_64 -kernel build/zephyr/zephyr.elf -serial mon:stdio -display none  # Test
```
## 📊 Installation Summary
|Step|	Size	|Time|
|:---|:---:|:---:|
|System packages	|500MB	|5 min|
|Workspace + modules	|3GB	|20 min|
|SDK 0.17.4	|1GB	|3 min|
|TOTAL	|5GB	|30 min|


