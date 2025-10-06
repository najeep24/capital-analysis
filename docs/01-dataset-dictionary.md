# Data Dictionary
Dataset ini berisi informasi kredit perusahaan yang mencakup data profil perusahaan, laporan keuangan, riwayat pinjaman, histori pembayaran, serta kondisi makroekonomi. Data dikumpulkan dari berbagai sumber untuk mendukung analisis risiko kredit dan pengambilan keputusan pemberian pinjaman.

Dataset terdiri dari 7 tabel yang saling terkait melalui ID perusahaan, mencerminkan struktur data yang umum ditemukan dalam sistem informasi kredit perbankan. Setiap tabel merepresentasikan aspek berbeda dari profil risiko kredit suatu perusahaan.

**Struktur Dataset:**

- **company_info_raw**: Data identitas dan profil dasar perusahaan
- **financials_raw**: Laporan keuangan tahunan (2022-2023)
- **loan_history_raw**: Riwayat pinjaman yang pernah diajukan
- **repayment_history_raw**: Catatan pembayaran cicilan bulanan
- **macro_economics_raw**: Data kondisi ekonomi makro per sektor
- **sector_lookup**: Referensi tingkat risiko per sektor industri
- **ground_truth**: Label aktual status kredit macet (untuk evaluasi model)

## `company_info`
Tabel master berisi informasi profil dasar perusahaan peminjam, termasuk identitas, sektor usaha, bentuk badan hukum, dan lama operasional.

| Column             | Type   | Example Value     | Description                                                                       |
| ------------------ | ------ | ----------------- | --------------------------------------------------------------------------------- |
| company_id         | String | C00001            | ID unik perusahaan                                                                |
| company_name       | String | PT Nusantara Maju | Nama legal perusahaan                                                             |
| sector             | String | Manufacturing     | Sektor industri (Manufacturing, Trade, Services, Agriculture, Construction, Tech) |
| ownership_type     | String | PT                | Bentuk badan hukum (PT, CV, Firma, BUMN, Koperasi)                                |
| years_in_operation | String | 5, >10            | Lama tahun beroperasi                                                             |
| audited_financials | String | Yes, No           | Apakah laporan keuangan diaudit atau tidak                                        |
| default_flag       | int    | 0,1               | Status kredit macet aktual (0=tidak macet, 1=macet)                               |


### Data Quality

| column              | dtype                     | count | %_missing | unique_values | min  | max  | %_zeros | str_in_numeric |
|---------------------|---------------------------|--------|------------|----------------|------|------|----------|----------------|
| company_id          | object                    | 30000  | 0.00       | 30000          | NaN  | NaN  | 0.00     | 0              |
| company_name        | object                    | 30000  | 0.00       | 256            | NaN  | NaN  | 0.00     | 0              |
| sector              | object                    | 30000  | 0.00       | 4677           | NaN  | NaN  | 0.00     | 0              |
| ownership_type      | object                    | 27600  | 8.00       | 588            | NaN  | NaN  | 0.00     | 0              |
| years_in_operation  | object (possible numeric) | 30000  | 0.00       | 36             | 1.0  | 22.0 | 0.00     | 26445          |
| audited_financials  | object (possible numeric) | 28522  | 4.93       | 7              | 1.0  | 1.0  | 0.00     | 568            |
| default_flag        | int64                     | 30000  | 0.00       | 2              | 0.0  | 1.0  | 70.83    | 0              |

## `financials`
Laporan keuangan tahunan perusahaan yang mencakup pendapatan, aset, liabilitas, ekuitas, margin keuntungan, dan rasio kas. Data tersedia untuk tahun 2022 dan 2023.

|Column|Type|Example Value|Description|
|---|---|---|---|
|company_id|String|C00001|ID perusahaan (relasi ke company_info)|
|year|Integer|2023|Tahun laporan keuangan|
|annual_revenue|String|5.2M IDR, 100,000 USD|Pendapatan tahunan (format mixed currency)|
|total_assets|String|15M IDR, 500k USD|Total aset perusahaan|
|total_liabilities|String|8.5M IDR|Total kewajiban/utang|
|equity|String|6.5M IDR|Ekuitas/modal perusahaan|
|net_profit_margin|String|8.5%, 500,000 IDR|Margin keuntungan bersih (% atau nilai absolut)|
|cash_ratio|String|0.45|Rasio kas terhadap kewajiban jangka pendek|

### Data Quality

| column             | dtype                     | count | %_missing | unique_values | min       | max   | %_zeros | str_in_numeric |
|--------------------|---------------------------|--------|------------|----------------|-----------|--------|----------|----------------|
| company_id         | object                    | 57600  | 0.00       | 29961          | NaN       | NaN   | 0.00     | 0              |
| year               | int64                     | 57600  | 0.00       | 2              | 2022.000  | 2023.0| 0.00     | 0              |
| annual_revenue     | object (possible numeric) | 51344  | 10.86      | 14914          | 1.001     | 999.0 | 0.00     | 3918           |
| total_assets       | object (possible numeric) | 50866  | 11.69      | 19778          | 1.004     | 997.0 | 0.00     | 2155           |
| total_liabilities  | object (possible numeric) | 49726  | 13.67      | 14713          | 0.000     | 999.0 | 0.04     | 4118           |
| equity             | object (possible numeric) | 49155  | 14.66      | 18985          | -891.000  | 999.0 | 0.00     | 2893           |
| net_profit_margin  | object (possible numeric) | 54153  | 5.98       | 6657           | -992.000  | 998.0 | 0.04     | 7248           |
| cash_ratio         | float64                   | 54108  | 6.06       | 1868           | 0.003     | 2.0   | 0.00     | 0              |


## `loan_history`
Riwayat semua pinjaman yang pernah diajukan oleh perusahaan, mencakup jumlah pinjaman, suku bunga, jangka waktu, dan status keterlambatan pembayaran.

| Column              | Type           | Example Value     | Description                             |
| ------------------- | -------------- | ----------------- | --------------------------------------- |
| loan_id             | String         | L000001           | ID unik pinjaman                        |
| company_id          | String         | C00001            | ID perusahaan peminjam                  |
| loan_amount         | String         | 2.5M IDR, 50k USD | Jumlah pinjaman                         |
| interest_rate       | String         | 12.5%             | Suku bunga per tahun                    |
| term_months         | Integer        | 36                | Jangka waktu pinjaman (bulan)           |
| start_date          | String         | 15-03-2022        | Tanggal mulai pinjaman                  |
| existing_loans_desc | String         | 3 loans, one      | Deskripsi jumlah pinjaman aktif         |
| past_due_days       | Integer/String | 45, 0             | Jumlah hari keterlambatan pembayaran    |
| credit_utilization  | String         | 75.5%             | Persentase utilisasi kredit             |
| default_flag        | Integer/String | 0, 1              | Status kredit macet (0=lancar, 1=macet) |

### Data Quality

| column                | dtype                     | count | %_missing | unique_values | min     | max    | %_zeros | str_in_numeric |
|------------------------|---------------------------|--------|------------|----------------|---------|--------|----------|----------------|
| loan_id               | object                    | 33640  | 0.00       | 33640          | NaN     | NaN    | 0.00     | 0              |
| company_id            | object                    | 33640  | 0.00       | 18523          | NaN     | NaN    | 0.00     | 0              |
| loan_amount           | object (possible numeric) | 31005  | 7.83       | 7934           | 1.007   | 999.0  | 0.00     | 4263           |
| interest_rate         | object                    | 33640  | 0.00       | 2262           | NaN     | NaN    | 0.00     | 0              |
| term_months           | int64                     | 33640  | 0.00       | 5              | 12.000  | 60.0   | 0.00     | 0              |
| start_date            | object                    | 32603  | 3.08       | 1944           | NaN     | NaN    | 0.00     | 0              |
| existing_loans_desc   | object (possible numeric) | 33640  | 0.00       | 17             | 1.000   | 8.0    | 0.00     | 19113          |
| past_due_days         | float64                   | 32650  | 2.94       | 238            | 0.000   | 5850.0 | 77.73    | 0              |
| credit_utilization    | object                    | 31946  | 5.04       | 7084           | NaN     | NaN    | 0.00     | 0              |
| default_flag          | float64                   | 20243  | 39.82      | 2              | 0.000   | 1.0    | 39.17    | 0              |


## `repayment_history`

Catatan detail pembayaran cicilan bulanan untuk setiap pinjaman, termasuk jumlah yang harus dibayar, jumlah yang terbayar, dan keterlambatan.

| Column        | Type    | Example Value | Description                             |
| ------------- | ------- | ------------- | --------------------------------------- |
| loan_id       | String  | L000001       | ID pinjaman (relasi ke loan_history)    |
| month         | String  | 2023-06       | Bulan periode pembayaran                |
| amount_due    | Integer | 5000000       | Jumlah cicilan yang harus dibayar (IDR) |
| amount_paid   | Integer | 5000000, 0    | Jumlah yang terbayar (IDR)              |
| days_past_due | Integer | 0, 15, 60     | Hari keterlambatan pembayaran           |

### Data Quality

| column        | dtype  | count  | %_missing | unique_values | min      | max       | %_zeros | str_in_numeric |
| ------------- | ------ | ------ | --------- | ------------- | -------- | --------- | ------- | -------------- |
| loan_id       | object | 102845 | 0.0       | 5000          | NaN      | NaN       | 0.00    | 0              |
| month         | object | 102845 | 0.0       | 24            | NaN      | NaN       | 0.00    | 0              |
| amount_due    | int64  | 102845 | 0.0       | 102311        | 200021.0 | 9999974.0 | 0.00    | 0              |
| amount_paid   | int64  | 102845 | 0.0       | 89915         | 0.0      | 9999974.0 | 12.17   | 0              |
| days_past_due | int64  | 102845 | 0.0       | 6             | 0.0      | 120.0     | 37.74   | 0              |

## `macro_economics`
Data kondisi ekonomi makro agregat per sektor industri, mencakup pertumbuhan GDP, inflasi, dan tingkat kredit macet sektoral.

| Column              | Type         | Example Value | Description                                         |
| ------------------- | ------------ | ------------- | --------------------------------------------------- |
| year                | Integer      | 2023          | Tahun data ekonomi                                  |
| sector              | String       | Manufacturing | Sektor industri                                     |
| gdp_growth          | Float/String | 5.2           | Pertumbuhan GDP (%)                                 |
| inflation           | Float/String | 3.8           | Tingkat inflasi (%)                                 |
| sector_default_rate | String       | 0.045         | Rata-rata tingkat kredit macet di sektor (proporsi) |

### Data Quality

| column              | dtype   | count | %_missing | unique_values | min       | max       | %_zeros | str_in_numeric |
|---------------------|---------|--------|------------|----------------|-----------|-----------|----------|----------------|
| year                | int64   | 12     | 0.0        | 2              | 2022.000  | 2023.000  | 0.0      | 0              |
| sector              | object  | 12     | 0.0        | 11             | NaN       | NaN       | 0.0      | 0              |
| gdp_growth          | float64 | 12     | 0.0        | 12             | 3.397704  | 6.992680  | 0.0      | 0              |
| inflation           | float64 | 12     | 0.0        | 12             | 0.494785  | 6.384949  | 0.0      | 0              |
| sector_default_rate | float64 | 12     | 0.0        | 10             | 0.020000  | 0.058000  | 0.0      | 0              |

## `sector_lookup`
Tabel referensi yang berisi profil risiko standar untuk setiap sektor industri, digunakan sebagai benchmark penilaian risiko.

| Column                  | Type   | Example Value | Description                                               |
| ----------------------- | ------ | ------------- | --------------------------------------------------------- |
| sector                  | String | Manufacturing | Nama sektor industri                                      |
| sector_risk             | Float  | 0.20          | Skor risiko sektor (0-1, semakin tinggi semakin berisiko) |
| sector_avg_default_rate | Float  | 0.035         | Rata-rata historis tingkat kredit macet di sektor         |

### Data Quality

| column                   | dtype   | count | %_missing | unique_values | min    | max    | %_zeros | str_in_numeric |
|--------------------------|---------|--------|------------|----------------|--------|--------|----------|----------------|
| sector                   | object  | 6      | 0.0        | 6              | NaN    | NaN    | 0.0      | 0              |
| sector_risk              | float64 | 6      | 0.0        | 6              | 0.150  | 0.350  | 0.0      | 0              |
| sector_avg_default_rate  | float64 | 6      | 0
