# Dataset Dictionary
Dataset ini berisi data historis perusahaan dari berbagai sektor industri yang mengajukan pembiayaan ke lembaga keuangan.  
Tujuannya adalah untuk memahami faktor-faktor yang memengaruhi kemungkinan terjadinya **kredit bermasalah (default)** berdasarkan profil keuangan, aktivitas pinjaman, serta kondisi ekonomi makro.

Data dikumpulkan dari beberapa sumber internal dan eksternal, termasuk informasi perusahaan, laporan keuangan, histori pinjaman, dan indikator ekonomi nasional.  
Setiap tabel mewakili sumber data berbeda yang saling terhubung melalui `company_id` dan tahun laporan.

## `company_info.csv`

Berisi profil dasar setiap perusahaan, termasuk sektor, jenis kepemilikan, lama beroperasi, dan status audit laporan keuangan.

| Column               | Type                         | Description                                                                                         |
| :------------------- | :--------------------------- | :-------------------------------------------------------------------------------------------------- |
| `company_id`         | string                       | Unique identifier untuk setiap perusahaan.                                                          |
| `company_name`       | string                       | Nama perusahaan (kadang tidak konsisten / typo).                                                    |
| `sector`             | string                       | Sektor industri perusahaan, misal: Manufacturing, Trade, Services, Tech, Construction, Agriculture. |
| `ownership_type`     | string                       | Jenis kepemilikan: Private, Public, State-Owned (kadang case tidak konsisten).                      |
| `years_in_operation` | int                          | Lama perusahaan beroperasi dalam tahun.                                                             |
| `audited_financials` | binary (0/1 atau “Yes”/“No”) | Menunjukkan apakah laporan keuangan telah diaudit.                                                  |
| `audit_flag`         | binary                       | Flag audit tambahan, dihasilkan dari kolom sebelumnya.                                              |
| `sector_risk`        | float                        | Skor risiko sektor (0–1) dari data lookup sektor.                                                   |
| `default_flag`       | binary                       | Label target (0 = non-default, 1 = default). Ground truth untuk modeling.                           |


## `financials.csv`

Memuat data laporan keuangan tahunan, seperti pendapatan, aset, kewajiban, ekuitas, margin laba, serta rasio-rasio finansial utama.

| Column               | Type         | Description                                                                                           |
| :------------------- | :----------- | :---------------------------------------------------------------------------------------------------- |
| `company_id`         | string       | Foreign key mengacu ke `company_info_raw`.                                                            |
| `year`               | int          | Tahun laporan (2022 atau 2023).                                                                       |
| `annual_revenue`     | string/float | Pendapatan tahunan (bercampur IDR dan USD, dengan format bervariasi seperti “4.5M”, “2 million USD”). |
| `total_assets`       | string/float | Total aset perusahaan.                                                                                |
| `total_liabilities`  | string/float | Total kewajiban perusahaan.                                                                           |
| `equity`             | string/float | Modal sendiri (assets - liabilities). Bisa negatif untuk perusahaan rugi.                             |
| `net_profit_margin`  | float        | Rasio profit margin bersih (%).                                                                       |
| `cash_ratio`         | float        | Rasio kas terhadap kewajiban jangka pendek.                                                           |

## `loan_history.csv`
Menyajikan histori pinjaman perusahaan, mencakup jumlah pinjaman aktif, beban kredit dan keterlambatan pembayaran.

| Column               | Type   | Description                                                               |
| :------------------- | :----- | :------------------------------------------------------------------------ |
| `company_id`         | string | Foreign key ke `company_info_raw`.                                        |
| `existing_loans`     | int    | Jumlah pinjaman aktif.                                                    |
| `loan_amount`        | float  | Total nominal pinjaman aktif.                                             |
| `past_due_days`      | int    | Jumlah hari keterlambatan pembayaran terakhir.                            |
| `loan_burden`        | float  | Proporsi beban pinjaman terhadap aset.                                    |
| `credit_utilization` | float  | Persentase penggunaan kredit terhadap limit.                              |


## `macro_economics`
Berisi indikator ekonomi makro tahunan seperti pertumbuhan GDP, inflasi, dan indeks guncangan ekonomi (_macro shock_).

| Column        | Type  | Description                           |
| :------------ | :---- | :------------------------------------ |
| `year`        | int   | Tahun data makro ekonomi (2022–2023). |
| `gdp_growth`  | float | Pertumbuhan GDP nasional (%).         |
| `inflation`   | float | Inflasi tahunan (%).                  |
| `macro_shock` | float | Indeks kejutan ekonomi (0–1).         |

## `sector_lookup`
Menyediakan lookup table sektor industri beserta rata-rata tingkat gagal bayar dan skor risiko sektoral untuk konteks analisis.

|Column|Type|Description|
|:--|:--|:--|
|`sector`|string|Nama sektor industri.|
|`sector_avg_default_rate`|float|Rata-rata tingkat gagal bayar di sektor tersebut.|
|`sector_risk`|float|Skor risiko sektoral (semakin tinggi = lebih berisiko).|


