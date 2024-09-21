---
title: scan
date: 2024-09-21 
draft: false
tags:
 - bash
 - linux
---

scan all new disk
```bash
    for host in /sys/class/scsi_host/*; do echo "- - -" | sudo tee $host/scan; ls /dev/sd* ; done
```
