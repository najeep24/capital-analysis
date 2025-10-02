# Dataset Dictionary

**Dataset:** 50,000 Indonesian corporate borrowers  
**Structure:** 4 tables
**Period**: Setiap baris di dataset konteksnya adalah 1 tahun

## ERD Diagram
![[alt]](https://github.com/najeep24/credit-default-risk-analysis/blob/d1ead5aa18142e3cd18b67009e88e029a5829a36/data/ERD_capital_analysis.jpg)

Entity relationship diagram memperlihatkan relasi one to one untuk semua tabel yang ada, jadi bisa diasumsikan kita tinggal melakukan **left join** saja dan sudah bisa menggabungkan keseluruhan dataset tanpa masalah. ini akan mempermudah analisa khususnya EDA. Jika ada indikasi variabel gabungan yang menarik, maka akan dilakukan feature engineering.

## `company_info.csv`
| Variable               | Type          | Description (Business Meaning)                                                                                                             |
| ---------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **company_id**         | String (UUID) | ID unik untuk setiap perusahaan, digunakan untuk menghubungkan semua tabel.                                                                |
| **company_name**       | String        | Nama perusahaan (legal name) sesuai registrasi di Indonesia.                                                                               |
| **sector**             | Categorical   | Sektor industri tempat perusahaan beroperasi (contoh: manufaktur, konstruksi, teknologi). Penting untuk membandingkan risiko antar sektor. |
| **years_in_operation** | Integer       | Umur perusahaan (berapa tahun sudah berjalan).                                                                                             |
| **audited_financials** | Binary        | Apakah laporan keuangan sudah diaudit. Audit meningkatkan kredibilitas dan kepercayaan pemberi pinjaman.                                   |
| **ownership_type**     | Categorical   | Jenis kepemilikan (contoh: swasta, BUMN, asing). Penting karena profil risiko bisa berbeda antar jenis kepemilikan.                        |

## `financials.csv`

| Variable              | Type   | Description (Business Meaning)                                                                                             |
| --------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------- |
| **company_id**        | String | Kunci penghubung ke tabel profil perusahaan.                                                                               |
| **annual_revenue**    | Float  | Pendapatan tahunan perusahaan. Semakin tinggi, semakin besar kapasitas bayar.                                              |
| **net_profit_margin** | Float  | Persentase laba bersih dibanding pendapatan. Mengukur efisiensi & profitabilitas. Margin negatif = indikasi risiko tinggi. |
| **total_assets**      | Float  | Total aset yang dimiliki perusahaan (tanah, bangunan, mesin, kas). Indikator ukuran perusahaan.                            |
| **total_liabilities** | Float  | Total kewajiban/hutang perusahaan. Rasio hutang tinggi = risiko gagal bayar lebih tinggi.                                  |
| **equity**            | Float  | Selisih aset dengan kewajiban. Equity negatif menandakan kondisi keuangan kritis.                                          |
| **cash_ratio**        | Float  | Likuiditas jangka pendek (kas ÷ kewajiban lancar). Menunjukkan kemampuan perusahaan membayar kewajiban segera.             |

## `credit_history.csv`
| Variable               | Type    | Description (Business Meaning)                                                                                                       |
| ---------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **company_id**         | String  | Kunci penghubung ke tabel profil perusahaan.                                                                                         |
| **existing_loans**     | Integer | Jumlah pinjaman aktif yang dimiliki. Terlalu banyak pinjaman bisa meningkatkan risiko kredit macet.                                  |
| **past_due_days**      | Integer | Jumlah hari keterlambatan pembayaran terlama dalam 12 bulan terakhir. Semakin lama keterlambatan, semakin tinggi risiko gagal bayar. |
| **credit_utilization** | Float   | Persentase pemakaian plafon kredit (0–100%+). Kredit terpakai di atas 90% atau melebihi limit = tanda tekanan keuangan.              |
## `macro_context`
| Variable                    | Type   | Description (Business Meaning)                                                                                          |
| --------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------- |
| **company_id**              | String | Kunci penghubung ke tabel profil perusahaan.                                                                            |
| **sector_avg_default_rate** | Float  | Rata-rata tingkat gagal bayar di sektor tersebut. Sebagai baseline risiko sektor.                                       |
| **gdp_growth**              | Float  | Pertumbuhan ekonomi Indonesia. Pertumbuhan rendah → tekanan pada semua sektor. Pertumbuhan tinggi → peluang lebih baik. |
| **inflation**               | Float  | Tingkat inflasi nasional. Inflasi tinggi menekan margin perusahaan karena biaya meningkat.                              |
