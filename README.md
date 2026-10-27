# Kalkulator Sizing Server AI On-Premise

Aplikasi satu file (`index.html`) yang bisa dijalankan tanpa instalasi.

## Cara menjalankan

**Cara 1: buka langsung**

Klik dua kali `index.html`. File akan terbuka di Chrome, Edge, atau Firefox.

**Cara 2: lewat web server lokal (opsional, misalnya untuk dipakai bersama di jaringan kantor)**

```bash
npx serve .
```

Bisa juga dengan `python -m http.server 8080`, lalu buka `http://localhost:8080`.
Untuk dipakai banyak orang, salin `index.html` ke web server internal (IIS, Nginx, Apache).

## Yang butuh koneksi internet

| Fitur | Sumber |
|---|---|
| Export Excel | SheetJS 0.18.5 dari cdnjs.cloudflare.com |
| Export PDF | jsPDF 2.5.1 + jspdf-autotable 3.8.2 dari cdnjs.cloudflare.com |
| Font Archivo / JetBrains Mono | Google Fonts (tanpa internet akan memakai font sistem) |
| Isi otomatis dari link Hugging Face | huggingface.co (model di katalog bawaan tetap jalan tanpa internet) |

Perhitungan sizing berjalan sepenuhnya di browser. Data input tidak dikirim ke server mana pun.

## Perbedaan dengan versi di claude.ai

- Export langsung mengunduh file `.xlsx` / `.pdf` ke folder Downloads.
- Isi otomatis dari link Hugging Face mengambil `config.json` langsung, termasuk model yang tidak ada di katalog.
  Model gated (mis. Llama) tetap perlu ditempel manual karena butuh login Hugging Face.

## Mengubah data referensi

Buka `index.html` di editor teks, lalu cari:

- `const GPUS` untuk spesifikasi dan harga indikatif GPU
- `const MODELS` untuk katalog model (trending Hugging Face per 15 Okt 2026)
- `nodeBase` di fungsi `buildCluster` untuk harga dasar server per node
