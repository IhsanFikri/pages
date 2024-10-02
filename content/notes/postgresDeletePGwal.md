---
title: postgres Delete PGwal 
date: 2024-10-01
draft: false
tags: 
- postgresql,
---
1. matikan service postgres => pg_ctlcluster 13 main stop

setting postgres
1. wal_level = minimal
2. archive_mode = off
3. comment archive_command => #archive_command

2. hapus file di pg_archive => masuk ke folder pg_archive jalankan perintah rm -rf *
3. hapus file di pg_wal 3 hari sebelum hari ini => masuk ke folder pg_wal jalankan perintah find . -type f -mtime +3 -exec rm {} \;
4. nyalakan kembali service postgres => pg_ctlcluster 13 main start