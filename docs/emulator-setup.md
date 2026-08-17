# Rooted Android Emulator Setup Guide

This document collects practical ways to obtain root on Android emulators so we can access the underlying Linux environment.

## 1. Android Studio AVD + Google APIs Image (Simplest)

1. Open Android Studio → Device Manager → Create Device.
2. Choose a device (Pixel recommended).
3. When selecting system image:
   - Prefer images labeled **Google APIs** (not "Google Play").
   - Google Play images intentionally restrict `adbd` from running as root.
4. Create and launch the AVD.
5. From host terminal:
   ```bash
   adb devices
   adb root          # should say "restarting adbd as root"
   adb shell
   id                # uid=0(root)
   ```
6. For writable system partition:
   ```bash
   adb remount
   ```

If you need Google Play Store + root, use the methods below.

## 2. rootAVD + Magisk (Works on Play Store Images)

[rootAVD](https://github.com/newbit1/rootAVD) is currently one of the most reliable tools.

```bash
git clone https://github.com/newbit1/rootAVD.git
cd rootAVD

# List available images / running AVDs
./rootAVD.sh list          # Linux/macOS
# or rootAVD.bat list      # Windows

# Root a specific ramdisk (example path — adjust to your SDK)
./rootAVD.sh system-images/android-34/google_apis_playstore/x86_64/ramdisk.img
```

The script installs Magisk. After reboot (cold boot recommended):

```bash
adb shell
su
# Magisk prompt appears on the emulator — grant it
id
```

You now have Magisk Manager on the device and full root.

## 3. AERoot (On-the-fly Root for Google Play AVDs)

[AERoot](https://github.com/quarkslab/AERoot) uses GDB to temporarily elevate privileges without permanently modifying the image.

```bash
pip install aeroot

# Start emulator with GDB server
emulator @Your_AVD -qemu -s

# In another terminal
aeroot daemon     # makes adbd root so new shells are rooted
# or aeroot name <process>
# or aeroot pid <pid>
```

Useful for temporary experiments.

## 4. Waydroid (Linux Host Preferred)

Waydroid runs Android in an LXC container sharing the host kernel — excellent performance and easy root access on many setups.

```bash
# Install (Debian/Ubuntu example)
sudo apt install waydroid
sudo waydroid init
sudo waydroid session start

# Root is often available depending on image; many community images ship with Magisk or su.
```

See official docs: https://waydro.id

## 5. Verifying Linux Access

Once rooted:

```bash
adb shell
su

# Kernel version
uname -a

# Processes
ps -A | head

# Mount points / filesystems
mount | head

# /proc and /sys are fully readable/writable (with care)
ls /proc
cat /proc/cpuinfo

# You can even chroot into a full Linux userspace if desired
```

## Next Steps for This Project

- Script automated root verification.
- Experiment with running a simple AI agent that monitors `/proc` and makes decisions.
- Document any custom Magisk modules or eBPF programs we create.

Contributions welcome — open an issue with your preferred method or results.
