# Pembaruan data mingguan

Panduan ini dijalankan oleh tugas terjadwal Claude di komputer pemilik setiap Selasa pukul 18.00 WIB (salinan kerja: `C:\Users\fadli\Documents\AI-Infrastructure-Calculator`). Tujuannya menjaga **data referensi** di `index.html` tetap terkini tanpa mengubah logika perhitungan maupun tampilan.

## Aturan umum

- Ubah hanya blok data yang disebut di bawah, beserta tanggal penandanya. Jangan mengubah rumus, struktur HTML/CSS, atau fitur.
- Jangan pernah mengarang SKU, part number, atau angka spesifikasi. Setiap nilai baru harus berasal dari sumber resmi (Hugging Face API/config.json, spec sheet/QuickSpecs/datasheet vendor). Kalau tidak bisa dipastikan, biarkan nilai lama.
- Kalau sebuah SKU atau produk dinyatakan End-of-Sale oleh vendor, ganti ke penggantinya yang resmi dan tambahkan catatan di `notes`/`extraNotes` vendor tersebut.
- Pakai tanggal hari ini (`date -u`, format Indonesia mis. `3 Nov 2026`) untuk semua penanda tanggal yang diperbarui.

## 1. Katalog model (`const MODELS`, `HF_DATE`)

1. Ambil trending:
   - `https://huggingface.co/api/models?pipeline_tag=text-generation&sort=trendingScore&limit=60&expand[]=safetensors&expand[]=downloads`
   - `https://huggingface.co/api/models?pipeline_tag=image-text-to-text&sort=trendingScore&limit=40&expand[]=safetensors&expand[]=downloads`
2. Kelompok `g:'trend'`: sekitar 20 model **resmi** teratas (repo milik pembuat aslinya, mis. Qwen, google, openai, meta-llama, deepseek-ai, zai-org, mistralai, nvidia, LiquidAI, moonshotai, XiaomiMiMo, openbmb). Abaikan fine-tune pihak ketiga, GGUF/AWQ/NVFP4/quantized, uncensored/abliterated, dan repo berunduhan sangat kecil.
3. Untuk tiap model baca `https://huggingface.co/<repo>/resolve/main/config.json` dan total parameter dari `https://huggingface.co/api/models/<repo>?expand[]=safetensors` (`safetensors.total`). Isi field memakai logika yang sama dengan fungsi `parseHFConfig` di `index.html`: `params`, `active` (untuk MoE; pakai angka dari nama seperti `-A3B` bila ada), `layers`, `hidden`, `kvHeads`, `headDim`, `kvLayers`, `swLayers`, `swWindow`, `mla`.
4. Kelompok `g:'pop'` (populer & enterprise) dipertahankan; tambah/hapus hanya bila jelas usang. Model gated (mis. meta-llama) yang config-nya tidak bisa dibaca: pertahankan nilai lama.
5. Pertahankan entri `{id:'custom', ...}` di akhir. `id` harus unik. Perbarui `HF_DATE`.

## 2. GPU (`const GPUS`)

Periksa apakah ada GPU data center baru yang sudah dijual umum dan relevan untuk on-premise (NVIDIA/AMD). Tambahkan hanya bila spesifikasi resmi (VRAM, bandwidth, BF16/FP8 dense TFLOPS, TDP) tersedia. Harga (`price`) adalah estimasi pasar; ubah hanya bila ada perubahan besar yang jelas. Jangan menghapus GPU yang dipakai di pemetaan vendor (`sub`, `platform`).

## 3. SKU BoQ vendor

Periksa dokumen resmi terbaru lalu perbarui konstanta terkait bila ada revisi, EoS, atau model baru:

| Vendor | Konstanta | Sumber |
|---|---|---|
| Cisco | `CISCO` (termasuk `refDate`), `buildBoQCisco` | Spec sheet UCS C845A M8, C880A M8, data sheet C885A M8, Cisco AI POD Ordering Guide, pengumuman EoL Cisco |
| Dell | `DELL`, `DELL_CPU_AMD` | Halaman produk dell.com PowerEdge XE7745/XE9780/XE9785 dan PowerSwitch Z-series |
| HPE | `HPE`, `HPE_*` | HPE QuickSpecs ProLiant Compute DL380a Gen12 dan XD685 |
| Supermicro | `SUPERMICRO`, `SM_*` | Datasheet supermicro.com untuk sistem yang dipakai |

Perbarui teks `ref`/`refDate` vendor dan komentar "Sumber ... Dicek <tanggal>" di atas konstanta tersebut.

## 4. Uji sebelum commit

1. Cek sintaks JavaScript:
   - Jika `node` tersedia: ekstrak blok `<script>` terakhir (tanpa tag) ke file sementara, lalu jalankan `node --check` padanya.
   - Jika tidak ada `node`: sajikan `index.html` lewat server lokal sederhana dan buka di browser bawaan Claude, lalu pastikan tidak ada error di console dan bagian hasil, BoQ, dan dashboard tampil.
   Harus lolos tanpa error.
2. Pastikan dengan `grep` bahwa `const MODELS`, `const GPUS`, `const CISCO`, `const DELL`, `const HPE`, `const SUPERMICRO`, dan `const VENDORS` masih ada, dan setiap `id` di `MODELS` unik.
3. Jika ada pengujian yang gagal dan tidak bisa diperbaiki dengan aman, jangan push. Buat branch `update/<tanggal>` dan push branch itu saja.

## 5. Commit & push

- Jika ada perubahan: commit ke `main` dengan pesan `Pembaruan data mingguan <tanggal>` dan badan pesan berisi ringkasan per bagian (model yang ditambah/dihapus, GPU, SKU vendor yang berubah, beserta sumbernya). Akhiri pesan dengan baris `Co-Authored-By: Claude <noreply@anthropic.com>`. Lalu `git push origin main`.
- Jika tidak ada perubahan: jangan membuat commit kosong.
- Laporan akhir: ringkas apa yang berubah, apa yang dicek tetapi tidak berubah, dan item yang perlu diverifikasi manusia.
