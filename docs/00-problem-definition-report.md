### **Project Background :**

Lembaga keuangan dan bank menghadapi tantangan dalam menilai kelayakan kredit perusahaan.

- **Manual assessment masih dominan** → banyak lembaga masih menggunakan laporan keuangan manual atau penilaian subjektif analis. Hal ini rawan bias dan lambat.
- **Risiko gagal bayar akan tinggi** → tanpa sistem prediksi yang tepat, bank bisa memberi pinjaman pada perusahaan yang berpotensi gagal bayar. Akibatnya: kerugian finansial besar dan meningkatnya angka NPL (Non-Performing Loan).
- **Data tidak terintegrasi** → informasi tentang perusahaan tidak hanya ada di laporan keuangan, tetapi juga pada riwayat kredit dan kondisi makro. Banyak lembaga tidak mampu menggabungkan ini dengan baik.
- **Regulasi makin ketat** → otoritas keuangan menuntut transparansi dalam pengelolaan risiko kredit. Tanpa sistem yang berbasis data, bank sulit memenuhi compliance.

---

### **Problem Statement:**

Bagaimana membangun model prediktif yang dapat menilai kemungkinan suatu perusahaan mengalami default (default_flag = 1) dengan memanfaatkan data profil perusahaan, kondisi finansial, riwayat kredit, serta faktor makroekonomi?

---

### **Solution :**

Sebagai penawar dari masalah di atas, pendekatannya adalah **membangun Credit Risk Scoring Model berbasis data science**:

1. **EDA (Exploratory Data Analysis)** → untuk memahami pola distribusi, missing values, dan outlier. Ini mengurangi blind spot dalam pengambilan keputusan.
2. **Bangun Machine Learning Classification Model**:
    - Logistic Regression untuk baseline yang interpretable.
    - Random Forest / XGBoost untuk akurasi tinggi dan menangkap non-linearitas.
    - Model ini otomatis **menggabungkan data internal (financials, credit history) dan eksternal (macro context)**.
3. **Gunakan Feature Importance & SHAP values** → memberikan transparansi (explainability) ke manajemen & regulator, sehingga mereka tahu faktor utama penyebab default (misal: debt-to-equity tinggi, utilisasi kredit berlebihan, atau sektor dengan risiko makro tinggi).
4. **Validasi Model dengan AUC & F1-score** → memastikan model tidak hanya akurat, tetapi juga seimbang dalam menangani data imbalanced (default vs non-default).
5. **Risk Segmentation (High, Medium, Low Risk)** → hasil model dipakai untuk:
    - **Penawar utama:** mengurangi kerugian kredit macet (karena pinjaman bisa difokuskan ke perusahaan dengan risiko rendah/terkelola).
    - Mempercepat proses approval kredit (otomatisasi keputusan kredit).
    - Meningkatkan trust regulator karena scoring berbasis data yang jelas dan explainable.



