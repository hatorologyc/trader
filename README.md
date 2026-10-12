# Radar Sinyal Forex

Halaman web satu file (`index.html`), ditambah indikator Pine Script untuk TradingView (`radar-sinyal.pine`), yang menghitung skor beli/jual dari enam indikator teknikal (EMA, SMA 50, MACD, RSI, Bollinger, ADX), menguji aturan itu pada data historis, lalu menampilkan stop loss, take profit, ukuran posisi, dan jam transaksi dalam WIB.

Alat bantu analisis, bukan nasihat keuangan. Tidak ada metode yang memastikan arah harga. Percayai hasil uji ulang pada data nyata, bukan skor hari ini.

## Instrumen dan sumber data

| Instrumen | Periode | Sumber |
|---|---|---|
| EUR/USD, GBP/USD, USD/JPY, AUD/USD, USD/IDR | 1 hari | Kurs acuan ECB via Frankfurter (tanpa High/Low) |
| BTC/USDT, ETH/USDT, DOGE/USDT | 5 menit, 1 jam, 4 jam, 1 hari | Binance (data publik) |
| XAU/USD (emas) | 5 menit, 1 jam, 4 jam, 1 hari | Binance PAXGUSDT sebagai pengganti emas |
| Forex dan XAU/USD (opsional) | 5 menit, 1 jam, 4 jam, 1 hari, dengan High/Low | Twelve Data, memakai API key Anda sendiri |

Harga di aplikasi broker dikutip sendiri oleh broker dan bisa berbeda dari sumber di atas. Isi kolom spread dengan angka dari layar broker Anda. Anda juga bisa mengunggah atau menempel CSV (Date, Open, High, Low, Close).

## Memasang di GitHub Pages

1. Buat repositori **publik** baru, misalnya `radar-forex`.
2. Klik **Add file → Upload files**, unggah `index.html` (dan `radar-sinyal.pine` serta `README.md` bila mau), lalu **Commit changes**.
3. Buka **Settings → Pages**. Pada **Build and deployment**, pilih **Deploy from a branch**, cabang `main`, folder `/ (root)`, lalu **Save**.
4. Tunggu 1 sampai 2 menit. Alamatnya `https://<nama-akun>.github.io/radar-forex/`.

Tidak perlu server, kunci API, atau proses build.

## Twelve Data (opsional)

Untuk forex dan emas spot dengan High/Low dan periode intraday, pakai Twelve Data:
1. Daftar gratis di twelvedata.com, lalu salin API key dari dashboard.
2. Tempel di kolom **Kunci API Twelve Data**, lalu klik **Ambil dari Twelve Data**. Pilih instrumen dan periode seperti biasa.
3. Kunci disimpan hanya di browser Anda (localStorage) dan dikirim langsung ke twelvedata.com. Jangan menulis kunci di file `index.html` yang diunggah ke GitHub.

Paket gratis Twelve Data dibatasi 8 permintaan per menit dan 800 per hari. Satu pemuatan memakai 1 kredit. Syarat paket gratisnya untuk pemakaian pribadi (tanpa menampilkan data ke orang lain), jadi pakai halaman ini sendiri dan jangan menampilkan datanya ke orang lain. Instrumen kripto tetap diambil dari Binance. Bila kolom kunci kosong, forex memakai kurs harian ECB.

## TradingView

TradingView tidak menyediakan cara bagi aplikasi luar untuk menulis sinyal atau gambar ke akun Anda. Ada dua cara yang didukung:

**1. Grafik TradingView di halaman.** Klik **Tampilkan grafik TradingView**. Grafik mengikuti instrumen dan periode yang dipilih (RSI, MACD, dan Bollinger sudah terpasang). Halaman memuat skrip dari `s3.tradingview.com` hanya saat tombol ditekan. Untuk emas, simbol yang dipakai `OANDA:XAUUSD`. Bila TradingView menolak menampilkannya, ganti simbol lewat kotak pencarian di dalam grafik.

**2. Indikator Pine Script `radar-sinyal.pine`.** Panah BELI/JUAL, garis SL/TP, tabel skor, dan peringatan muncul langsung di grafik TradingView Anda.
1. Buka grafik di TradingView, lalu buka tab **Pine Editor** di bagian bawah.
2. Pilih **Open → New blank indicator**, hapus isinya, tempel seluruh isi `radar-sinyal.pine`, lalu klik **Add to chart**.
3. Untuk notifikasi otomatis: klik ikon **Alert**, pada **Condition** pilih **Radar Sinyal Forex**, pilih **Sinyal BELI**, **Sinyal JUAL**, atau **Sinyal BELI atau JUAL**, dan atur **Trigger** ke **Once Per Bar Close**. Notifikasi bisa lewat aplikasi, email, atau webhook. Webhook dan jumlah alert aktif bergantung pada paket TradingView Anda.

Angka skor di TradingView bisa sedikit berbeda dari halaman web, karena sumber datanya berbeda (harga TradingView, bukan Binance atau ECB) dan awal perhitungan indikatornya tidak identik. Uji ulang historis hanya ada di halaman web.

## Bila data tidak termuat

- **Pesan "Data internet tidak terjangkau"**: matikan ad-blocker atau VPN yang memblokir `api.binance.com`, `data-api.binance.vision`, atau `api.frankfurter.dev`, atau `api.twelvedata.com`, lalu klik **Ambil data internet**.
- **HTTP 451 atau 403**: Binance menolak wilayah atau alamat jaringan Anda. Coba jaringan lain, atau unggah CSV.
- **Data lama**: kurs ECB terbit sekali per hari kerja (sekitar 21:00 WIB). Data Binance diambil ulang setiap kali Anda menekan tombol atau mengganti instrumen dan periode.

## Cara kerja singkat

- Skor −100 sampai +100. Bobot indikator ikut-tren dan indikator pantulan bergeser menurut ADX.
- Uji ulang masuk di open candle berikutnya, memasang SL/TP dari ATR, menghitung SL lebih dulu bila keduanya kena di candle yang sama, dan mengurangi spread. 1R sama dengan jarak stop loss.
- 30% data terakhir dipisahkan sebagai data uji. Aturan yang untung di data latih tetapi rugi di data uji kemungkinan hanya kebetulan.
- Candle Binance yang belum selesai dibuang agar sinyal tidak berubah-ubah.

## Layanan pihak ketiga

Halaman memanggil API publik gratis dari browser pengunjung (Binance dan Frankfurter). Setiap pemuatan mengirim 1 sampai 3 permintaan Binance, atau 9 permintaan Frankfurter untuk forex. Hindari memuat ulang berulang kali.
