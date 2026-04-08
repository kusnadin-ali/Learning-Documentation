# [Apps Split-It]
> Split-it merupakan aplikasi untuk menjadi kalkulator atau alat bantu untuk menghitung hasil split bill kamu

## 1. Project Overview
**Nama Aplikasi:** Split-it
**Tujuan:** Aplikasi ini dibuat untuk membantu pengguna menghitung pembagian tagihan belanja atau makan bersama secara adil, termasuk perhitungan pajak dan biaya layanan secara proporsional.

---

## 2. Analisis Kebutuhan
### Problem Statement
Ide ini datang dari keresahan saya saat menghitung tagihan teman teman saya saat patungan, karena menghitung patungan secara manual seringkali rumit, terutama jika ada item yang dikonsumsi bersama dan ada pajak/diskon yang harus dibagi rata berdasarkan proporsi pesanan masing-masing orang agar adil.

### User Stories
- **Sebagai Pengguna**, Saya ingin membuat tagihan baru agar dapat mencatat seluruh pengeluaran secara terorganisir.
- **Sebagai Pengguna**, Saya ingin membagi harga setiap item secara individu guna memastikan akurasi beban biaya tiap partisipan.
- **Sebagai Pengguna**, Saya ingin menambahkan komponen pajak dan biaya layanan secara dinamis dan opsional agar perhitungan dapat disesuaikan dengan kebutuhan struk yang bervariasi.
- **Sebagai Pengguna**, Saya ingin melihat ringkasan detail hasil pembagian item untuk memverifikasi total nominal yang harus dibayarkan oleh setiap partisipan.
- **Sebagai Pengguna**, Saya ingin terdapat fitur berbagi rincian dalam format teks agar informasi tagihan mudah disalin dan dibagikan ke grup komunikasi.
- **Sebagai Pengguna**, Saya ingin mengakses riwayat tagihan agar dapat meninjau kembali data pengeluaran yang pernah dibuat sebelumnya.
- **Sebagai Pengguna**, Saya ingin melihat total pengeluaran bulan ini berapa dari tagihan di bulan ini.

---

## 3. Fitur Utama (MVP)
- [ ] **Create New Bill**: Membuat judul acara, Item tagihan dan total pajak/diskon.
- [ ] **Member Management**: Menambahkan orang yang terlibat dengan tagihan.
- [ ] **Itemized Entry**: Input nama item, harga, dan kuantitas.
- [ ] **Grand Total Calculation**: Perhitungan otomatis pembagian pajak dan biaya layanan.
- [ ] **Summary View**: Laporan akhir berisi rincian "Siapa bayar berapa".
- [ ] **Total Spending Money**: Menampilkan total uang yang telah dikeluarkan bulan ini.
- [ ] **Share Summary**: Membagikan rincian tagihan tiap masing masing member yang terlibat
- [ ] **History Bill**: Menampilkan History Bill yang pernah dibuat.

---

## 4. Perancangan Basis Data
### Entity Relationship Diagram (ERD)
![image](https://image2url.com/r2/default/images/1775656794870-face1ae6-68f8-4b5d-9279-3a65356507c0.jpg)

**Penjelasan Singkat:**
- **USERS**: Menyimpan data user yang menggunakan aplikasi.
- **BILLS**: Menyimpan informasi utama tagihan.
- **ITEMS**: Menyimpan detail barang/makanan yang dibeli.
- **PARTICIPANTS**: menyimpan member yang ikut tagihan.
- **BILL_SUMMARY**: menyimpan rincian kalkulasi dari tagihan.
- **ITEM_PARTICIPANT**: Tabel menyimpan relasi antara item dan participan sebagai perantara, karena item dan participant mempunyai relasi many to many.
- **PARTICIPANT_SUMMARY**: Tabel yang menyimpan relasi antara participants dan bill_summary.
- **TAX**: menyimpan informasi Tax dari tagihan.
- **EXTRA_FEE**: menyimpan informasi tax tambahan seperti layanan service dari tagihan.


---

## 5. Business Logic
### Rumus Perhitungan Proporsional
Untuk memastikan keadilan, pajak dibagi berdasarkan persentase total belanja individu terhadap subtotal keseluruhan.

#### A. Alokasi Item & Subtotal Individu ($S_i$)
Sistem akan menghitung total belanja murni tiap individu sebelum biaya tambahan.

**Aturan Validasi:**
1. **Equal Split:** Jika partisipan pada item tersebut $> 1$, harga item dibagi rata ke seluruh partisipan yang ditandai.
2. **Unit Integrity:** Jika $P < Q$ (Partisipan lebih sedikit dari jumlah unit), user wajib memecah item (misal: "Pensil A" dan "Pensil B") agar setiap unit teralokasi ke individu yang tepat.

**Rumus Subtotal Individu:**
$$S_i = \sum_{j=1}^{n} \left( \frac{Harga\_Item_j}{Jumlah\_Partisipan\_Item_j} \right)$$

---

#### B. Kalkulasi Biaya Tambahan (Tax & Extra Fee)
Biaya tambahan bersifat opsional dan dinamis (bisa berupa PPN, Service Charge, atau biaya lainnya).

1. **Total Subtotal Grup ($S_{total}$):**
   $$S_{total} = \sum S_i$$

2. **Total Biaya Tambahan ($E_{total}$):**
   $$E_{total} = \sum (Tax_1 + Tax_2 + Fee_n)$$

3. **Rasio Kontribusi ($R_i$):**
   Digunakan untuk menentukan beban pajak yang adil berdasarkan besarnya pesanan.
   $$R_i = \frac{S_i}{S_{total}}$$

---

#### C. Hasil Akhir (Grand Total Individu)
Total yang harus dibayarkan oleh individu $i$ adalah:
$$Grand\_Total_i = S_i + (R_i \times E_{total})$$

---

#### D. Contoh Simulasi Perhitungan
**Data Input:**
- **A**: Membeli 2 Pensil (@2.000) + 1 Sapu (@10.000)
- **B**: Membeli 1 Pensil (@2.000) + 1 Penghapus (@1.000)
- **Extra**: Pajak 10% + Layanan 5%

**Langkah Perhitungan:**
1. **Subtotal A ($S_A$)**: $4.000 + 10.000 = 14.000$
2. **Subtotal B ($S_B$)**: $2.000 + 1.000 = 3.000$
3. **Subtotal Grup ($S_{total}$)**: $17.000$
4. **Total Extra ($E_{total}$)**: $(10\% \times 17.000) + (5\% \times 17.000) = 1.700 + 850 = 2.550$
5. **Rasio A ($R_A$)**: $14.000 / 17.000 \approx 0,8235$
6. **Rasio B ($R_B$)**: $3.000 / 17.000 \approx 0,1765$
7. **Final A**: $14.000 + (0,8235 \times 2.550) = 16.100$
8. **Final B**: $3.000 + (0,1765 \times 2.550) = 3.450$

---

## 6. Tech Stack
- **Frontend:** React Native dengan NativeWind
- **Backend:** Golang
- **Database:** PostgreSQL
- **Tools:** Docker, Postman, VSCODE

---
