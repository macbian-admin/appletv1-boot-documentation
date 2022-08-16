# appletv1-boot-documentation
Documentation for the boot process of the first generation Apple TV (AppleTV1,1) under Linux and Mac OS X/Apple TV OS


## 1. Early stages
When the 1st generation Apple TV is plugged into a power source, it likely performs some forms of self-testing, although this is unknown. The Apple TV then scans the USB bus for a disk containing an Apple boot.efi. If one is not found, the Apple TV boots its operating system from /boot.efi (or /System/Library/CoreServices/boot.efi) on the hard drive.

Since Apple never intended people to use alternative OSes on their Apple TVs (i.e. they hate their customers) they purposefully made it impossible to use any boot file other than the official Apple one (SHA-1 checksum: 74f03470469a836b95bdd8794164dafe71c62a31). While the exact method used to prevent users from using alternative EFI files is unknown (I\'m not Steve Jobs) it has been [theorized](https://github.com/davilla/atv-bootloader/blob/wiki/bootefi.md) that hashing is used. For this reason, booting Linux requires some incredibly weird hacks.

## 2. OS Boot Process
### a. Mac OS X/Apple TV OS
- The official boot.efi (in /System/Library/CoreServices) loads /mach_kernel.
- The Mach kernel (XNU/Darwin/Mac OS X) proceeds to load all kernel extensions (KEXTs) and start the system.

### b. Linux
- The official boot.efi (in /) loads /mach_kernel. This file, contrary to its name, is not the Mach kernel: it is a Linux kernel wrapped and can trick boot.efi into thinking that it's booting Mac OS X/Apple TV OS.
- Linux is booted from this file.
- On most linux distributions for the Apple TV, such as [OSMC](https://osmc.tv), a secondary kernel is loaded using kexec once the first one has initialized. This allows for easy and clean kernel updates without having to use a darwin cross compiler.
- Linux is booted as normal.

## X. Sources
- [Apple TV Bootloader/Documentation on Google Code](https://web.archive.org/web/20130525025321/http://code.google.com/p/atv-bootloader/)
