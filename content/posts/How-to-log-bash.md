---
title: "How to log all bash commands by all user on Linux [id]"
date: 2024-09-15
draft: false

tags:
- bash
- linux
---

<!-- ![SENTRY](/assets/logging.png) -->

Melakukan Logging command pada user linux merupakan salah satu fitur yang diperlukan oleh system administrator, maka dari itu, berikut salah satu cara untuk melakukan pencatatan semua command yang di jalan kan oleh user/

---

## 
1. **Untuk BASH shell, edit pada system-wide runtime BASH config**
    
    ```bash
    sudo vim /etc/bash.bashrc
    ```
2. **Tambahkan perintah ini dibaigan paling bawah**

    ```bash
        export PROMPT_COMMAND='RETRN_VAL=$?;logger -p local6.debug "$(whoami) [$$]: $(history 1 | sed "s/^[ ]*[0-9]\+[ ]*//" )"'
    ```
   
3. **Edit file `/etc/rsyslog.d/bash.conf`**
    ```bash
        sudo -e /etc/rsyslog.d/bash.conf
    ```
    tambahkan kode berikut: 
    ```bash
        local6.*    /var/log/commands.log
    ```

4. **Edit file `/etc/logrotate.d/syslog`**
    ```bash
        sudo -e /etc/logrotate.d/syslog
    ```
    dan tambahkan kode berikut:
    ```bash
        /var/log/commands.log
    ```
5. **Restart `rsyslog` service**
    ```bash
        sudo service rsyslog restart 
    ```

Hasilnya akan nampak seperti ini:
```
Sep 15 03:32:52 DB-server ihsan: ihsan [64949]: df -h [0]
Sep 15 03:33:04 DB-server ihsan: ihsan [64949]: ls [0]
Sep 15 03:33:08 DB-server ihsan: ihsan [64949]: ls -al [0]
Sep 15 03:33:28 DB-server ihsan: ihsan [64949]: df -h [0]
Sep 15 03:33:48 DB-server ihsan: ihsan [64949]: cd /tmp/data/ [0]
Sep 15 03:33:51 DB-server ihsan: ihsan [64949]: ls -al [0]
Sep 15 03:33:55 DB-server ihsan: ihsan [64949]: cd .. [0]
```