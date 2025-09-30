# SSHRD_Script
- An unofficial, modified version of [Nathan verygenericname's SSHRD_Script](https://github.com/verygenericname/SSHRD_Script)
- All the extra features have been tested working on Ubuntu 24.04 and macOS Sonoma hackintosh. However there are no warranties especially for ARM macOS, please use at your own risk
- Linux or macOS required. Virtual machine and windows are not and will never be supported even if some features are available on them. It is recommended to use USB-A cable and Intel PC
- A7-A11 devices only. For 32-bit devices, use [Legacy iOS Kit](https://github.com/LukeZGD/Legacy-iOS-Kit)
## Usage
0. Clone this repository:   
`git clone https://github.com/iPh0ne4s/SSHRD_Script --recursive`   
cd into SSHRD_Script directory. Run `chmod +x sshrd.sh` if running the script for the first time
1. Run `./sshrd.sh <ramdisk version>` to create ramdisk
  - For iOS 7-9 devices, run `./sshrd.sh 12.0`
    - A7 iOS 7 devices will be stuck in recovery loop after loading a higher version ramdisk, run `./sshrd.sh 8.0` and boot the 8.0 ramdisk to fix this
  - For iOS 10+ devices, use device version as ramdisk version, e.g., run `./sshrd.sh 11.2.2` for iOS 11.2.2 iPhone 6s, or the closest one if the ipsw of device version doesn't exist, e.g., `./sshrd.sh 11.1` for iOS 11.0.1 iPhone X
    - A wrong ramdisk version might cause bootloop, and this always happens on 16.4+ devices, check device version first
  - It is common to see "an error occurred" or device rebooting, just try again
2. Run `./sshrd.sh boot` to boot ramdisk
  - If unable to connect to device, unplug and replug the cable
3. Run `./sshrd.sh ssh` to SSH into device, if the terminal says "WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!", run `rm -f ~/.ssh/known_hosts` and try again. Here are some other commands:
  - Reboot device: `./sshrd.sh reboot`
  - Erase device: `./sshrd.sh reset`
    - Run this one before `./sshrd.sh boot`
  - Dump onboard blobs: `./sshrd.sh dump-blobs`
  - Remove temporary files: `./sshrd.sh clean`
  - To exit recovery mode, run `./sshrd.sh --exit-recovery`
  - For extra commands see below
## Extra Features
### In this part, all the commands starting with ./sshrd.sh are designed to be executed after booting ramdisk, i.e., after creating ramdisk and running `./sshrd.sh boot`, before `./sshrd.sh ssh`
- Add support for 10.0.1-11.2.6 and 16.4+ ramdisk
  - Creating 16.1+ ramdisk is only supported on macOS, this is due to ramdisks switching to APFS over HFS+, and another dmg library has to be used
- Backup and restore activation files (iOS 10+)
  - Run `./sshrd.sh --backup-activation` to backup activation files, `./sshrd.sh --restore-activation` to restore them
- Backup and restore activation files (iOS 7-9, partially supported)
  - Commands are `./sshrd.sh --backup-activation-hfs` and `./sshrd.sh --restore-activation-hfs`
  - On 7.0-9.3.5, activation files cannot be downloaded using scp or sftp command, instead they can be moved to /private/var/mobile/Media (the directory that is accessible in normal mode without a jailbreak) to become downloadable, therefore open menu devices only, passcode locked devices are not supported
  - On 8.3+, activation files can be restored in the same way, place them in /private/var/mobile/Media first
- Install TrollStore on supported versions by running `./sshrd.sh --install-trollstore`
- Backup and restore the entire contents on NAND (dangerous, might cause bootloop)
  - Run `./sshrd.sh --dump-nand` to backup NAND to disk0.gz, `./sshrd.sh --restore-nand` to restore disk0.gz to /dev/disk0 on device. On 7.0-10.2.1, another option is to run `./sshrd.sh --dump-disk0s1s1` and `./sshrd.sh --restore-disk0s1s1` to backup and restore system partition
  - Do not mount any partition before running these commands
- iOS 7-8 brute force
  - Run `./sshrd.sh --brute-force` to get unlimited passcode attempts on passcode locked and disabled devices, iOS 7-8 only
- Create 8.0 ramdisk for A7 iOS 7 devices
- Partial A7 support on Linux
  - A7 devices still need to be manually placed into pwnDFU using [ipwnder_lite](https://github.com/LukeZGD/ipwnder_lite), pwning with gaster or ipwndfu on macOS may not work. [Usage](https://github.com/LukeZGD/Legacy-iOS-Kit/wiki/Pwning-Using-Another-iOS-Device)
## Notes
- If there are permission denied, terminated or operation not permitted errors with sshrd.sh, try running sshrd.sh with sudo, especially on macOS
- Even if mounting /mnt2 as read/write, some files like photos still won't be downloadable, that's due to userdata encryption and there's actually nothing wrong