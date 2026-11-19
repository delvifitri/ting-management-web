# TING Management Web (management.ting.id)

Aplikasi Web TING Management dengan halaman **Subscription Management** (100% persis referensi `tingsubscription.netlify.app`).

## 🚀 Fitur Utama
- **Plus Jakarta Sans Font** sesuai sistem `management.ting.id`.
- **Sky Blue Branding** khas TING Management.
- **Top Navbar TING**:
  - Logo TING
  - Switcher Toko: **Toko Pak Bagus** & **Unit Bisnis**
  - Saldo Koin: **🪙 102,446**
  - Dropdown Profil **D** (`Toko Pak Bagus`, `delvi@intelove.com`, Tema Terang, Informasi Bisnis, Riwayat Top Up, Subscription NEW, Pengaturan AI TING, Mulai Ulang Tour, Logout)
- **Sidebar TING**:
  - Toggle Switch **Advanced Mode** ("Upgrade fitur pro")
  - Menu: Ringkasan, Kasir, Daftar Produk, Produk Masuk, Catat Biaya, Atur Pengguna, **Subscription (Active Menu)**
  - Box Affiliate Program
- **Halaman Subscription (3 Tab)**:
  - **Tab 1: Paket Aktif**: Multi-subscription banner (*TING Advance* & *AI TING Assistant* lengkap dengan sisa hari, kuota, progress bar, tombol perpanjang)
  - **Tab 2: Katalog Subscription**: Filter kategori (Semua, POS, AI, Enterprise), 3 kartu paket langganan lengkap dengan harga dan fitur
  - **Tab 3: Riwayat Pembelian Subscription**: 3 transaksi dummy persis referensi (*Testing System Analyst*, *Rp 1*, status *PAID*, badge *ACTIVE PLAN*, tombol *Download Receipt*)
- **Modal Kwitansi Elektronik (Receipt Modal)**: Kwitansi resmi dengan tombol cetak/simpan PDF (`window.print()`).
- **Modal Checkout / Upgrade**: Pembayaran via QRIS, Virtual Account, atau Koin TING.

## 🛠️ Cara Menjalankan
```bash
# Install dependencies
npm install

# Jalankan server development
npm run dev

# Build untuk produksi / Netlify
npm run build
```
