---
title: "Menambahakan Arcade Demo video pada Hugo Page [id]"
date: 2023-02-03T07:30:50+07:00
draft: false

tags:
- hugo

---


<!-- ![Laravel](/assets/laravel.jpg) -->
{{<image src=https://d33wubrfki0l68.cloudfront.net/c38c7334cc3f23585738e40334284fddcaf03d5e/2e17c/images/hugo-logo-wide.svg posistion="center">}}

---
Post ini di buat karena kemarin lumayan lama untuk mencari tutorial menambahakan embed video dari site selain youtube. Long story short, pas mau bikin post terkait cara menambahkan projek laravel pada sentry, disalah satu tutorialnya terdapat UI Demonya, dan ternyata menggunakan salah satu vendor aplikasi demo generator bernama Arcade software. 

Jadi [Arcade](https://www.arcade.software/) ini merupakan sebuah aplikasi untuk membuat video demo secara interaktif yang mana usernya bisa hanya menambahkan plugin pada browsernya untuk melakukan record atau tutorial. Untuk *free user* hanya mendapatkan *3 video* demo yang dapat di publis.

Nah berikut cara menambahakn Arcade Video Demo pada Hugo Page.
Terdapat beberapa cara untuk menambahkan HTML script embed pada hugo page salah satunya adalah dengan menggunakan shortcode. Pada dasarnya HUGO sudah memiliki predefined /  built-in shortcodes seperti *`gist`* ataupun *`youtube`* nanti akan kita contohkan juga, disini kita akan membuat *custom shortcodes* sesuai dengan apa yang kita inginkan, untuk kasus saat ini adalah kita ingin menambahkan demo video dari arcade pada page hugo kita.

1. **File location**

Untuk membuat sebuah shortcode, tempatkan *HTML Template* pada lokasi 
- *`layouts/shortcodes/<SHORTCODE>.html`*
- *`themes/<THEME>/layouts/shortcodes/<SHORTCODE>.html`*

secara default untuk mengakses shortcodes pada hugo page adalah
```
    {{ <yourshortcode param >}}
```

jika kalian ingin membuat shortcode per-folder agar terorganisir itu bisa dilakukan contoh *`layouts/shortcodes/gambar/`* maka untuk mengakses shortcode nya adalah 
```
    {{ <gambar/yourshortcode param >}}
```

2. **Membuat Template HTML**

Setelah kita membuat video demo menggunakan *Arcade* akan di berikan beberapa pilihan yaitu
- Embed on Website
- Embed Post on X (Twitter)
-  Gif
- Video

<!-- ![arcade](/assets/exportarcade.png) -->
{{<image src="/assets/exportarcade.png" position="center" style= "width: 400px; height: 600px" >}}

lalu pilih ***copy code*** pada bagian website, didapatkan code berikut
```html
    <div style="position: relative; padding-bottom: calc(56.88524590163935% + 41px); height: 0; width: 100%">
    <iframe
        src="https://demo.arcade.software/<YOUR_VIDEO_ID>?embed"
        frameborder="0" 
        loading="lazy"
        webkitallowfullscreen mozallowfullscreen allowfullscreen
        style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;color-scheme: light;"
        title=" Sentry">
    </iframe>
</div>

```

code tersebut kita simpan pada *lokasi shortcodes* yang sebelumnya kita bahas, lalu kita lakukan perubahan pada bagian `src`
```html
    <!-- dari -->
    <iframe
        src="https://demo.arcade.software/<YOUR_VIDEO_ID>?embed"
        frameborder="0" 
        ...
        >
    </iframe>
    <!-- menjadi -->
    <iframe
        src="{{ .Get 0 }}"
        frameborder="0" 
        ...
        >
    </iframe> 
```


### Contoh Embed video Arcade
- ` {{ < arcade "https://demo.arcade.software/0y1ULvJXzVZCF3QtTr0y?embed" >}} `


{{< arcade "https://demo.arcade.software/0y1ULvJXzVZCF3QtTr0y?embed" >}}

<!-- ## For LVM
1. **Rescan Disk without restart machine**
    
    ```bash
    sudo echo 1>/sys/class/block/sda/device/rescan
    ``` -->
## Referensi
- https://gohugo.io/templates/shortcode-templates/

[//]: 
    [Arcade1]: (https://www.arcade.software/)