---
title: "Bash Cheat sheet"
date: 2023-01-04
draft: false

tags:
- bash
---

![bash](data:image/png;base64,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)


Bash (The Bourne-Again Shell) adalah *command-line interpreter* atau *shell* yang digunakan untuk komunikasi dengan system operasi. Bash merupakan shell terpopuler pada Distribusi Linux, dan juga digunakan pada macOS.

<!-- ## Alias
---
If you are a lazy developer/sysadmin like me, the first thing you should do on your system is to make easy alias of all the long commands, below are the ones I often use on any system I use daily:

These can be imported on ~/.bashrc (if you use bash) or ~/.zshrc (if you are a MAC user and use ZSH) -->

<!-- {{< gist rizwan-kh 032e587751e54a2fd26f44c0267ea5c5 >}} -->
---
### **Basic Commands**:
- **`pwd`**: Untuk mengetahui **posisi direktori** saat ini (Current Working Directory)
- **`cd`**: Untuk **berpindah** ke direktori lain
- **`ls`**: Untuk **listing** pada direktori saat ini
- **`mkdir`**: Untuk **membuat direktori** baru
- **`touch`**: Untuk **mebuat file** baru
- **`mv`**: Untuk **merubah** nama file / direktori ataupun memindahkan file / direktori
- **`cp`**: Untuk **mengduplikasi** file / direktori
- **`rm`**: Untuk **menghapus** file / direktori

### **File permissions:**

- **`chmod`**: Untuk **merubah permission** pada filie / direktori
- **`chown`**: Untuk **merubah kepemilikan** (owner) pada file /direktori

### **Command history:**

- **`history`**: Untuk **melihat histori** perintah
- **`!!`**: Menjalankan perintah terakhir yang dijalankan
- **`!<number>`**: **Menjalankan perintah pada index** yang tertera pada histori

### **Text processing:**

- **`grep`**: Untuk mencari pattern pada sebuah file atau beberapa file
- **`sed`**: Untuk melakukan editing file dengan melakukan replace dengan pattern secara spesifik
- **`awk`**: memperoses file dengan memecah file tersebut menjadi beberapa record 

### **Process management:**

- **`ps`**: List proses yang berjalan pada sistem
- **`top`**: Menampilkan informasi proses suatu sistem secara real-time
- **`kill`**: Terminate sebuah proses

### **Environment variables:**

- **`export`**: Melakukan Set variable pada environment 
- **`unset`**: Melakukan Unset (penghapusan) variable pada environment 
- **`echo`**: Print nilai dari sebuah variable pada environment

### **Bash scripting:**

- **`#`**: Memulai Comment
- **`;`**: Memisahkan beberapa perintah pada satu line 
- **`&&`**: Menjalankan beberapa perintah, Jika perintah sebelumnya sukses
- **`||`**: Menjalankan beberapa perintah , Jika hanya perintah sebelumnya gagal
- **`if`**: Menjalankan perintah dengan sebuah kondisi
- **`for`**: Melakukan perulangan dengan batasan yang ditentukan
- **`while`**: Melakukan perulangan hingga kondisi terpenuhi
- **`function`**: Mendefine suatu fungsi

### **Some useful keyboard shortcuts for bash:**

- **`CTRL + A`** : Memindahkan cursor ke awal sebuah line
- **`CTRL + E`**: Memindahkan cursor ke akhir sebuah line
- **`CTRL + U`**: Menghapus line sebelum cursor
- **`CTRL + K`**: Menghapus line sesudah cursor
- **`CTRL + W`**: Menghapus sebuah kata sebelum crusor
- **`CTRL + L`**: Menghapus text pada screen
- **`CTRL + C`**: Interupsi pada suatu perintah
- **`CTRL + D`**: Keluar dari shell saat ini
- **`CTRL + Z`**: Suspend perintah saat ini