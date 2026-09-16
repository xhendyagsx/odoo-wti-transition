# Business Requirement Document (BRD)
## Kustomisasi Alur Persetujuan Pengeluaran (Expenses Approval Workflow) - Odoo.sh v19

### 1. Latar Belakang (Background)
Saat ini, modul kustom *Expenses* mengalami kendala ketidaksinkronan alur (*workflow misalignment*) dan penamaan tombol yang membingungkan bagi pengguna. Field penugasan persetujuan masih bersifat manual, dan status dokumen antara sistem inti Odoo dengan catatan log kustom tidak selaras, sehingga meningkatkan risiko *human error* dan kegagalan proses operasional.

### 2. Tujuan (Objective)
* Menyinkronkan alur status (*state workflow*) dokumen antara sistem inti Odoo dengan modul kustom persetujuan berjenjang.
* Mengotomatiskan field persetujuan pertama guna mencegah kesalahan input.
* Memperbaiki penamaan tombol (*button labeling*) agar mencerminkan tindakan akuntansi yang standar dan mudah dipahami oleh pengguna.

### 3. Persyaratan Bisnis & Fungsional (Business & Functional Requirements)

#### 3.1 Otomatisasi & Penguncian Field "Manager Approver"
* **Pengisian Otomatis (Auto-Populate):** Field "Manager Approver" harus langsung terisi otomatis sejak dokumen pengajuan baru dibuat (*Draft/New State*).
* **Dinamis (Tidak Di-hardcode):** Sistem harus mengambil data secara dinamis berdasarkan **Hak Akses/Role (Access Rights Group)** Finance Manager yang aktif, atau berdasarkan struktur departemen keuangan. Jika di masa depan personel Finance Manager berubah (tidak lagi Pak Jefry Ardiansyah), sistem harus otomatis membaca user baru setelah *role* diubah di menu Settings, tanpa perlu mengubah kode.
* **Hak Akses Read-Only:** Field ini wajib di-set sebagai *read-only* bagi karyawan biasa yang membuat dokumen untuk menghindari manipulasi alur persetujuan.

#### 3.2 Perbaikan Tombol dan Sinkronisasi Status Dokumen
* **Perubahan Nama Tombol (Re-labeling):** Tombol **"TARGET ACQUIRED"** wajib diubah namanya menjadi **"Submit to Finance"** atau **"Ajukan Pengeluaran"** agar fungsinya jelas bagi pengguna akhir.
* **Koreksi Alur Logika & Status (Workflow Logic):**
  * **Kondisi Awal (Draft):** Saat dokumen baru dibuat, status pada breadcrumb Odoo adalah `Draft` dan status pada *Approval Workflow* (kiri bawah) harus menunjukkan `Draft` atau `Draft Expense`.
  * **Kondisi Setelah Diklik "Submit to Finance":** Dokumen resmi terkirim. Status breadcrumb kanan atas berubah menjadi `Submitted` dan status kustom di kiri bawah otomatis berubah menjadi `Waiting Finance Approval` (Menunggu Persetujuan Pak Jefry).
  * **Kondisi Setelah Disetujui Tahap 1:** Setelah Pak Jefry memberikan persetujuan, status kustom kiri bawah baru berubah menjadi `Manager Approved` / `Approved by Finance`, dan dokumen otomatis masuk ke antrean persetujuan tahap 2 (Pak Tedhi Achdiana - Owner) untuk proses pembayaran.

### 4. Rekomendasi Teknis untuk SME / Developer
* Gunakan fungsi `_compute_default_approver` pada model `hr.expense` untuk menarik user dengan grup akses khusus (misal: `group_finance_manager`).
* Pastikan fungsi tombol kustom yang saat ini bernama "Target Acquired" memicu *state transition* standar Odoo (`action_submit_expenses`) dan mengubah *state fields* kustom secara bersamaan (atomik/sinkron).
