# Dokumentasi Fitur: Peta Posko - BARATA
*(Jawa Barat Berbudaya Tangguh Bencana)*

Dokumen ini berisi spesifikasi teknis dan fungsional dari fitur **Peta Posko** pada aplikasi BARATA. Fitur ini dirancang untuk mendata dan memetakan titik-titik Pos Pengamanan (Pos PAM) selama kegiatan operasi pengamanan besar, seperti Operasi Lilin (NATARU) dan Operasi Ketupat (Lebaran).

---

## 1. Gambaran Umum Fitur
* **URL Akses**: `https://barata.jabarprov.go.id/posko`
* **Output Utama**: Saat ini, halaman hanya berupa tabel *Data Record* (CRUD) yang menampilkan daftar titik posko yang telah diinput. **Belum ada output visualisasi peta interaktif** yang menampilkan sebaran posko-posko tersebut pada satu halaman untuk publik/dashboard.
* **Sumber Data**: Data merupakan hasil *data entry* manual oleh pengguna berdasarkan hasil rapat/keputusan mengenai titik-titik mana saja yang akan didirikan Posko Pengamanan.
* **Status Fungsional**: Terbatas pada fitur Tambah (Add), Edit (Ubah), dan Hapus (Delete) - dengan *catatan khusus pada fungsi Hapus (lihat Bagian 4)*.

---

## 2. Struktur Tata Letak (List View)

Halaman utama menampilkan tabel *Data Table* dengan struktur kolom sebagai berikut:
1. **No**: Nomor urut data.
2. **Kab/Kota**: Wilayah administrasi letak posko (dilengkapi dengan filter pencarian per baris).
3. **Lokasi**: Nama tempat atau titik lokasi spesifik posko (dilengkapi dengan filter pencarian per baris).
4. **Keterangan**: Rangkuman yang menampilkan titik koordinat posko dan daftar instansi yang terlibat.
5. **Aksi**: Kolom tombol aksi untuk *Edit* (Ubah) dan *Hapus* (Delete).

---

## 3. Detail Form Input (Tambah & Edit)
Saat pengguna menekan tombol **"TAMBAH +"** atau tombol ikon **Edit**, pengguna akan diarahkan ke form input yang memuat field berikut:

| Nama Field | Tipe Input | Status | Penjelasan |
|------------|------------|--------|------------|
| **Kab/Kota** | Select2 Dropdown | **Wajib** | Memilih letak wilayah administrasi posko tingkat kabupaten/kota. |
| **Tempat / Lokasi Posko** | Text Input | **Wajib** | Nama lokasi atau *landmark* spesifik (misal: "Pos PAM Simpang Gadog"). |
| **Keterangan / Instansi** | Summernote Editor (Rich Text) | Opsional | Kolom teks bebas bergaya WYSIWYG. Digunakan untuk mendeskripsikan instansi yang berjaga/terlibat (misal: TNI, Polri, BPBD, Dinkes). *Tidak ada field instansi khusus.* |
| **Latitude & Longitude**| Text Input & Peta Interaktif | **Wajib** | Titik koordinat geografis posko. Operator dapat mengisi angka koordinat manual atau menggeser marker pada **Peta Leaflet** yang disediakan, sehingga angka koordinat akan terisi secara otomatis. |

---

## 4. Temuan Masalah (Bug Report) & Catatan Error

Berdasarkan pengujian teknis pada halaman fitur ini, ditemukan *error* fungsional:

> [!WARNING]
> **BUG KRITIS: Tombol HAPUS Tidak Berfungsi (Unresponsive)**
> * Saat tombol **HAPUS** (berwarna merah pada tabel) diklik, tidak terjadi reaksi apapun (tidak memunculkan *alert/modal* konfirmasi, tidak menghapus data, dan tidak ada *network request* ke server).
> * **Analisis Teknis**: Tautan tombol menggunakan atribut `href="javascript:void(0);"`. Masalah ini umumnya disebabkan oleh *Event Listener Binding* JavaScript yang gagal pada elemen tabel yang di-render secara asinkron (dinamis) oleh komponen DataTable. Aplikasi legacy sering menggunakan `$('.deleted').click(...)` yang mana seharusnya direfaktor menjadi event delegation seperti `$(document).on('click', '.deleted', ...)`.

> [!TIP]
> **Tombol EDIT Berfungsi Normal**
> Form pengeditan berhasil mengambil data koordinat, mengisi input teks lama, dan meletakkan ulang titik marker pada Leaflet map dengan akurat.

---

## 5. Catatan untuk Re-Engineering

Untuk iterasi sistem (re-engineering) ke depan, terdapat beberapa rekomendasi peningkatan (enhancement) pada Modul Peta Posko:
1. **Perbaikan Fungsi Hapus**: Menggunakan arsitektur front-end modern (seperti *React/Next.js* atau sekadar *Event Delegation* murni) untuk menjamin tombol delete selalu reaktif.
2. **Realisasi Visualisasi Peta**: Membuat halaman visual utama yang menampilkan seluruh Pos PAM dalam satu kesatuan Peta interaktif (contoh: Peta Jawa Barat dengan kluster icon *Shield/Tent*).
3. **Pemisahan Field Instansi**: Merubah kolom deskripsi 'Keterangan' (Summernote) menjadi pilihan *Multi-Select Checkbox* untuk instansi yang terlibat, sehingga sistem dapat menghasilkan statistik jumlah personil instansi yang turun di lapangan.
