# Persiapan aplikasi Absensi PKn SMPN 6 Manokwari

Proyek Firebase: `scan-smp-6`. Berkas web utama: `index.html`. Kode telah diintegrasikan dengan Firebase Web SDK dan memakai zona waktu `Asia/Jayapura` (WIT).

## Langkah sebelum diakses siswa

1. Buka **Firebase Console → Build → Firestore Database**. Buat database jika belum tersedia. Pilih lokasi database yang sesuai; perhatikan kebijakan sekolah terkait lokasi data.
2. Masuk **Firestore Database → Rules**. Salin isi `firestore.rules`, lalu **Publish**. Jangan memakai aturan `allow read, write: if true`.
3. Buka **Authentication → Sign-in method**, aktifkan **Anonymous** (untuk murid) dan **Email/Password** (untuk guru).
4. Di **Authentication → Users**, buat akun Email/Password **guru**. Salin `UID` akun tersebut.
5. Di **Firestore Database → Data**, buat koleksi `teachers` dan dokumen dengan **Document ID tepat sama dengan UID guru**. Tambahkan field sederhana, misalnya `name` (string) = `Guru PKn`. Dokumen ini hanya boleh dibuat oleh administrator di Firebase Console/Admin SDK. **Tidak perlu dan jangan menyimpan password di Firestore**.
6. Login di Portal Guru dengan **email/password Firebase** tadi. Gunakan tombol **Tambah Siswa** atau **Template & Sinkron Excel** untuk memasukkan daftar siswa. Tidak ada daftar siswa contoh yang dipublikasikan di dalam source HTML.
7. Di **Authentication → Settings → Authorized domains**, bila diperlukan tambahkan domain GitHub Pages Anda (`namauser.github.io`). Gunakan alamat domain yang benar-benar akan menayangkan aplikasi.
8. Unggah `index.html` ke repository GitHub, lalu buka **Settings → Pages → Deploy from branch**, pilih branch dan direktori root. File `firestore.rules` disimpan untuk referensi/deploy CLI; **GitHub Pages tidak menerapkan Firestore Rules secara otomatis**.
9. Pengujian dua perangkat: isi satu absensi murid, lihat di Portal Guru, kirim sekali lagi (harus ditolak server), coba tambah/ekspor/hapus sebagai guru, lalu coba akses database tanpa izin (harus ditolak). Periksa juga tanggal/jam WIT dan mode mobile.

## Hal yang sangat penting

- **Konfigurasi API key web Firebase tidak sama dengan password administrator**, tetapi pembatasan API key dan aturan Firestore tetap perlu dievaluasi. Jangan simpan service account, private key, atau password di GitHub.
- Versi ini **belum memverifikasi identitas murid**. Siapa pun yang membuka situs dapat memilih nama siapa saja dalam roster dan mengirim absensi atas namanya. Sistem mencegah duplikasi dokumen pada server, **bukan peniruan identitas**. Untuk penggunaan resmi yang mensyaratkan keabsahan absensi, gunakan autentikasi murid atau mekanisme verifikasi sekolah di backend.
- Semua pengunjung situs dengan Firebase Anonymous aktif dapat membaca **daftar nama roster**, karena pilihan nama di Portal Murid mengandalkan data ini. Terapkan kebijakan privasi sekolah dan izin yang relevan, atau rancang ulang pemilihan siswa bila daftar tidak boleh terekspos.
- Catatan `Sakit` atau `Izin` berpotensi memuat informasi pribadi. Jangan masukkan diagnosis atau data sensitif yang tidak diperlukan.
- Firestore tidak otomatis mengirim data lama dari `localStorage`. Versi ini menggunakan koleksi Firestore root `roster_data` dan `attendance_records` (bukan path `artifacts/.../public/data/...`). Bila sebelumnya ada data pada path lain, perlu migrasi terpisah.
- Aplikasi **menolak penyimpanan saat belum tersambung ke Firebase**; tidak lagi mengklaim data tersimpan ke cloud padahal hanya tersimpan pada satu perangkat.
- Ekspor berupa **CSV yang dapat dibuka Excel**, bukan berkas XLSX asli.

## Keterbatasan audit

Pemeriksaan sintaks dan unit test lokal tersedia, tetapi **koneksi nyata ke proyek Firebase belum diuji langsung** karena membutuhkan akses dan perubahan pada proyek Anda. Pastikan uji manual tersebut berhasil sebelum diumumkan kepada siswa.
