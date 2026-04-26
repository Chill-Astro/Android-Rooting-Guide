<p align="center">
  <kbd>  
    <img alt="Android Rooting Guide Promo" src="https://github.com/user-attachments/assets/60ba4ead-4ae6-46e8-bd35-adfac108c3a9" />
  </kbd>
</p>

<div align="center">

This Repository Contains an Online Copy of the `Guide` Section of Project : [FOSS Root Checker](https://github.com/Chill-Astro/FOSS-Root-Checker) for Easy Access from Computers.

I hope that this Project helps in your Journey to Break Corporate Shackles!

</div>

> [!IMPORTANT]
> This Project is not Affiliated with Google, Magisk, and other Open Source / Closed Source Referenced in this Project. Certain Brands mentioned are for General Information Only.

---

# Chill-Astro Presents : The GOATED Rooting Guide! 🐐🐐🐐🐐🐐🐐

## 1. Rooting: An Introduction

> [!CAUTION]
> Please BE CAREFUL what apps you are giving Root Permissions to. I am not responsible for Data or Money Theft by Malware on your Device.

### Introduction: What is Rooting?
Rooting an Android device means gaining full administrative (superuser) control, similar to an administrator on a computer, by unlocking deep system access restricted by manufacturers.

---

### Pros of Rooting
* **Bloatware Removal** ✅
* **System-wide Adblocking** ✅
* **Advanced Theming and Modification** ✅
* **Full Data Backups** ✅
* **Unlimited Google Photos Backups** ✅
* **Unlocking Higher FPS in Games** ✅
* **Sound Enhancement** ✅
* **Running FULL BLOWN Linux on Android using chroot** ✅
* **Battery Longevity (ACC)** ✅

*And Many Others.......*

---

### Cons of Rooting
* **Usually Voids Warranty** ❌
* **Increased Security Risks** ❌
* **Loss of Hardware Encoding** ❌
* **No Official Updates (OTA)** ❌
* **Data loss** ❌
* **Risk of Bricking Device** ❌

> [!IMPORTANT]
> Now with that out of the way, let me inform you about some **ADDITIONAL STUFF** that you **WILL FACE** during your modding journey.

---

### What is a Bootloader?
The **Bootloader** is the first piece of software that runs every time you turn on your Android device. It acts as a security guard and a guide, directing the hardware on how to start up and which operating system to "hand off" control to. This is locked by default to ensure stability and prevent malware from infecting the device.

---
## What is Bricking?
Bricking refers to a device becoming completely non-functional, usually due to a corrupted software update or a failed firmware modification.

### Types of Bricking and How to Fix Them

#### 1. Soft Brick
A soft brick is a **"recoverable"** state. The device might be stuck in a boot loop (constantly restarting at the logo) or booting straight into recovery mode.
* **The Cause:** Usually a minor software error, incompatible app, or a bad module.
* **The Fix:** Can often be fixed by a factory reset, clearing the cache, or reflashing the original firmware using a computer.

#### 2. Hard Brick
A hard brick is much more serious. The device shows no signs of life—no lights, no vibration, and the screen remains black.
* **The Cause:** This happens when the bootloader or the kernel is corrupted or deleted.
* **The Fix:** This often requires specialized hardware tools to bypass the main software, or in many cases, a physical replacement of the motherboard. Tools such as **SP Flash Tool** and **mtkclient** can sometimes fix this, but **FASTBOOT** is not accessible during this time.

---

### What is Device Mapper Verity (dm-verity)?
**Device Mapper Verity** is a transparent integrity-checking feature of the Linux kernel. Its sole job is to ensure that the data on critical partitions (like `/system`, `/vendor`, or `/product`) has not been modified even by a single bit. This is why it is sometimes disabled while modding.

### How does dm-verity work?
The system creates a **"Hash Tree" (Merkle Tree)**:
1. It hashes every 4KB block on the partition.
2. It then hashes those hashes.
3. It keeps doing this until only one hash remains at the very top.

This final single hash is called the **Root Hash**. This hash is digitally signed by the manufacturer and stored in a read-only area (the **VBMeta** partition).

When Android wants to read a file, the kernel reads the 4KB block from the disk and calculates its hash. It compares it against the "parent" hash in the tree, all the way up to the Root Hash. If the math doesn't match perfectly, it knows the block was tampered with. This is when you get the **"dm-verity corruption"** and **"System is Destroyed"** warnings.

---

### Suggestion from My Experience
From my experience with rooting: **Use Magisk if you are not sure.** It works on almost every device, can be flashed via PC or Custom Recovery (like TWRP or OrangeFox), and does the job very well. 

**Unless your device is old, DO NOT USE EXPLOITS!** I have soft-bricked my own device doing that, so please be careful!

---

## 2. Unlocking Bootloader

> [!CAUTION]
> This process will wipe all user data. Ensure you have a backup before proceeding. Also Xiaomi, Oppo and Realme have Additional Steps. Vivo, iQOO and certain Manufacturers don't support Bootloader Unlocking.

### Fastboot Method ( Recommended ) :

<details>

### Step 1 : Reboot Phone to Bootloader :
```bash
$ adb reboot bootloader
```

### Step 2 : Unlock Bootloader using Fastboot :
**• For most devices :**
```bash
$ fastboot flashing unlock
```
**• For some older devices :**
```bash
$ fastboot oem unlock
```

### Pros :
* Unlocking doesn't brick device immediately ✅
* Safe and Easy to Use ✅

### Cons :
* Not available on all devices ❌
* Xiaomi Devices need permission from Xiaomi Community and then Mi Unlock Tool is used ❌
* Oppo and Realme Devices use 'Deep Testing' or 'In-Depth Test' for Fastboot Permissions ❌

</details>

---

### MTKClient ( For MTK Devices ) 

> [!CAUTION]
> Please BE CAREFUL as it doesn't work on very new device and can cause 'System is Destroyed' and 'dm-verity corruption' Ensure that your device has no Replay Protected Memory Block (RPMB) before proceeding.

<details>

Hardware-level bypass for locked MediaTek chipsets.

First install USBdk if using Windows (Recommended).

**NOTE :** For Each Step, Run the Command, Press both Volume Buttons and Connect Phone to PC.

### Step 1 : Dump vbmeta : 
```bash
$ python mtk.py r vbmeta_a,vbmeta_b vbmeta_a.img,vbmeta_b.img
$ python mtk.py r vbmeta vbmeta.img # For Old Devices
```

### Step 2 : Unlock Bootloader : 
```bash
$ python mtk.py da seccfg unlock
```

### Step 3 : Disable dm-verity (Easy Way) :  
```bash
$ python mtk.py da vbmeta 3
```

### Step 4 : Erase Userdata : 
```bash
$ python mtk.py e metadata,userdata
```

### Step 5 : Reboot Device : 
```bash
$ python mtk.py reset
```

### Pros :
* Easy to Recover with Backups ✅
* Can fix Hard-Bricks ✅
* Fast and Easy to Use ✅

### Cons :
* Does not Support QualComm and UniSOC Devices ❌
* High Chances of Bricking ❌
* Doesn't work on very new devices ❌
* Fastboot may not be usable as on Realme Devices ❌

**Link :** [mtkclient by @bkerler](https://github.com/bkerler/mtkclient)

</details>

---

## 3. Rooting Methods :

> [!CAUTION]
> 1. Use Official Sources Only
> 2. Don't use 'One-Click Root' Apps
> 3. UNLOCK Bootloader first
> 4. FASTBOOT devices ONLY ( Excludes Samsung & Odin )

### Magisk (Recommended)

<details>

First, obtain your stock `boot.img` or `init_boot.img`, patch it using the Magisk App, and then flash it.

### Flash Commands:
**Flash to Active Slot:**
```bash
$ fastboot flash boot_a patched.img
$ fastboot flash init_boot_a patched.img
```
**Flash to Inactive Slot (If Needed):**
```bash
$ fastboot flash boot_b patched.img
$ fastboot flash init_boot_b patched.img
```
**For Older Devices:**
```bash
$ fastboot flash boot patched.img
```

### Pros:
* **Truly Systemless** ✅
* **Widest Module Support** ✅
* **Works on pretty much anything** ✅
* **Best possible documentation and compatibility** ✅

### Cons:
* **Easily Detectable as it leaves Traces** ❌

**Link:** [Magisk by @topjohnwu](https://github.com/topjohnwu/Magisk)

</details>

---

### KernelSU

> [!WARNING]
> If your kernel version is below **5.10**, this device doesn't support KernelSU OFFICIALLY. You will have to compile your device's kernel and integrate KernelSU into it YOURSELF!

<details>

First, obtain your stock `boot.img` or `init_boot.img`, patch it using the KernelSU App, and then flash it.

### Flash Commands:
**Flash to Active Slot:**
```bash
$ fastboot flash boot_a patched.img
$ fastboot flash init_boot_a patched.img
```
**Flash to Inactive Slot (If Needed):**
```bash
$ fastboot flash boot_b patched.img
$ fastboot flash init_boot_b patched.img
```
**For Older Devices:**
```bash
$ fastboot flash boot patched.img
```

### Pros:
* **Fully Systemless** ✅
* **Very hard to detect by Banking Apps** ✅
* **Leaves no Traces** ✅

### Cons:
* **Only Supports devices with Generic Kernel Image (GKI)** ❌

**Links:**
* [KernelSU by @tiann](https://github.com/tiann/KernelSU)
* [KernelSU Next by @KernelSU-Next](https://github.com/KernelSU-Next/KernelSU-Next)
* [SkiSU Ultra by @SkiSU-Ultra](https://github.com/SkiSU-Ultra/SkiSU-Ultra)

</details>

---

### APatch

> [!WARNING]
> Not all Devices support APatch! Please ensure that your kernel has `kallsyms`. **DO YOUR OWN RESEARCH!**

<details>

First, obtain your stock `boot.img` and patch it using the APatch App and then flash it.

### Flash Commands:
**Flash to Active Slot:**
```bash
$ fastboot flash boot_a patched.img
```
**Flash to Inactive Slot (If Needed):**
```bash
$ fastboot flash boot_b patched.img
```
**For Older Devices:**
```bash
$ fastboot flash boot patched.img
```

### Pros:
* **Fully Systemless** ✅
* **Very hard to detect by Banking Apps** ✅
* **Leaves no Traces** ✅
* **Doesn't need a GKI Device** ✅

### Cons:
* **Doesn't work on every device** ❌

**Link:** [APatch by @bmax121](https://github.com/bmax121/APatch)

</details>

---         

## 4. Root Hiding :

> [!CAUTION]
> This allows you to Bypass Root Checks used by Banking apps for YOUR FINANCIAL SAFETY! Please be cautious while hiding Root.

### Introduction: What is Root Hiding?
Now that your device is unlocked and rooted, it’s time to hide this status! Certain apps—like banking apps and games with anti-cheat—check for the presence of **Zygisk**, **Magisk**, the **"su" binary**, and more for user safety. 

However, with the power of **Systemless Rooting** and **Modules**, your device can provide a software-level lie to all apps, making them believe the system is completely stock and locked.

---

### Enabling Zygisk
* **If using Magisk:** Enable **Zygisk** in the app settings.
* **If using KernelSU, APatch, or Magisk (with built-in Zygisk OFF):** Flash one of the following modules:
    * [ReZygisk by @PerformanC](https://github.com/PerformanC/ReZygisk)
    * [Zygisk Next by @Dr-TSNG](https://github.com/Dr-TSNG/ZygiskNext)

---

### Root Hiding Modules

### 1. Tricky Store (Closed Source but Recommended)
This module spoofs **Hardware Backed Attestation** (via TEE) by injecting a valid `KeyBox.xml`. When combined with **Tricky Addon** and its WebUI interface, the process becomes much easier.

**Instructions:**
1. Obtain the `.zip` files for both modules and flash them.
2. After rebooting, tap the **Action** button under Tricky Store.
3. In the WebUI, select all relevant apps and tap **Set Valid Keybox**.

* [Tricky Store by @5ec1cff](https://github.com/5ec1cff/TrickyStore)
* [Tricky Addon by @KOWX712](https://github.com/KOWX712/Tricky-Addon-Update-Target-List)

### 2. Shamiko (Closed Source)
Used to hide root status, all traces of Zygisk, and root paths. It effectively fakes the "Locked" status of your bootloader.
* [Shamiko by @LSPosed](https://github.com/LSPosed/LSPosed.github.io/releases/)

### 3. Play Integrity Fix (For Custom ROM Users)
This assigns a valid fingerprint of a locked device systemlessly. Flash **one** of these modules and tap the **Action** button after rebooting.
* [Play Integrity Fork by @osm0sis](https://github.com/osm0sis/PlayIntegrityFork)
* [Play Integrity Fix by @KOWX712](https://github.com/KOWX712/PlayIntegrityFix)

---

### Open Source Alternatives
Haha, what an irony! 💀 A FOSS app recommending closed-source modules! Peak logic. If you prefer keeping it open-source, check these out:

* [TEESimulator by @JingMatrix](https://github.com/JingMatrix/TEESimulator)
* [NoHello by @MhmRdd](https://github.com/MhmRdd/NoHello)
* [Tricky Store OSS by @beakthoven](https://github.com/beakthoven/TrickyStoreOSS)
* [Zygisk Assistant by @snake-4](https://github.com/snake-4/Zygisk-Assistant)

---

## HALL OF FAME 👍 : 

// Will add Forked Repos which are genuinely good. 🤩 I will list everything Good about them.

---

## HALL OF NEUTRALITY 😐 :

// Will add Inactive Forks. Uh yeah that's it atleast it's Forking not Cloning! 😅

- philipprochazka/Android-Rooting-Guide
- vloco2417-lab/Android-Rooting-Guide

---

## HALL OF SHAME 👎 :

// Includes Clones who are working against the MIT Licence and Distributing Malware. All Flaws are mentioned. 😑

---

## ⚠️ IMPORTANT NOTICE ⚠️

Please be aware: There are fraudulent repositories on GitHub that are cloning this project's name and using AI-generated readmes, but they contain **completely random and unrelated files in each release**. These are NOT official versions of this project.

**ALWAYS ensure you are downloading or cloning this project ONLY from its official and legitimate source:**
`https://github.com/Chill-Astro/Android-Rooting-Guide`

Check [here](https://github.com/Chill-Astro/FOSS-Root-Checker/issues/1) for more details. I am trying my best to report these people.

---

## ⚠️ Smoking Gun for Danger :

<details>
<summary><b>View Details</b></summary>
  
**If your download contains any of the following, DELETE IT IMMEDIATELY:**

* **Suspicious Windows Executables:** Files ending in `.exe`, `.bat`, or `.dll` (e.g., `luau.exe`, `StartApp.bat`).
* **Compressed Archives:** This Repository **HAS NO RELEASES**, so don't expect a `.zip` or `.7z` containing Windows binaries.
* **Hidden Scripts:** Text files like `asm.txt` used to execute malicious code on your PC.
* The Following Folder Structure is used by Malware (Shown in a VM) :

![Screenshot_2026-03-01-18-52-39-337_com clone android dual space](https://github.com/user-attachments/assets/be691c9f-7def-4e8b-982c-c7ca2e9a067d)

![Screenshot_2026-03-01-18-53-09-759_com clone android dual space](https://github.com/user-attachments/assets/1c75031d-95be-4716-9347-b762e3dad5b8)

</details>

---

## Credits :

- [Magisk by @topjohnwu](https://github.com/topjohnwu/Magisk) : For Rooting pretty much anything these days.
- [KernelSU by @tiann](https://github.com/tiann/KernelSU) : For Kernel-Level Rooting on GKI Devices.
- [APatch by @bmax121](https://github.com/bmax121/APatch) : For Easy Kernel-Level Rooting.
- [mtkclient by @bkerler](https://github.com/bkerler/mtkclient) : For allowing MTK Devices to be Rooted Easily ( Including my Phone ).
- [Shamiko by @LSPosed](https://github.com/LSPosed/LSPosed.github.io/releases/) : For hiding root traces and faking bootloader status.
- [Tricky Store by @5ec1cff](https://github.com/5ec1cff/TrickyStore) : For spoofing Hardware Backed Attestation.
- [Tricky Addon by @KOWX712](https://github.com/KOWX712/Tricky-Addon-Update-Target-List) : For making the Tricky Store process accessible via WebUI.
- [Zygisk Next by @Dr-TSNG](https://github.com/Dr-TSNG/ZygiskNext) : For providing a standalone Zygisk implementation.
- [ReZygisk by @PerformanC](https://github.com/PerformanC/ReZygisk) : For an alternative Zygisk implementation.
- [Zygisk Assistant by @snake-4](https://github.com/snake-4/Zygisk-Assistant) : For helping hide Zygisk from detection.
- [Play Integrity Fix by @KOWX712](https://github.com/KOWX712/PlayIntegrityFix) : For maintaining Google Play Integrity standards.
- [Play Integrity Fork by @osm0sis](https://github.com/osm0sis/PlayIntegrityFork) : For the widely used community fork of the integrity fix.
- [TEESimulator by @JingMatrix](https://github.com/JingMatrix/TEESimulator) : An open-source alternative for TEE spoofing.
- [NoHello by @MhmRdd](https://github.com/MhmRdd/NoHello) : An open-source alternative for hiding root.
- [Tricky Store OSS by @beakthoven](https://github.com/beakthoven/TrickyStoreOSS) : For providing an open-source version of Tricky Store.
- [KernelSU Next by @KernelSU-Next](https://github.com/KernelSU-Next/KernelSU-Next) : For the continued development and community fork of KSU.
- [SkiSU Ultra by @SkiSU-Ultra](https://github.com/SkiSU-Ultra/SkiSU-Ultra) : For providing specialized kernel-level rooting features.
- [TWRP & OrangeFox](https://twrp.me/) : For the custom recoveries that make flashing these modules possible.

---

## Note from Developer :

Appreciate my effort? Why not leave a Star ⭐ ! Also if forked, please credit me for my effort and thanks if you do! :)

---
