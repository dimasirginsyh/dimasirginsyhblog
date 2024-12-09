---
title: Jika Kamu Bingung Kok Ada File __init__.py tapi gak ada dicontoh, ini solusinya
date: 2024-12-09
draft: false
tags:
  - mitoyus
  - blog
  - python
  - indonesia
  - chatgpt
  - fastapi
---
Berikut adalah contoh isi dari file `__init__.py` yang perlu dimasukkan dalam setiap folder untuk memastikan folder tersebut diperlakukan sebagai paket Python (module), dan memungkinkan Anda untuk mengimpor modul-modul dalam folder tersebut.

### 1. **`my_fastapi_project/app/__init__.py`**

File ini akan menginisialisasi folder `app` sebagai paket. Biasanya file ini kosong, tetapi kadang bisa berisi pengaturan atau import global.

```python
# app/__init__.py

# Tidak ada yang perlu dimasukkan di sini jika tidak ada pengaturan tambahan
# Tetapi Anda bisa menambah kode global yang ingin diekspor jika diperlukan
```

### 2. **`my_fastapi_project/app/api/__init__.py`**

Folder ini berisi modul API. Anda bisa menambah pengaturan impor umum di sini, terutama jika Anda ingin mengelompokkan versi API.

```python
# app/api/__init__.py

# Anda bisa menambah logika untuk import atau pengaturan lainnya di sini
# Untuk sekarang, biarkan kosong jika tidak ada pengaturan tambahan
```

### 3. **`my_fastapi_project/app/api/v1/__init__.py`**

Jika Anda menggunakan versi API, seperti `v1`, ini adalah tempat yang tepat untuk menginisialisasi folder versi pertama. Biasanya file ini kosong.

```python
# app/api/v1/__init__.py

# Biasanya kosong jika tidak ada pengaturan tambahan
# Atau Anda bisa melakukan impor di sini jika diperlukan, seperti:
# from .endpoints import user, item
```

### 4. **`my_fastapi_project/app/api/v1/endpoints/__init__.py`**

Folder ini berisi endpoint API. Anda bisa menambahkan impor endpoint di sini untuk memudahkan penggunaan.

```python
# app/api/v1/endpoints/__init__.py

from .user import router as user_router
from .item import router as item_router

# Mengimpor endpoint lain jika diperlukan
```

### 5. **`my_fastapi_project/app/core/__init__.py`**

Folder ini berisi komponen inti aplikasi seperti pengaturan, konfigurasi, dan keamanan. Biasanya file ini kosong, tetapi jika Anda ingin mengelompokkan beberapa logika, Anda bisa melakukannya di sini.

```python
# app/core/__init__.py

# Anda bisa menambahkan pengaturan atau utilitas global di sini jika diperlukan
```

### 6. **`my_fastapi_project/app/models/__init__.py`**

Folder ini berisi model ORM. Biasanya file ini kosong karena semua model diimpor langsung di dalam file lainnya.

```python
# app/models/__init__.py

# Anda bisa melakukan impor model di sini jika diperlukan
# Misalnya:
# from .user import User
# from .item import Item
```

### 7. **`my_fastapi_project/app/schemas/__init__.py`**

Folder ini berisi schema Pydantic untuk validasi dan serialisasi data. File ini dapat digunakan untuk melakukan impor bersama jika diperlukan.

```python
# app/schemas/__init__.py

# Anda bisa melakukan impor Pydantic schema di sini
# Misalnya:
# from .user import User, UserCreate
# from .item import Item, ItemCreate
```

### 8. **`my_fastapi_project/app/services/__init__.py`**

Folder ini berisi logika bisnis aplikasi. Sama seperti folder lainnya, file `__init__.py` dapat diimpor jika Anda ingin mengimpor beberapa layanan.

```python
# app/services/__init__.py

# Biasanya kosong, namun jika ada beberapa pengaturan global atau impor, Anda bisa menambahkannya
```

### 9. **`my_fastapi_project/app/db/__init__.py`**

Folder ini berisi kode terkait database (misalnya pengaturan koneksi dan inisialisasi database). Biasanya file ini kosong, tetapi Anda bisa menambahkan pengaturan atau utilitas di sini jika diperlukan.

```python
# app/db/__init__.py

# Biasanya kosong, tetapi bisa berisi impor sesi DB atau utilitas lainnya
```

### 10. **`my_fastapi_project/app/utils/__init__.py`**

Folder ini berisi berbagai utilitas yang digunakan di seluruh aplikasi. Anda dapat mengimpor utilitas di sini jika diperlukan.

```python
# app/utils/__init__.py

# Anda bisa mengimpor utilitas atau helper functions di sini
# Misalnya:
# from .date_utils import current_time
```

### Kesimpulan

File `__init__.py` bertanggung jawab untuk menginisialisasi folder sebagai paket Python, yang memungkinkan Anda mengimpor file atau modul di dalam folder tersebut ke dalam kode Anda. Secara default, file ini biasanya kosong, kecuali jika ada pengaturan atau logika tertentu yang perlu diimpor atau disetel secara global untuk folder tersebut.