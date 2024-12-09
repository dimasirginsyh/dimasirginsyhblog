---
title: Pengalaman VPS menggunakan DEBIAN 12
author: Dimas Irgiansyah
date: 2024-12-07
draft: false
tags:
  - debian
  - mitoyus
  - blog
  - rust
  - indonesia
---
DEBIAN adalah distro yang sungguh popular, tetapi kenapa banyak hal yang cukup berbeda dengan UBUNTU yang merupakan salah satunya. Berikut adalah beberapa hal yang cukup berbeda di DEBIAN 12

#### 1. Install rust yang tidak lancar
Jika anda ingin menginstall rust, tidak cukup dengan perintah

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Anda juga harus menambahkan beberapa installasi yang harus dilakukan seperti

```bash
sudo apt install build-essential pkg-config libudev-dev libpq-dev
```

Setelah install itu semua, saya masih tidak bisa melakukan build release aplikasi rust saya.