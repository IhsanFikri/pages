---
title: "List all Crontab User [id]"
date: 2024-09-20
draft: false

tags:
- bash
- linux
---

```
for user in $(cut -f1 -d: /etc/passwd); do crontab -u $user -l; done


```
