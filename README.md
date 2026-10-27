# AI Infrastructure Calculator

Kalkulator sizing server AI on-premise untuk inference, RAG, fine-tuning, dan pre-training.
Hasilnya berupa spesifikasi GPU, node, CPU, RAM, storage, jaringan, daya, ruang rak, dan estimasi CAPEX,
yang bisa diexport ke Excel dan PDF.

**Aplikasi live:** https://fadliramm.github.io/AI-Infrastructure-Calculator/

## Versi utama

Repo ini adalah **versi utama** (source of truth) kalkulator.

- Semua perubahan dibuat di `index.html` pada repo ini, lalu di-push ke branch `main`.
- GitHub Pages otomatis menerbitkan ulang situs dalam sekitar 1 menit setelah push.
- Salinan lain (artifact di claude.ai, file lokal) diambil dari repo ini dan bisa tertinggal versinya.

## Cara mengubah

```bash
git clone https://github.com/fadliramm/AI-Infrastructure-Calculator.git
```

Edit `index.html`, buka di browser untuk dicek, lalu:

```bash
git commit -am "Deskripsi perubahan"
```

```bash
git push
```

Data referensi yang paling sering diubah (cari di `index.html`):

- `const GPUS`: spesifikasi dan harga indikatif GPU
- `const MODELS`: katalog model (trending Hugging Face per 15 Okt 2026)
- `nodeBase` di fungsi `buildCluster`: harga dasar server per node

## Menjalankan secara lokal

Klik dua kali `index.html`, atau jalankan web server lokal:

```bash
npx serve .
```

## Yang butuh koneksi internet

| Fitur | Sumber |
|---|---|
| Export Excel | SheetJS 0.18.5 dari cdnjs.cloudflare.com |
| Export PDF | jsPDF 2.5.1 + jspdf-autotable 3.8.2 dari cdnjs.cloudflare.com |
| Font Archivo / JetBrains Mono | Google Fonts (tanpa internet memakai font sistem) |
| Isi otomatis dari link Hugging Face | huggingface.co (model di katalog bawaan tetap jalan tanpa internet) |

Perhitungan berjalan sepenuhnya di browser. Data input tidak dikirim ke server mana pun.
Model gated (mis. Llama) perlu menempel `config.json` secara manual.

## Catatan

Angka adalah estimasi teknis untuk tahap pre-sales. Harga bersifat indikatif; validasi dengan vendor dan benchmark sebelum penawaran final.
