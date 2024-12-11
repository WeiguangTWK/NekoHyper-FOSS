# Basic Terms You need to Know

Here is some terms that will be frequently mentioned in later articles, you need to know and keep in mind.

## ROM
In computer terms "ROM" often refer to "Read-Only Memory" so does this. But call a file "ROM" it actually refers to "firmware". It is ROM where these firmwares are flashed into.

## Fastboot （线刷）
Fastboot (or named bootloader mode) is mode for device maintenance opt (etc. format userdata, unlock/lock bootloader and flash certain partitions) through other device like PC. [Here](https://android.googlesource.com/platform/system/core/+/master/fastboot/README.md) for more tech detail.
For us, fastboot is actually a command interface to perform operation above like
```
$ fastboot erase userdata
```
This command will clear all your userdata (in other word, this formats the userdata partition)
```
$ fastboot flash boot ~/boot.img
```
And this will flash "boot.img" under your home dir into your device's boot partition
I will introduce more command but here you just need to know what it is.

## Recovery （卡刷）

> Android recovery mode is a unique startup mode available in all Android devices that provide a set of tools for diagnosing and resolving issues that cannot be addressed from within the operating system. This mode is typically used to perform system updates, factory resets, or install custom ROMs. [Source Article](https://www.androidauthority.com/android-recovery-mode-3329920/)

Beside what mentioned above, Recovery is essentially a minimum Linux system which like windows PE or Linux Live disk for you to perform, of course, device maintenance. It do what fastboot can do but without a PC.

In most cases, stock recovery won't have an option to enable you to flash custom ROMs or unsigned zip update or even create backups
That's why we need to use custom recovery

## Bootloader Lock / OEM lock
"Bootloader" mentioned here refers to a program that load the system. (In the ABL partition of a phone).
### So as for the Lock?
If you try to flash certain partition with a modded image file without unlock it, you will get

> Failed (remote: 'command not allowed')

Or your phone won't boot after the flash

Locked bootloader will execute AVB (Android Verified Boot) to check if everything is trusted, if check failed, the phone will refuse to start up

## AVB / VBMeta

> Verified Boot strives to ensure all executed code comes from a trusted source (usually device OEMs), rather than from an attacker or corruption. It establishes a full chain of trust, starting from a hardware-protected root of trust to the bootloader, to the boot partition and other verified partitions including `system`, `vendor`, and optionally `oem` partitions. During device boot up, each stage verifies the integrity and authenticity of the next stage before handing over execution. [Source Article](https://source.android.com/docs/security/features/verifiedboot)

After bootloader is unlocked, the next one to deal with is AVB
VBmeta contain hash values and signatures of each partitions that need to be verified (see [here](https://android.googlesource.com/platform/external/avb/+/master/README.md) of tech detail)

Without disabling it or patching it (like replace public key of VBmeta and replace new hash into it) after flash custom partition images, your phone won't boot