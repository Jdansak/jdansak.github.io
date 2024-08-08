---
layout: post
title: "Recovering Root Access in RHEL 9.0: A Step-by-Step Guide"
date: 2024-08-07
categories: [RHEL, Linux, Troubleshooting]
---

# Recovering Root Access in RHEL 9.0: A Step-by-Step Guide

If you've found yourself locked out of the root account in Red Hat Enterprise Linux (RHEL) 9.0, don't panic. There's a method to regain access by resetting the root password directly from the GRUB bootloader. This process involves booting into single-user mode using GRUB and making changes to the system configuration. Here’s a detailed guide on how to do this.

### Step 1: Access GRUB Menu
Upon system reboot, you'll encounter the GRUB bootloader screen. It’s crucial to act quickly here. Use the `UpArrow` and `DownArrow` keys to stop the automatic boot countdown. The countdown can be quite short, ranging from 1 to 5 seconds.

### Step 2: Modify Boot Parameters
With the GRUB menu open, navigate to the boot entry you wish to edit. Typically, you'll edit the default boot entry. Look for the line that starts with `linux` press the `e` key to edit the entry.

   make the following adjustments:
- **Remove Parameters**: Delete any `console=` and `vconsole=` parameters. These are often present in virtual machine installations.
- **Add Init Parameter**: Append ` rw init=/bin/bash` at the **end** of the line to instruct the system to boot directly to a bash shell.

After making these changes, press `Ctrl+x` to start booting with these parameters.

<img src=https://github.com/Jdansak/jdansak.github.io/blob/main/assets/img/GRUB_Edit.gif class="img-responsive" alt="">

### Step 4: Change Root Password
Now that you have write access, you can reset the root password by executing:
```bash
passwd root
```
Follow the prompts to enter and confirm the new password.

### Step 5: Prepare for Relabeling
Before you reboot, it's important to set the system to relabel the file contexts on the next boot. This is crucial for SELinux systems to function correctly after such changes. Execute:
```bash
touch /.autorelabel
```
Make sure to enter this command correctly to avoid any issues.

### Step 6: Reboot the System
Finally, initiate a forced reboot by running:
```bash
/usr/sbin/reboot -f
```

### What Happens Next?
The system will reboot and start the relabeling process due to the existence of the `/.autorelabel` file. This process can take some time, depending on the number of files in your system. Once it’s done, the system will remove the `/.autorelabel` file and reboot normally.

At this stage, you should be able to log in as root with the new password you set. This method is a lifesaver for situations where you need emergency access to a system for which the root password has been lost or forgotten.

Remember, this method should only be used in situations where it's absolutely necessary and in compliance with your organization's security policies, as it involves significant security risks. Always ensure that any system accessed in this manner is secured properly afterwards.
