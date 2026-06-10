 Data Dampak Bencana

## Ada 3 form utama:

1. **Form Identitas Bencana** : mendata identitas bencana, seperti dinas mana yang menangani antara BPBD atau Damkar, kemudian ada jenis kejadian, nama kejadian, waktu kejadian, lokasi kejadian (hanya kabupaten/kota saja beserta lat long nya), penyebab kejadian, kronologi, keterangan tambahan. Setelah submit form ini, akan muncul 2 table:
- table pertama adalah table untuk tambah kota baru, karena pada kasus bencana tertentu bisa menyebar ke kota lain, contoh seperti gempa bumi. Untuk kota yang diinput diawal akan menjadi kota utama. 
- table kedua adalah table synchronnisasi kab/kota.
2. **Form Data Kecamatan dan Desa**: form ini mencatat data kecamatan dan desa yang terdampak bencana. Add kecamatan bentuknya akan jadi tab, setiap tab kecamatan bisa tambah desa, setiap tambah desa akan muncul form untuk mengisi data desa. Isi formnya yaitu detail lokasi, waktu, latitude & longitude, data kerusakan (seperti rumah, bangunan, kendaraan, dll), data prasarana & sarana vital (jalan, jembatan, jaringan air bersih, dll). Korban (menderita/terdampak, meninggal, mungungsi, terancam, dan hilang). Kebutuhan (dana, sdm, logistik, dll.), luas terdampak, dan gambar. 
3. **Form Penanggulangan Bencana**: form ini berisi Kategori Penanganan (bentuk table yang listnya sudah baku, hanya check kabupaten dan provinsi). Keterangan Kaji Cepat (textarea, inginnnya auto fill karena biasanya formatnya sama hanya beda data kota saja). kondisi terkini (textarea). Nilai Kerusakan (bisa otomatis isi tapi butuh rumus, atau bisa isi manual untuk koreksi). Nilai Kerugian. Status Kejadian (hijau, kuning, merah). Penerima Informasi. Sumber Informasi (diisi otomatis nama kota/kab). Tanggal dan Waktu terima laporan. Kebutuhan Mendesak (textarea).

## Tampilan untuk masyarakat
1. menampilkan informasi ada bahaya/bencana apa di lokasi user
2. menampilkan edukasi kebencanaan yang sesuai dengan lokasi user 
3. ada fitur routing ketika user akan menuju suatu wilayah ada potensi bencana apa saja yang dihadapi di perjalanan.