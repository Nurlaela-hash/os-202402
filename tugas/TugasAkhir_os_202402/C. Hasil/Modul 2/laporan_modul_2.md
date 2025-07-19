# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `Nurlaela kusumandari `  
**NIM**: `240202877`  
**Modul yang Dikerjakan**:  
Modul 2 – Non-Preemptive Priority Scheduler

---

## 📌 Deskripsi Singkat Tugas

- **Modul 2 – Non-Preemptive Priority Scheduler**:  
  Mengubah scheduler XV6 default menjadi scheduler berbasis prioritas non-preemptive. Proses dengan nilai `priority` lebih kecil akan dijalankan terlebih dahulu. Nilai prioritas lebih tinggi (angka lebih kecil) berarti lebih diprioritaskan.

---

## 🛠️ Rincian Implementasi

### Perubahan-perubahan utama:

✅ **File `proc.h`**

- Menambahkan atribut `int priority` pada `struct proc`.

✅ **File `proc.c`**

- Mengubah fungsi `scheduler()` untuk memilih proses RUNNABLE dengan `priority` terendah.
- Menghilangkan penggunaan variabel global `proc`; menggantinya dengan `cpu->proc` sesuai konvensi XV6.
- Memastikan pemilihan proses tidak menyebabkan panic dengan validasi alokasi `kstack`.

✅ **File `ptest.c`**

- Menambahkan program uji `ptest` untuk membuat 2 child dan 1 parent dengan prioritas berbeda.

✅ **Langkah tambahan**

- Build ulang image XV6: `make clean && make qemu-nox`
- Menyimpan perubahan dalam container Docker dengan volume agar tidak hilang saat restart.

---

## ✅ Uji Fungsionalitas

**Program Uji:** `ptest`

Program ini membuat proses parent dan 2 child, masing-masing diberi prioritas berbeda.

Kode:

```c
int main() {
  int pid1 = fork();
  if (pid1 == 0) {
    setpriority(3);
    exit();
  }

  int pid2 = fork();
  if (pid2 == 0) {
    setpriority(2);
    exit();
  }

  wait();
  wait();
  exit();
}

```

<h2>📷 Hasil Uji</h2>
📍 Output Terminal XV6
Screenshot 2025-07-19 at 7.50.53 PM.png

<img src="./Screenshot%202025-07-19%20at%207.50.53%20PM.png" alt="Screenshot Terminal" width="600">

📝 Output menunjukkan proses dengan prioritas lebih tinggi (angka lebih kecil) dijalankan terlebih dahulu:
Child 2 (priority 2) → Child 1 (priority 3) → Parent

## ⚠️ Kendala yang Dihadapi

### ❌ Kernel Panic: `switchuvm: no kstack`

Masalah ini muncul saat proses dijalankan **tanpa stack kernel (`kstack`)**, sehingga terjadi panic saat switch context. Hal ini disebabkan oleh kesalahan pada implementasi scheduler, yaitu:

```c
proc = p; // ❌ Salah: menggunakan variabel global proc

```

📚 Referensi
Buku xv6 MIT: https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf

Repositori xv6-public: https://github.com/mit-pdos/xv6-public

Diskusi praktikum, Stack Overflow, dan dokumentasi Docker Volume
