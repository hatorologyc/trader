# Radar Sinyal Forex

Halaman web satu file (`index.html`) yang menghitung skor beli/jual dari enam indikator teknikal (EMA, SMA 50, MACD, RSI, Bollinger, ADX), menguji aturan itu pada data historis, lalu menampilkan stop loss, take profit, ukuran posisi, dan jam transaksi dalam WIB.

Alat bantu analisis, bukan nasihat keuangan. Tidak ada metode yang memastikan arah harga. Percayai hasil uji ulang pada data nyata, bukan skor hari ini.

## Instrumen dan sumber data

| Instrumen | Periode | Sumber |
|---|---|---|
| EUR/USD, GBP/USD, USD/JPY, AUD/USD, USD/IDR | 1 hari | Kurs acuan ECB via Frankfurter (tanpa High/Low) |
| BTC/USDT, ETH/USDT, DOGE/USDT | 5 menit, 1 jam, 4 jam, 1 hari | Binance (data publik) |
| XAU/USD (emas) | 5 menit, 1 jam, 4 jam, 1 hari | Binance PAXGUSDT sebagai pengganti emas |

Harga di aplikasi broker dikutip sendiri oleh broker dan bisa berbeda dari sumber di atas. Isi kolom spread dengan angka dari layar broker Anda. Anda juga bisa mengunggah atau menempel CSV (Date, Open, High, Low, Close).

## Memasang di GitHub Pages

1. Buat repositori **publik** baru, misalnya `radar-forex`.
2. Klik **Add file → Upload files**, unggah `index.html` (dan `README.md` bila mau), lalu **Commit changes**.
3. Buka **Settings → Pages**. Pada **Build and deployment**, pilih **Deploy from a branch**, cabang `main`, folder `/ (root)`, lalu **Save**.
4. Tunggu 1 sampai 2 menit. Alamatnya `https://<nama-akun>.github.io/radar-forex/`.

Tidak perlu server, kunci API, atau proses build.

## Bila data tidak termuat

- **Pesan "Data internet tidak terjangkau"**: matikan ad-blocker atau VPN yang memblokir `api.binance.com`, `data-api.binance.vision`, atau `api.frankfurter.dev`, lalu klik **Ambil data internet**.
- **HTTP 451 atau 403**: Binance menolak wilayah atau alamat jaringan Anda. Coba jaringan lain, atau unggah CSV.
- **Data lama**: kurs ECB terbit sekali per hari kerja (sekitar 21:00 WIB). Data Binance diambil ulang setiap kali Anda menekan tombol atau mengganti instrumen dan periode.

## Cara kerja singkat

- Skor −100 sampai +100. Bobot indikator ikut-tren dan indikator pantulan bergeser menurut ADX.
- Uji ulang masuk di open candle berikutnya, memasang SL/TP dari ATR, menghitung SL lebih dulu bila keduanya kena di candle yang sama, dan mengurangi spread. 1R sama dengan jarak stop loss.
- 30% data terakhir dipisahkan sebagai data uji. Aturan yang untung di data latih tetapi rugi di data uji kemungkinan hanya kebetulan.
- Candle Binance yang belum selesai dibuang agar sinyal tidak berubah-ubah.

## Layanan pihak ketiga

Halaman memanggil API publik gratis dari browser pengunjung (Binance dan Frankfurter). Setiap pemuatan mengirim 1 sampai 3 permintaan Binance, atau 9 permintaan Frankfurter untuk forex. Hindari memuat ulang berulang kali.
