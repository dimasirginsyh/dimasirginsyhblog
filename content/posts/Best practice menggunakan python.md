---
title: Best practice menggunakan bahasa Python
author: Dimas Irgiansyah
date: 2025-03-18
draft: false
tags:
  - mitoyus
  - blog
  - python
  - indonesia
  - chatgpt
---
Best practice dalam Python tergantung pada konteks penggunaannya, misalnya untuk web development, data science, atau automation. Tapi secara umum, berikut adalah beberapa **best practice** yang berlaku secara luas:  

---

## ✨ **1. Gunakan Versi Python yang Terbaru (Stable)**
Pastikan selalu menggunakan versi Python terbaru yang masih mendapatkan update keamanan dan fitur baru.  

Cek versi Python yang sedang digunakan:
```bash
python --version
```

---

## 📜 **2. Gunakan Penulisan Kode Sesuai PEP 8**
PEP 8 adalah standar gaya penulisan kode Python yang membuat kode lebih mudah dibaca. Gunakan **black** atau **flake8** untuk memeriksa dan memformat kode secara otomatis.  

**Contoh format yang benar**:
```python
def hitung_luas_persegi(sisi: float) -> float:
    """Menghitung luas persegi"""
    return sisi * sisi
```
> ❌ Hindari penamaan seperti `luaspersegi()` → gunakan **snake_case**: `hitung_luas_persegi()`

Gunakan **black** untuk memformat kode:
```bash
black script.py
```

---

## 📦 **3. Gunakan Virtual Environment**
Jangan install package secara global, gunakan **venv** atau **pipenv** untuk mengelola dependency secara terpisah per proyek.
```bash
python -m venv env
source env/bin/activate  # Untuk Linux/Mac
env\Scripts\activate  # Untuk Windows
```

---

## 🏗 **4. Gunakan Type Hinting**
Menambahkan tipe data pada parameter dan return function membuat kode lebih jelas dan mudah di-debug.
```python
def tambah(a: int, b: int) -> int:
    return a + b
```
Gunakan **mypy** untuk mengecek type hinting:
```bash
mypy script.py
```

---

## 🛠 **5. Gunakan Logging, Jangan print()**
Daripada `print()`, lebih baik pakai `logging` untuk debugging dan mencatat aktivitas aplikasi.
```python
import logging

logging.basicConfig(level=logging.INFO)
logging.info("Aplikasi berjalan dengan sukses!")
```
Logging bisa dikonfigurasi untuk menulis ke file, mengatur level log (INFO, DEBUG, ERROR, dll.), dan lainnya.

---

## 🚀 **6. Gunakan List Comprehension untuk Kode yang Lebih Efisien**
Alih-alih:
```python
angka_genap = []
for i in range(10):
    if i % 2 == 0:
        angka_genap.append(i)
```
Gunakan:
```python
angka_genap = [i for i in range(10) if i % 2 == 0]
```
List comprehension lebih **ringkas dan cepat**.

---

## 🔄 **7. Gunakan Context Manager untuk File Handling**
Daripada:
```python
file = open("data.txt", "r")
data = file.read()
file.close()
```
Gunakan:
```python
with open("data.txt", "r") as file:
    data = file.read()
```
Keuntungannya: **file akan otomatis ditutup** setelah selesai digunakan.

---

## 🏃 **8. Gunakan Exception Handling dengan Baik**
Tangani error dengan `try-except` agar aplikasi tetap berjalan meskipun terjadi kesalahan.
```python
try:
    hasil = 10 / 0
except ZeroDivisionError as e:
    print(f"Terjadi kesalahan: {e}")
```
Hindari **catch-all exceptions**:
```python
except Exception:  # ❌ Tidak spesifik
```
Gunakan exception yang lebih spesifik, seperti `ValueError`, `KeyError`, dll.

---

## ⏳ **9. Gunakan Multiprocessing atau Asyncio untuk Kode yang Lambat**
Untuk kode yang CPU-bound:
```python
from multiprocessing import Pool

def square(n):
    return n * n

with Pool(4) as p:
    print(p.map(square, [1, 2, 3, 4]))
```
Untuk kode yang I/O-bound:
```python
import asyncio

async def fetch_data():
    await asyncio.sleep(2)
    return "Data selesai diambil"

asyncio.run(fetch_data())
```
Gunakan **multiprocessing** untuk operasi CPU-heavy, dan **asyncio** untuk operasi I/O-heavy.

---

## 📌 **10. Gunakan f-string untuk String Formatting**
Alih-alih:
```python
name = "Dimas"
print("Halo, %s!" % name)  # ❌ Lama
print("Halo, {}!".format(name))  # ✅ Tapi kurang efisien
```
Gunakan **f-string** (lebih cepat dan mudah dibaca):
```python
print(f"Halo, {name}!")
```

---

## 🏆 **11. Gunakan Dependency Management (requirements.txt atau Poetry)**
Buat daftar dependency agar bisa diinstal ulang dengan mudah:
```bash
pip freeze > requirements.txt
pip install -r requirements.txt
```
Atau pakai **Poetry**:
```bash
poetry add requests
```

---

## 🔍 **12. Gunakan Linter dan Formatter**
- **Linter**: Mengecek kesalahan kode dan style (PEP8).
  ```bash
  flake8 script.py
  ```
- **Formatter**: Memformat kode secara otomatis.
  ```bash
  black script.py
  ```

---

## 📚 **13. Dokumentasi Itu Penting!**
Gunakan **docstring** agar kode lebih mudah dipahami.
```python
def bagi(a: int, b: int) -> float:
    """Membagi a dengan b. Mengembalikan float."""
    if b == 0:
        raise ValueError("Tidak bisa dibagi nol!")
    return a / b
```
Gunakan **Sphinx** atau **MkDocs** untuk membuat dokumentasi otomatis.

---

## 🔗 **14. Gunakan `.gitignore` untuk Menghindari File yang Tidak Perlu**
Saat menggunakan Git, buat file `.gitignore` agar tidak meng-commit file yang tidak perlu:
```
__pycache__/
env/
*.pyc
```
---

## 🚨 **15. Jangan Hardcode Password atau API Key**
Gunakan **.env file** dengan `python-dotenv`:
```python
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv("API_KEY")
```
Simpan variabel di `.env`:
```
API_KEY=123456789
```

---

Itu tadi beberapa **best practice Python** yang bisa diterapkan di proyekmu. 🔥  
Kalau kamu punya **kasus spesifik** (misalnya untuk web, AI, atau sistem), bisa kita bahas lebih lanjut! 🚀