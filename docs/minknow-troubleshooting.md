# Minknow Troubleshooting  

Even though MinKNOW should install out of the box using a simple ```ìnstall``` command, on some systems this doesn't work. It has been reported by other Oxford Nanopore users that re-installing MinKNOW can give serious issues and might block your sequencing flow in the lab. Before you plan to update your current MinKNOW installation, make sure you make the necessary backups, system snapshots etc. to go back untill before you reach the point of no return.  

This document provides a complete workflow to:

- Perform a clean MinKNOW installation
- Fully uninstall Oxford Nanopore Technologies (ONT) software
- Recover from broken or partial installations
- Clean system state (APT, services, caches, repositories)
- Verify that a system is completely cleaned before reinstalling
- Troubleshoot common installation issues  

!!! warning
    Make sure no previous installation has been done on your system. Otherwise start at Chapter 2 and perform an uninstall first.  


Applies to:

- Ubuntu 22.04 (Jammy)
- ONT MinION
- ONT GridION
- ONT P2 standalone installations

---

## Chapter 1. Clean Installation

### 1.1 Preconditions
 
Before starting, ensure:

- You have sudo access
- Stable internet connection (preferably wired)
- At least 20 GB free disk space
- No active sequencing run is ongoing
- System is fully updated
- GPU drivers are correctly installed (if applicable)

Check disk space:

```bash
df -h
```

Check for package issues:

```bash
sudo dpkg --configure -a
sudo apt --fix-broken install
```

Update package information:

```bash
sudo apt update
```

---

## 1.2 Add ONT Repository

Re-add the ONT repository if not already present:

```bash
wget -O- https://cdn.oxfordnanoportal.com/apt/ont-repo.pub | sudo apt-key add -
```

Create repository file:

```bash
echo "deb http://cdn.oxfordnanoportal.com/apt jammy-stable non-free" | sudo tee /etc/apt/sources.list.d/nanoporetech.sources.list
```

Update repositories:

```bash
sudo apt update
```

---

### 1.3 Install MinKNOW

Install MinKNOW:

```bash
sudo apt install ont-standalone-minknow-release
```

Depending on the release, additional components such as Dorado may be installed automatically.

---

### 1.4 Verify Installation

Check installed packages:

```bash
dpkg -l | grep minknow
```

Check services:

```bash
systemctl status minknow
systemctl status doradod
```

Launch MinKNOW:

```bash
minknow
```

Verify systemd services:

```bash
systemctl list-unit-files | grep -i minknow
```

---

### 1.5 Post-Install Validation

Verify the installation directories:

```bash
ls -la /opt/ont
```

Check logs:

```bash
journalctl -u minknow --no-pager
```

Confirm device detection and permissions.

If ONT hardware is not detected, verify udev rules:

```bash
ls /etc/udev/rules.d
```

---

## Chapter 2. Uninstall and System Cleanup

### 2.1 Stop ONT Services

Stop all running ONT services before uninstalling:

```bash
sudo systemctl stop minknow 2>/dev/null
sudo systemctl stop minknow-core 2>/dev/null
sudo systemctl stop minknow-gui 2>/dev/null
sudo systemctl stop doradod 2>/dev/null
```

Disable services:

```bash
sudo systemctl disable minknow 2>/dev/null
sudo systemctl disable minknow-core 2>/dev/null
sudo systemctl disable minknow-gui 2>/dev/null
```

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Verify services are stopped:

```bash
systemctl status minknow
systemctl status doradod
```

---

### 2.2 Remove ONT Packages

Remove MinKNOW release package:

```bash
sudo apt purge -y ont-standalone-minknow-release
```

Remove all related ONT packages:

```bash
sudo apt purge -y '*minknow*' '*ont*' '*nanopore*'
sudo apt purge -y 'ont-*' 'minknow*' 'dorado*'
```

Remove orphaned dependencies:

```bash
sudo apt autoremove --purge -y
```

Clean package metadata:

```bash
sudo apt autoclean
```

Update package database:

```bash
sudo apt update
```

---

### 2.3 Remove Installation Directories

Remove all ONT software installations:

```bash
sudo rm -rf /opt/ont
sudo rm -rf /opt/minknow
sudo rm -rf /opt/dorado
sudo rm -rf /opt/guppy
```

---

### 2.4 Remove Data, Logs and Configurations

Delete MinKNOW runtime data:

```bash
sudo rm -rf /var/lib/minknow
```

Remove logs:

```bash
sudo rm -rf /var/log/minknow*
sudo rm -rf /var/log/ont*
```

Remove configuration directories:

```bash
sudo rm -rf /etc/minknow
sudo rm -rf ~/.config/MinKNOW
sudo rm -rf ~/.minknow
```

---

### 2.5 Remove Systemd Service Files

Remove leftover service definitions:

```bash
sudo rm -f /etc/systemd/system/minknow*
```

Reload service manager:

```bash
sudo systemctl daemon-reload
sudo systemctl reset-failed
```

---

### 2.6 Remove Udev Rules

Check for installed ONT rules:

```bash
ls /etc/udev/rules.d | grep -i ont
```

Remove rules:

```bash
sudo rm -f /etc/udev/rules.d/*ont*
sudo rm -f /etc/udev/rules.d/*minknow*
```

Reload udev:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

---

### 2.7 Remove ONT Users and Groups

Check whether the account exists:

```bash
id minknow 2>/dev/null
```

Remove user and group:

```bash
sudo userdel -r minknow 2>/dev/null
sudo groupdel minknow 2>/dev/null
```

---

### 2.8 Remove ONT Repository (Full Reset)

Remove repository definition:

```bash
sudo rm -f /etc/apt/sources.list.d/nanoporetech.sources.list
```

List installed keys:

```bash
sudo apt-key list
```

Remove old ONT key if required:

```bash
sudo apt-key del <KEY_ID>
```

Update repository cache:

```bash
sudo apt update
```

---

### 2.9 Clean APT Cache and Package State

Clean package cache:

```bash
sudo apt clean
```

Remove cached packages:

```bash
sudo rm -rf /var/cache/apt/archives/*
```

Remove temporary installer files:

```bash
sudo rm -rf /tmp/apt-dpkg-install-*
```

Repair package database:

```bash
sudo dpkg --configure -a
sudo apt --fix-broken install
```

---

## Chapter 3. System Verification

After cleanup, verify that no ONT software remains.

---

### 3.1 Check Installed Packages

```bash
dpkg -l | grep -Ei "minknow|ont|nanopore|guppy|dorado"
```

Expected result:

```shell 
No packages should be listed
```  

---

### 3.2 Check Remaining Files

Search the entire system:

```bash
sudo find / -iname '*minknow*' 2>/dev/null
```

Additional checks:

```bash
find /opt -iname "*ont*" \
-o -iname "*minknow*" \
-o -iname "*guppy*" \
-o -iname "*dorado*" 2>/dev/null
```

Expected result:

```shell
No ONT-related directories should remain.
```

---

### 3.3 Check Services

Verify no MinKNOW services remain:

```bash
systemctl list-unit-files | grep -i minknow
```

Expected result:

No service definitions should be found.

---

### 3.4 Check User Accounts

```bash
id minknow
```

Expected result:

```text
id: 'minknow': no such user
```

---

### 3.5 Check Udev Rules

```bash
ls /etc/udev/rules.d
```

Verify no ONT-related rules remain.

---

### 3.6 Verify APT Repository Removal

```bash
ls /etc/apt/sources.list.d/
```

Ensure:

```text
nanoporetech.sources.list
```

is no longer present.

---

## Chapter 4. Reboot

After a complete cleanup:

```bash
sudo reboot
```

A reboot is strongly recommended before reinstalling MinKNOW.

---

## Chapter 5. Troubleshooting

### 5.1 Installation Fails with Checksum or Gzip Errors

Cause:

- Corrupted package downloads
- Damaged APT cache

Fix:

```bash
sudo apt clean
sudo rm -rf /var/cache/apt/archives/*
sudo apt update
```

---

### 5.2 MinKNOW Service Does Not Start

Check service logs:

```bash
journalctl -u minknow --no-pager
```

Check service status:

```bash
systemctl status minknow
```

---

### 5.3 Disk or Filesystem Errors

Review kernel messages:

```bash
dmesg | grep -i error
```

Run filesystem checks if required:

```bash
sudo e2fsck -f /dev/sdX
```

Replace `/dev/sdX` with the correct device.

---

### 5.4 Permission issues ("Nuclear Option")

On some multi user systems (like Ubuntu based systems) MinKNOW runs in writing permissions. This results in MinKNOW that starts to sequence but cannot write your output to your disk.  
A Nuclear Option was described by [Miles Benton](https://github.com/sirselim) in that you make the MinKNOW services to run as root. This has also been adopted and described by [David Eccles](https://gringer.gitlab.io/presentation-notes/2021/10/08/gpu-calling-in-minknow/#preparation).  A nuclear option indeed, but on my Ubuntu machine a necessary and standard approach. The specific issue on my machine, with tons of error logs beeing sent back and forth to Nanopore specialist led to no conclusive solution.  
Hence, I go nuclear every time :-).  

After a clean uninstall and re-installation follow following protocol to go nuclear:  

Temporarily run MinKNOW as root:

```bash
sudo perl -i -pe 's/(User|Group)=minknow/$1=root/' /lib/systemd/system/minknow.service
```

!!! Note
    Revert to standard configuration if you hesitate:  
    sudo perl -i -pe 's/(User|Group)=root/$1=minknow/' /lib/systemd/system/minknow.service


Reload and restart services:

```bash
sudo systemctl daemon-reload
sudo systemctl restart minknow
```

Verify configuration:

```bash
grep -E 'User=|Group=' /lib/systemd/system/minknow.service
```

This minimal adjustment should do the trick to resolve any writing permissions.  

---

## Chapter 6. Best Practices

- Use wired internet whenever possible.
- Maintain at least 20 GB of free disk space.
- Avoid system sleep or hibernation during installation.
- Do not interrupt package downloads.
- Dorado model downloads can exceed 600 MB to 3+ GB.
- Keep BIOS and memory settings stable.
- Verify GPU drivers before installation.
- Always reboot after a complete uninstall before reinstalling.
- Verify system cleanliness before attempting a fresh installation.
- If you need an "Offline MinKNOW" version contact Oxford Nanopore before you leave on your field expidition

---

## Summary

This procedure covers:

- Clean MinKNOW installation
- Full MinKNOW uninstall
- Removal of Dorado and Guppy
- Removal of ONT repositories
- Removal of system services
- Removal of udev rules
- Removal of logs and runtime data
- Removal of ONT users and groups
- System validation checks
- Troubleshooting and recovery procedures
- Clean reinstall workflow

## Usefull links  
- <https://github.com/sirselim>
- <https://gringer.gitlab.io/presentation-notes/2021/10/08/gpu-calling-in-minknow/#preparation>