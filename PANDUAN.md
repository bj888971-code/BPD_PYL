# Panduan Membuat Aplikasi SIAP-BPD (GitHub Pages + Google Apps Script)

**Cara kerjanya**
- `index.html` → tampilan aplikasi, di-hosting gratis di **GitHub Pages**.
- `Code.gs` → server (API) di **Google Apps Script**.
- Data tersimpan di **Google Sheet** (satu tab per buku), berkas surat di **Google Drive**.
- `index.html` terhubung ke server lewat **API_URL** (alamat Web App Apps Script).

Yang dibutuhkan: akun Google dan akun GitHub (gratis).

---

## Tahap 1 — Buat Google Sheet
1. Buka <https://sheets.google.com> → **Kosong (Blank)**.
2. Beri nama, misalnya `Database SIAP-BPD Payo Lebar`.

## Tahap 2 — Pasang Code.gs di Apps Script
1. Di Sheet tadi, klik **Ekstensi → Apps Script**.
2. Hapus isi `Code.gs` bawaan, lalu **tempel seluruh isi file `Code.gs`** dari paket ini.
3. Ubah baris berikut menjadi kunci rahasia pilihan Anda (hanya anggota BPD yang tahu):
   ```js
   const KUNCI = 'GANTI-DENGAN-KUNCI-RAHASIA';
   ```
4. Klik ikon **Simpan** (💾).

## Tahap 3 — Beri izin (sekali saja)
1. Di bagian atas editor, pilih fungsi **`otorisasi`**, lalu klik **Jalankan**.
2. Klik **Tinjau izin** → pilih akun Google Anda.
3. Jika muncul peringatan "Google belum memverifikasi aplikasi ini": klik **Lanjutan → Buka … (tidak aman) → Izinkan**. Ini normal karena skrip dibuat sendiri.
4. Setelah selesai, tab untuk semua buku dan folder Drive `Berkas BPD Payo Lebar` terbentuk otomatis.

## Tahap 4 — Deploy sebagai Web App (menghasilkan API_URL)
1. Klik **Terapkan (Deploy) → Penerapan baru (New deployment)**.
2. Klik ikon roda gigi ⚙️ → pilih jenis **Aplikasi web (Web app)**.
3. Isi:
   - **Jalankan sebagai (Execute as):** *Saya (Me)*
   - **Yang memiliki akses (Who has access):** *Siapa saja (Anyone)*
4. Klik **Terapkan**, lalu **salin URL Aplikasi Web** yang berakhiran `/exec`.
   Inilah **API_URL** Anda, contoh:
   `https://script.google.com/macros/s/AKfycb.../exec`

> Akses "Siapa saja" aman karena semua permintaan wajib membawa **KUNCI**; tanpa kunci server menolak.

## Tahap 5 — Sematkan API_URL di index.html
1. Buka file `index.html` dengan Notepad/VS Code (atau langsung di GitHub, tahap 6).
2. Cari baris:
   ```js
   const API_URL='GANTI_DENGAN_URL_WEB_APP_ANDA';
   ```
3. Ganti dengan URL dari Tahap 4:
   ```js
   const API_URL='https://script.google.com/macros/s/AKfycb.../exec';
   ```
4. Simpan. **Jangan menulis KUNCI di file ini** — kunci diketik saat pertama kali membuka aplikasi.

## Tahap 6 — Unggah ke GitHub & aktifkan GitHub Pages
1. Masuk ke <https://github.com> → **New repository**.
   - Nama: misalnya `siap-bpd`
   - Pilih **Public** (GitHub Pages gratis memerlukan repo publik) → **Create repository**.
2. Klik **uploading an existing file**, unggah **`index.html`** (opsional: `Code.gs` dan `PANDUAN.md` sebagai arsip — pastikan `KUNCI` di `Code.gs` **sudah diganti/dikosongkan** bila ikut diunggah, karena repo publik).
3. Klik **Commit changes**.
4. Buka **Settings → Pages**:
   - **Source:** *Deploy from a branch*
   - **Branch:** `main` — folder `/ (root)` → **Save**.
5. Tunggu 1–2 menit. Alamat aplikasi Anda: `https://NAMA-GITHUB.github.io/siap-bpd/`

## Tahap 7 — Pemakaian
1. Buka alamat aplikasi → masukkan **kunci akses** (tersimpan di perangkat, bisa dikeluarkan dengan tombol 🔒 Keluar).
2. Pilih buku di menu, klik **＋ Tambah**, isi, **Simpan**.
3. Data langsung masuk ke Google Sheet; berkas surat masuk ke folder Drive (maks. 5 MB per berkas).
4. Aplikasi bisa dibuka dari HP/laptop mana pun; tambahkan ke layar utama HP agar seperti aplikasi.

---

## Jika Code.gs diubah kemudian
Setiap perubahan kode **harus diterapkan ulang**: **Terapkan → Kelola penerapan → ✏️ → Versi: Versi baru → Terapkan**. URL tetap sama, jadi `index.html` tidak perlu diubah.

## Mengganti kunci akses
Ubah `KUNCI` di `Code.gs` → terapkan versi baru (lihat di atas). Semua perangkat akan diminta memasukkan kunci baru.

## Pemecahan masalah
| Gejala | Penyebab / Solusi |
|---|---|
| "API_URL belum diisi" | Tahap 5 belum dilakukan atau belum di-commit ke GitHub. |
| "Kunci akses salah" | Kunci berbeda dengan `KUNCI` di Code.gs, atau perubahan belum diterapkan sebagai versi baru. |
| "Tidak dapat terhubung ke server" | Akses Web App belum **Siapa saja**, URL salah (harus berakhiran `/exec`), atau izin Tahap 3 belum diberikan. |
| Berkas tidak terunggah | Ukuran > 5 MB, atau izin Drive belum diberikan (ulangi Tahap 3). |
| Perubahan di GitHub belum tampak | Tunggu 1–2 menit lalu muat ulang paksa (Ctrl + Shift + R). |

## Catatan keamanan
- Repo GitHub publik berarti `API_URL` terlihat orang lain; data tetap terlindungi oleh **KUNCI**, jadi pilih kunci yang panjang dan tidak mudah ditebak.
- Data penduduk (misalnya NIK anggota) tersimpan di Google Sheet milik Anda — batasi siapa yang diberi akses ke Sheet tersebut.
- Lakukan cadangan berkala: **File → Unduh → Excel (.xlsx)** di Google Sheet.
