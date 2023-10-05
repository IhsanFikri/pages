---
title: "Menambahakan Arcade Demo video pada Hugo Page [id]"
date: 2023-02-03T07:30:50+07:00
draft: false

tags:
- hugo

---


<!-- ![Laravel](/assets/laravel.jpg) -->
![HUGO](https://d33wubrfki0l68.cloudfront.net/c38c7334cc3f23585738e40334284fddcaf03d5e/2e17c/images/hugo-logo-wide.svg)

---
Post ini di buat karena kemarin lumayan lama untuk mencari tutorial menambahakan embed video dari site selain youtube. Long story short, pas mau bikin post terkait cara menambahkan projek laravel pada sentry, disalah satu tutorialnya terdapat UI Demonya, dan ternyata menggunakan salah satu vendor aplikasi demo generator bernama Arcade software. 

Jadi [Arcade](https://www.arcade.software/) ini merupakan sebuah aplikasi untuk membuat video demo secara interaktif yang mana usernya bisa hanya menambahkan plugin pada browsernya untuk melakukan record atau tutorial. Untuk *free user* hanya mendapatkan *3 video* demo yang dapat di publis.

Nah berikut cara menambahakn Arcade Video Demo pada Hugo Page.
Terdapat beberapa cara untuk menambahkan HTML script embed pada hugo page



### Panduan UI
{{<arcade "https://demo.arcade.software/0y1ULvJXzVZCF3QtTr0y?embed" >}}

## For LVM
1. **Rescan Disk without restart machine**
    
    ```bash
    sudo echo 1>/sys/class/block/sda/device/rescan
    ```

## Referensi
- https://gohugo.io/templates/shortcode-templates/