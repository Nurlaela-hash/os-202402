# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `Nurlaela kusumandari`  
**NIM**: `240202877`  
**Modul yang Dikerjakan**:  
Modul 1 – System Call dan Instrumentasi Kernel

---

## 📌 Deskripsi Singkat Tugas

**Modul 1 – System Call dan Instrumentasi Kernel**:  
Pada modul ini, saya menambahkan dua system call baru pada kernel XV6, yaitu:

- `getpinfo()` – Menampilkan daftar proses aktif beserta PID, penggunaan memori, dan nama proses.
- `getReadCount()` – Mengembalikan jumlah pemanggilan system call `read()` sejak sistem booting.

---

## 🛠️ Rincian Implementasi

### ✅ Penambahan Fitur

- **`getpinfo()`**:

  - Ditambahkan pada `proc.c` untuk menelusuri seluruh proses dalam ptable.
  - Menggunakan struktur baru `struct pinfo` untuk menyimpan informasi proses.
  - Proses user dapat mengambil data ini dengan memanggil syscall.

- **`getReadCount()`**:
  - Menambahkan counter global `readcount` di kernel.
  - Setiap kali system call `read()` dipanggil, counter ditambahkan.
  - Bisa diakses user melalui syscall.

### 📂 File yang Dimodifikasi

- `proc.c`:

  - Menambahkan fungsi `getpinfo()`
  - Menambahkan deklarasi untuk `initproc`, `nextpid`, dan fungsi lainnya agar tidak terjadi error build.

- `proc.h`:

  - Menambahkan struktur `struct pinfo`
  - Menambahkan deklarasi fungsi `getpinfo(void)` dan counter `readcount`.

- `sysproc.c`:

  - Menambahkan definisi syscall `sys_getpinfo()` dan `sys_getReadCount()`.

- `syscall.c`:

  - Menambahkan entry syscall baru untuk `SYS_getpinfo` dan `SYS_getReadCount`.

- `syscall.h`:

  - Menambahkan `#define SYS_getpinfo 23`
  - Menambahkan `#define SYS_getReadCount 24`

- `usys.S`:

  - Menambahkan stub syscall: `SYSCALL(getpinfo)` dan `SYSCALL(getReadCount)`

- `user.h`:
  - Menambahkan deklarasi `int getpinfo(void);` dan `int getReadCount(void);`

### 🧪 Program Uji

- `ptest.c`: Untuk menampilkan proses aktif (menggunakan `getpinfo()`).
- `rtest.c`: Untuk menguji counter system call `read()`.

---

## ✅ Uji Fungsionalitas

**Berhasil dilakukan pengujian tanpa error.**

### 📍 Output `ptest`:

PID MEM NAME
1 12288 init
2 16384 sh
3 12288 ptest

### 📍 Output `rtest`:

Read Count Sebelum: 12

Read Count Setelah: 13
