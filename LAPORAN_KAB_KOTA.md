# Dokumentasi Fitur: Laporan Kab/Kota - BARATA
*(Jawa Barat Berbudaya Tangguh Bencana)*

Dokumen ini berisi spesifikasi teknis dan fungsional dari fitur **Laporan Kab/Kota** pada aplikasi BARATA. Fitur ini dirancang sebagai pusat koordinasi komunikasi (berbasis obrolan/chat) antara Pusat Pengendali Operasi (Pusdalops) tingkat Provinsi dengan petugas lapangan/admin di tingkat Kabupaten/Kota.

---

## 1. Gambaran Umum Fitur
* **URL Akses**: `https://barata.jabarprov.go.id/lap_kab_kotav2`
* **Output**: Menyajikan data kejadian bencana yang diinput oleh petugas kabupaten/kota melalui aplikasi BARATA mobile, serta log interaksi dan riwayat status penanganan secara real-time.
* **Sumber Data**: Data primer berasal dari hasil *assessment* (kaji cepat) dan pelaporan awal oleh tim lapangan/Pusdalops Kab/Kota yang disinkronisasi ke akun Provinsi.
* **Keterangan**: Data yang diinput oleh kabupaten/kota melalui aplikasi mobile akan langsung muncul pada antarmuka *Laporan Kab/Kota* di akun Provinsi.

---

## 2. Struktur Tata Letak (Layout) Antarmuka

Halaman ini menggunakan desain *Chat-based UI* (antarmuka mirip aplikasi perpesanan) untuk mempermudah komunikasi respons cepat. Halaman terbagi menjadi dua panel utama:

### A. Panel Kiri (Daftar Kejadian Bencana)
Panel ini berfungsi sebagai kotak masuk (inbox) seluruh kejadian bencana.
* **Fitur Pencarian (Search)**: Input teks (`Cari Kejadian Bencana...`) untuk memfilter daftar laporan berdasarkan nama Kabupaten/Kota atau jenis bencana. Pencarian tereksekusi dengan menekan tombol Enter.
* **Tombol "+ Tambah Laporan"**: Tombol aksi berwarna oranye untuk memunculkan modal pembuatan laporan bencana awal secara manual.
* **Kartu Laporan (List Item)**: Setiap kejadian bencana direpresentasikan dalam sebuah kartu, yang memuat:
  1. Ikon representasi visual jenis bencana (misal: awan untuk cuaca ekstrem, ikon gelombang untuk banjir).
  2. Judul Jenis Kejadian.
  3. Nama Lokasi (Kabupaten/Kota).
  4. Waktu Kejadian (Tanggal & Jam).
  5. Tombol Edit Laporan (ikon pensil & kertas) untuk menyunting data awal.
  6. Indikator Status Warna (Titik/dot indikator di sudut kanan untuk status prioritas/kedaruratan).

### B. Panel Kanan (Detail Laporan & Jendela Koordinasi)
Panel ini aktif ketika pengguna memilih salah satu kartu laporan dari panel kiri.
* **Header Informasi**: Menampilkan rangkuman status penanganan aktif.
  * *Response Time (Waktu Respon)*: Indikator waktu penanganan cepat tanggap.
  * *Tombol Penanganan*: Tombol akses untuk memperbarui tahapan penanganan darurat.
* **Jendela Obrolan (Chat/Log Window)**:
  * Menampilkan kronologi pesan dan log koordinasi.
  * Pesan pertama *selalu* digenerate otomatis oleh sistem, berupa ringkasan terstruktur dari laporan awal (menggunakan template format laporan via WhatsApp/Teks). Isi laporan memuat data seperti jumlah korban terancam (KK/Jiwa), upaya Satgas/Muspika, kebutuhan mendesak, kondisi terkini, dan kerugian material.
* **Area Input Koordinasi (Bottom Action Bar)**:
  * Kolom teks untuk mengirim pesan obrolan terkait instruksi atau laporan tambahan.
  * Fitur unggah lampiran (ikon klip) untuk mengirim dokumen/foto bukti penanganan.
  * Fitur emoji.

---

## 3. Fitur Utama & Alur Kerja

### A. Formulir Tambah / Edit Laporan Awal
Meskipun data utamanya dari aplikasi mobile, admin juga dapat menambah/mengedit laporan melalui form dengan isian berikut:
1. **Kab/Kota** (Dropdown).
2. **Status Kejadian** (Dropdown prioritas/warna, misal: *Hijau*).
3. **Jenis Kejadian** (Dropdown).
4. **Nama Kejadian** (Dropdown dinamis).
5. **Tanggal Kejadian** (Date & Time Picker).
6. **Kondisi Terkini** (Teks).
7. **Keterangan Detail** (Teks).

### B. Daftar Periksa (Checklist) Penanganan
Saat tombol **Penanganan** di header diklik, akan muncul modal berisi 6 tahap kritis respon bencana yang dapat diceklis/diperbarui progresnya:
1. Kaji Cepat Dampak Bencana.
2. Penentuan Status Darurat.
3. Penyelamatan & Evakuasi Korban.
4. Pemenuhan Kebutuhan Dasar.
5. Perlindungan Kelompok Rentan.
6. Pemulihan Darurat Sarana & Prasarana.

### C. Sistem Perhitungan *Response Time* (Waktu Respon)
BARATA secara otomatis mengkalkulasi waktu respon (response time) kinerja Pusdalops. Waktu ini dihitung berdasarkan **selisih waktu** antara *Tanggal/Waktu Kejadian* yang diinput dengan *Waktu dikirimkannya pesan pertama (Log Laporan Awal)* di jendela koordinasi. Fitur ini sangat penting sebagai metrik audit kecepatan layanan tanggap darurat pemerintah.

---

## 4. Catatan Teknis Re-Engineering

Dalam pengembangan ulang sistem dari PHP/CodeIgniter ke stack modern, modul Laporan Kab/Kota harus sangat diperhatikan performanya, terutama:
1. **Real-time Chat Protocol**: Gunakan WebSockets (misal: Socket.io) atau *Server-Sent Events (SSE)* untuk jendela obrolan, menggantikan *long-polling* (jika masih digunakan di versi legacy). Hal ini agar pesan dan notifikasi dari aplikasi mobile masuk secara instan ke portal web.
2. **Optimasi Sinkronisasi Mobile**: Pastikan API endpoint yang menerima laporan dari aplikasi seluler BARATA terintegrasi dengan struktur database yang baru tanpa memutus flow laporan awal sistem obrolan.
3. **Pencarian Cepat (Instant Search)**: Fitur pencarian pada panel kiri sebaiknya menggunakan teknik *debouncing* pada sisi klien dan *indexing* yang baik pada basis data agar tidak membebani server saat jumlah laporan terus bertambah.
