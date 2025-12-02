Kelompok 1 
ERGI ADITYA (701230021)
KHURUL NAINI IKLIMA (701230021)
ADHITYA APRILIANDI (7001230073)

Restock Management System
Sistem Manajemen Stok Barang berbasis web dengan PHP, MySQL, dan Bootstrap.

Fitur Utama
Autentikasi & Keamanan
Login/Logout sistem
Level akses (Admin/User)
Password hashing dengan bcrypt
Session management

Manajemen Barang
CRUD data barang
Kategori barang
Supplier management
Stok minimum notifikasi
Generate kode barang otomatis

Transaksi
Barang masuk (restock)
Barang keluar (penjualan)
Riwayat transaksi
Validasi stok saat pengeluaran

Laporan & Dashboard
Dashboard statistik real-time
Grafik stok barang
Grafik transaksi bulanan
Laporan stok barang
Notifikasi stok menipis

Tools Tambahan
Kalkulator pintar
Format Rupiah otomatis

Struktur File
restock-management/
│
├── config.php              # Konfigurasi database dan fungsi helper
├── index.php               # Halaman redirect utama
├── login.php               # Halaman login
├── logout.php              # Proses logout
├── dashboard.php           # Dashboard utama dengan statistik
│
├── barang.php              # Manajemen data barang
├── barang_masuk.php        # Input barang masuk/restock
├── barang_keluar.php       # Input barang keluar
│
├── kategori.php            # Manajemen kategori barang
├── supplier.php            # Manajemen data supplier
├── pengguna.php            # Manajemen pengguna (admin only)
│
├── laporan.php             # Laporan stok barang
├── calculator.php          # Kalkulator lengkap
├── tambah_barang.php       # (Legacy) tambah barang
│
└── README.md               # Dokumentasi ini

cara Instalasi
Prasyaratan
PHP 7.4 atau lebih baru
MySQL 5.7 atau lebih baru
Web server (Apache/Nginx)
Composer (opsional)

Langkah Instalasi
1.Clone Repository
git clone https://github.com/username/restock-management.git
cd restock-management

2.Import Database
Buat database baru di MySQL
Import file SQL (jika ada) atau buat tabel sesuai struktur di config.php
Contoh struktur tabel ada dalam komentar kode

3.Konfigurasi Database
Edit file config.php: 
$host = 'localhost';
$db   = 'restock_management';
$user = 'username';
$pass = 'password';

4.Buat Tabel Database
Jalankan SQL berikut di phpMyAdmin atau MySQL CLI:
-- Tabel users
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    nama VARCHAR(100) NOT NULL,
    role ENUM('admin','user') DEFAULT 'user',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabel kategori
CREATE TABLE kategori (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama_kategori VARCHAR(100) NOT NULL
);

-- Tabel supplier
CREATE TABLE supplier (
    id INT AUTO_INCREMENT PRIMARY KEY,
    kode_supplier VARCHAR(20) UNIQUE NOT NULL,
    nama_supplier VARCHAR(100) NOT NULL,
    alamat TEXT,
    telepon VARCHAR(20)
);

-- Tabel barang
CREATE TABLE barang (
    id INT AUTO_INCREMENT PRIMARY KEY,
    kode_barang VARCHAR(20) UNIQUE NOT NULL,
    nama_barang VARCHAR(200) NOT NULL,
    kategori_id INT,
    supplier_id INT,
    stok INT DEFAULT 0,
    stok_minimum INT DEFAULT 10,
    harga_beli DECIMAL(12,2) DEFAULT 0,
    harga_jual DECIMAL(12,2) DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (kategori_id) REFERENCES kategori(id),
    FOREIGN KEY (supplier_id) REFERENCES supplier(id)
);

-- Tabel barang_masuk
CREATE TABLE barang_masuk (
    id INT AUTO_INCREMENT PRIMARY KEY,
    barang_id INT NOT NULL,
    jumlah INT NOT NULL,
    harga_satuan DECIMAL(12,2) DEFAULT 0,
    total_harga DECIMAL(12,2) DEFAULT 0,
    tanggal_masuk DATE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (barang_id) REFERENCES barang(id)
);

-- Tabel barang_keluar
CREATE TABLE barang_keluar (
    id INT AUTO_INCREMENT PRIMARY KEY,
    barang_id INT NOT NULL,
    jumlah INT NOT NULL,
    harga_satuan DECIMAL(12,2) DEFAULT 0,
    total_harga DECIMAL(12,2) DEFAULT 0,
    tanggal_keluar DATE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (barang_id) REFERENCES barang(id)
);

-- Tabel audit_log (opsional)
CREATE TABLE audit_log (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    action VARCHAR(50),
    table_name VARCHAR(50),
    record_id INT,
    old_values JSON,
    new_values JSON,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

5.Akses Aplikasi
Buka browser
Akses: http://localhost/restock-management
Login dengan:
Username: admin
Password: password
