# [EXP-003] Field Manager Approver pada informasi Workflow Approval berbentuk Drop-down list

## 📌 Ringkasan Informasi

| Atribut | Detail |
| :--- | :--- |
| **ID Temuan** | EXP-003 |
| **Modul Odoo** | Expense (`hr_expense`) |
| **Tanggal Ditemukan** | 17 September 2026 |
| **Ditemukan Oleh** | Tim Tim UAT - WTI |
| **Tingkat Urgensi** | 🔴 High / 🟡 Medium / 🔵 Low |
| **Status** | ⏳ Open / 🔄 In Progress / ✅ Resolved |

---

## 📝 Deskripsi Masalah
Jelaskan secara singkat apa yang terjadi. 
*Contoh:* Ketika user mencoba melakukan *submit* klaim pengeluaran (expense) dengan melampirkan file nota dalam format `.jpg` atau `.pdf` berukuran di atas 2MB, sistem Odoo.sh memunculkan pesan error dan memblokir proses validasi ke manajer.

### Langkah Replikasi (How to Reproduce)
1. Masuk ke modul **Expenses** > **My Expenses**.
2. Klik tombol **Create** dan isi formulir pengeluaran.
3. Unggah lampiran nota (*attachment*) berukuran > 2MB.
4. Klik tombol **Submit to Manager**.

---

## 💻 Log Error / Detail Teknis
*Gunakan bagian ini jika sistem Odoo memunculkan pesan error teknis (Traceback).*

```python
# Contoh Traceback Error dari Log Odoo.sh
Traceback (most recent call last):
  File "/home/odoo/src/odoo/addons/hr_expense/models/hr_expense.py", line 142, in action_submit_expenses
    if any(expense.attachment_ids.file_size > 2097152 for expense in self):
ValueError: Attachment size exceeds the maximum limit allowed for WTI procurement policy.
```

---

## 🎯 Rencana Tindak Lanjut (Action Plan)
* **Analisis:** Batasan ukuran file pada kustomisasi modul `hr_expense` di branch staging terlalu ketat.
* **Solusi/Perbaikan:** 
  1. Naikkan batas limit *attachment* menjadi 5MB pada konfigurasi parameter sistem Odoo.
  2. Tambahkan fungsi kompresi gambar otomatis pada kustomisasi kode XML/Python jika memungkinkan.
* **PIC Penanggung Jawab:** [Nama Developer / Konsultan Odoo]

---

## 🔄 Riwayat Perubahan & Validasi
*Bagian ini diisi saat isu sedang diperbaiki atau setelah selesai diuji ulang.*

* **17 Sep 2026 (Tim IT):** Isu dilaporkan dan direplikasi di branch `staging`.
* **18 Sep 2026 (Developer):** Perbaikan kode di-push ke branch `development` Odoo.sh.
* **19 Sep 2026 (User WTI):** Uji ulang berhasil, status diubah menjadi **Resolved**.
