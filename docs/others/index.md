# Panduan Pemanfaatan Fitur Admonitions di MkDocs Material

Fitur **Admonitions** memungkinkan kita membuat kotak catatan, peringatan, atau status yang menarik secara visual dengan warna dan ikon khusus. Tema *Material for MkDocs* otomatis mengoptimalkan fitur ini agar **bersifat printer-friendly** (warna latar belakang akan disesuaikan saat dicetak untuk menghemat tinta).

---

## 1. Aktivasi Fitur di `mkdocs.yml`

Sebelum menggunakannya, pastikan ekstensi berikut sudah terdaftar di dalam file konfigurasi utama **`mkdocs.yml`** pada bagian `markdown_extensions:`:

```yaml
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
```

*   `admonition`: Mengaktifkan blok kotak berwarna.
*   `pymdownx.details`: Memungkinkan pembuatan kotak yang bisa dibuka-tutup (*collapsible*).
*   `pymdownx.superfences`: Memastikan blok kode di dalam kotak tetap terformat dengan rapi.

---

## 2. Cara Penulisan Sintaks di File Markdown (`.md`)

Gunakan tanda tiga titik dua (`:::`) diikuti dengan **tipe kotak**. Anda bisa menambahkan judul kustom di dalam tanda kutip dua (`""`). Konten di dalam kotak harus diberikan **indentasi 4 spasi**.

### A. Kotak Isu Kritis (`danger` atau `error`)
Cocok untuk menandai *bug* kritikal atau *blocker* transisi Odoo WTI.

```markdown
::: danger "ISU KRITIS: PROSES PAYROLL TERGANGGU"
Temuan pada modul `accounting` ini menyebabkan perhitungan PPh 21 tidak akurat. 
Jangan melakukan merge ke branch **production** sebelum isu ini diselesaikan!
:::
```

### B. Kotak Catatan Teknis / Konfigurasi (`info` atau `note`)
Cocok untuk memberikan tips atau catatan setup server Odoo.sh.

```markdown
::: info "Catatan Konfigurasi Odoo.sh"
Setiap kali melakukan perubahan pada file `__manifest__.py`, pastikan untuk melakukan trigger *Update Apps List* melalui menu Developer Mode di Odoo.
:::
```

### C. Kotak Status Sukses (`success`)
Cocok untuk menandai modul atau skenario yang sudah lolos uji UAT.

```markdown
::: success "UAT Berhasil - Modul Expense"
Skenario pengujian untuk approval bertingkat (Manager -> HRD) telah dicoba oleh tim finance WTI dan dinyatakan **Lolos Uji**.
:::
```

### D. Kotak Buka-Tutup / Collapsible (`details`)
Berguna untuk menyembunyikan data log yang terlalu panjang agar halaman tetap rapi, namun data tersebut **akan otomatis terbuka penuh saat dicetak**.

```markdown
::: details "Klik untuk melihat log query SQL yang bermasalah"
```sql
SELECT id, name, state FROM hr_expense WHERE state = 'draft';
```
:::
```

---

## 3. Daftar Tipe Admonitions yang Tersedia

Anda bisa mengganti kata kunci setelah `::: ` untuk mencocokkan warna dan ikon sesuai kebutuhan:

| Tipe (Kata Kunci) | Warna Utama | Penggunaan yang Disarankan untuk Odoo WTI |
| :--- | :--- | :--- |
| `note` | Biru Muda | Catatan umum proyek |
| `info` | Biru | Informasi penting sistem Odoo.sh |
| `tip` | Hijau Toska | Tips efisiensi / trik kustomisasi kode |
| `success` | Hijau | Isu *Resolved* / Lolos UAT |
| `warning` | Oranye | Peringatan ringan / perlu perhatian segera |
| `failure` / `danger` | Merah | *Bug Blocker* / Gagal fungsi |
