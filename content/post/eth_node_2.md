---
title: "ETH Node Part 2: 64 Bit Ubuntu"
date: 2021-08-19
tags: ["ethereum", "web3", "node"]
categories: ["general"]
draft: false
---

# Out of memory
So I left the node running for a few days and noticed the sync rate dropping significantly. Upon watching the log print for a while, I saw the following:

> Aug 13 01:51:22 geth geth[12749]: runtime: out of memory: cannot allocate 4194304-byte block (2422145024 in use)
> 
> Aug 13 01:51:22 geth geth[12749]: fatal error: out of memory
> 
> Aug 13 01:51:22 geth geth[12749]: goroutine 16172 [running]:
> 
> Aug 13 01:51:22 geth geth[12749]: runtime.throw(0xce93c8, 0xd)
> 
> Aug 13 01:51:22 geth geth[12749]: #011runtime/panic.go:1117 +0x5c fp=0xc5b5334 sp=0xc5b5320 pc=0x5c328
> 
> Aug 13 01:51:22 geth geth[12749]: runtime.(*mcache).allocLarge(0xb6c53790, 0x8026, 0x20101, 0x29e20)
> 
> Aug 13 01:51:22 geth geth[12749]: #011runtime/mcache.go:226 +0x268 fp=0xc5b5364 sp=0xc5b5334 pc=0x3a888
> 
> Aug 13 01:51:22 geth geth[12749]: runtime.mallocgc(0x8026, 0xbaee50, 0x2001, 0x2bcf04)
> 
> Aug 13 01:51:22 geth geth[12749]: #011runtime/malloc.go:1078 +0x9c8 fp=0xc5b53c0 sp=0xc5b5364 pc=0x307d8
> 
> Aug 13 01:51:22 geth geth[12749]: runtime.makeslice(0xbaee50, 0x6922, 0x8026, 0x29ddc)
> 
> Aug 13 01:51:22 geth geth[12749]: #011runtime/slice.go:98 +0x6c fp=0xc5b53d4 sp=0xc5b53c0 pc=0x75a48

Well, that’s not good. After this repeated a few times, I googled and realised that the default Raspbian OS wouldn’t work in terms of memory addressing. The solution would be to download the image off: https://ethereum.org/en/developers/tutorials/run-node-raspberry-pi/

# Wrong Image
For some reason I downloaded the image for NanoPC instead of Raspberry Pi, which wrote the SD card as ext4 instead of FAT. The site took really long to download the image (averaging 200KB/s for a 700MB file took an hour zzz), but once that was done, it took 10 minutes for the initial set up.

Now to think of it, I downloaded the wrong image because the ethraspbian side wasn’t even allowing me to download the right image the other day, and going to ethraspbian.com directed me to the wrong image instead.

# SSH
The first thing I noticed was slow response to the prompt after entering the password for SSH.

The top result on Google suggested DNS: https://jrs-s.net/2017/07/01/slow-ssh-logins/ but this wasn’t it. The helpful reminder was to use the -vv flag to debug, which led me to this: https://serverfault.com/questions/792486/ssh-connection-takes-forever-to-initiate-stuck-at-pledge-network. Checking the ssh config file revealed that it was PAM which was causing the issue. A restart of SSH daemon and the issue disappeared.

# Running geth as a service
[Note: because I used the ETH 2.0 image, and geth isn’t installed, I had to install golang and the latest version (1.10.7 as of writing) of geth manually, following the instructions from the previous guide.]

This failed repeatedly, despite the command running perfectly fine. Journalctl turns up nothing, but wait, a few other services are failing too, referencing to ‘/dev/sda1’. But we have no /dev/sda1, so I commented out the line in /etc/fstab, and rebooted with bated breath…no issues on reboot! And now systemctl starts geth immediately. Who would have thought it?

After a few hours of troubleshooting, I have a sync-ing node again, and now I can monitor system parameters via Grafana. Let’s see how this goes!

