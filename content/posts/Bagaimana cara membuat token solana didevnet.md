---
title: Bagaimana cara membuat token solana didevnet
author: Dimas Irgiansyah
date: 2024-12-11
draft: false
tags:
  - mitoyus
  - blog
  - solana
  - indonesia
  - chatgpt
  - crypto
---
Untuk membuat token di jaringan **Devnet** Solana, Anda bisa mengikuti langkah-langkah berikut. Ini akan melibatkan penggunaan **Solana CLI** dan **Token Program** untuk membuat token yang akan dipublish di **Devnet**.

### Langkah-langkah untuk Membuat Token Solana di Devnet:

1. **Pasang Solana CLI**: Jika Anda belum menginstal Solana CLI, ikuti langkah-langkah berikut:
    
    - Buka terminal dan unduh Solana CLI dengan perintah:
        
        ```bash
        sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
        ```
        
    - Verifikasi instalasi dengan perintah:
        
        ```bash
        solana --version
        ```
        
2. **Setel Jaringan ke Devnet**: Setelah Solana CLI terinstal, pastikan Anda menggunakan jaringan **Devnet**. Jalankan perintah berikut untuk mengatur Solana ke Devnet:
    
    ```bash
    solana config set --url devnet
    ```
    
3. **Buat Wallet**: Jika Anda belum memiliki wallet, Anda bisa membuatnya dengan perintah berikut:
    
    ```bash
    solana-keygen new --outfile ~/my-wallet.json
    ```
    
    Ini akan menghasilkan file kunci privat yang bisa digunakan untuk berinteraksi dengan blockchain Solana.
    
4. **Akses Saldo Devnet**: Anda bisa mendapatkan **SOL** (Solana) gratis di Devnet dengan meminta faucet. Gunakan perintah berikut untuk menerima beberapa SOL ke akun Anda:
    
    ```bash
    solana airdrop 2
    ```
    
    Ini akan memberikan 2 SOL gratis ke dompet Anda di Devnet.
    
5. **Instal SPL Token CLI**: Token Solana dikelola oleh SPL Token Program, jadi Anda perlu menginstal SPL Token CLI untuk membuat dan mengelola token Anda:
    
    ```bash
    cargo install spl-token-cli
    ```
    
6. **Membuat Token Baru**: Setelah SPL Token CLI terinstal, Anda dapat membuat token baru dengan perintah berikut:
    
    ```bash
    spl-token create-token
    ```
    
    Ini akan menghasilkan alamat token untuk token yang baru Anda buat.
    
7. **Membuat Akun untuk Token**: Setelah membuat token, Anda perlu membuat akun untuk menyimpan token tersebut di wallet Anda:
    
    ```bash
    spl-token create-account <TOKEN_ADDRESS>
    ```
    
    Gantilah `<TOKEN_ADDRESS>` dengan alamat token yang baru Anda buat.
    
8. **Mint Token ke Akun Anda**: Setelah akun siap, Anda dapat melakukan minting sejumlah token ke akun Anda dengan perintah:
    
    ```bash
    spl-token mint <TOKEN_ADDRESS> <JUMLAH> <AKUN_ALAMAT>
    ```
    
    Gantilah `<TOKEN_ADDRESS>` dengan alamat token yang Anda buat, `<JUMLAH>` dengan jumlah token yang ingin Anda mint, dan `<AKUN_ALAMAT>` dengan alamat akun yang telah dibuat sebelumnya.
    
9. **Verifikasi Token**: Untuk memverifikasi apakah token berhasil dimintakan, Anda bisa menjalankan perintah:
    
    ```bash
    spl-token accounts
    ```
    
    Perintah ini akan menampilkan semua akun yang memegang token dan saldo mereka.
    

### Catatan:

- **Devnet** adalah jaringan pengujian (testnet), jadi token yang Anda buat tidak akan memiliki nilai nyata di dunia luar.
- Pastikan Anda menggunakan token di jaringan yang benar (seperti Devnet) saat pengujian dan percakapan.

Dengan mengikuti langkah-langkah di atas, Anda dapat membuat dan mengelola token Solana di Devnet untuk keperluan pengujian atau eksperimen.