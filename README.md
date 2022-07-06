# Bank Marketing Data Set

## Business Problem Understanding

### Context
Data yang digunakan adalah data marketing campaign dari sebuah bank Portugis. Dimana data ini akan dianalisa dan digunakan untuk memprediksi apakah seorang customer akan melakukan deposit dibank tersebut berdasarkan dari data yang sudah didapatkan.

### Problem Statement
Proses marketing campaign biasanya memakan waktu dan biaya yang tidak sedikit. Perusahaan ingin meningkatkan efisiensi marketing campaign dengan menentukan customer mana yang mempunyai probabilitas tertinggi untuk membuka deposit.

Deposito: Uang yang disimpan dalam rekening yang pada jangka waktu tertentu uang tersebut tidak boleh ditarik nasabah, dan hanya bisa dicairkan pada tanggal jatuh temponya. https://id.wikipedia.org/wiki/Deposito

### Goals
Berdasarkan permasalahan tersebut, perusahaan ingin memiliki pemahaman lebih dari data customer mereka untuk bisa memprediksi kemungkinan seorang customer akan membuka deposit atau tidak.

Perusahaan juga ingin mengetahui faktor apa yang membuat seorang customer mau membuka deposit, sehingga perusahaan dapat membuat marketing campaign yang lebih efisien dalam mendekati customer potensial, dan mengurangi biaya berlebih dalam approach ke calon customer yang ternyata tidak berpotensi membuka deposit.

### Analytic Approach
Pada kasus ini kita akan melakukan analisis data dasar dan machine learning untuk menentukan pola yang membedakan customer yang akan membuka deposit dan yang tidak akan membuka deposit.
Kemudian kita akan membangun model klasifikasi yang akan membantu perusahaan untuk dapat memprediksi probabilitas pembukaan deposit oleh customer.

### Metric Evaluation
<img src=https://pbs.twimg.com/media/FD2hTDOX0AIdRti.jpg>

Type 1 Error : False Positive
</br>Konsekuensi : Terbuangnya waktu dan biaya campaign tanpa adanya pembukaan deposit

Type 2 Error : False Negative 
</br>Konsekuensi : Kehilangan calon customer yang potensial

Berdasarkan konsekuensinya, maka sebisa mungkin kita membuat model yang mengurangi waktu dan biaya dalam campaign, tanpa membuat kurangnya calon customer potensial. Jadi kita harus antara nilai precision dan recall. Oleh karena itu kita akan menggunakan roc_auc sebagai metric utama.

## Data Understanding

Dataset source : https://archive.ics.uci.edu/ml/datasets/bank+marketing

Moro et al., 2014] S. Moro, P. Cortez and P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, Elsevier, 62:22-31, June 2014

Note:
- Data sedikit tidak balance (48:52)
- Kebanyakan datanya berbentuk kategorikal
- Setiap baris menggambarkan data customer yang sudah pernah diapproach dan hasilnya

### Attribute Information
| No. | Attribute | Data Type | About | Description |
| -- | -- | -- | -- | -- |
| 1. | Age | int64 | Client | Client's Age |
| 2. | Job | object | Client | Type of job (admin, blue-collar, entrepreneur, housemaid, management, retired, self-employed, sevices, student, technician, unemployed, unknown) |
| 3. | Balance | int64 | Client | Client's Balance |
| 4. | Housing | object | Client | Has housing loan (Yes, No) |
| 5. | Loan | object | Client | Has personal loan (Yes, No) |
| 6. | Contact | object | Current Campaign | Communication Type (cellular, telephone) |
| 7. | Month | object | Current Campaign | Last contact month of the year |
| 8. | Campaign | int64 | Current Campaign | Number of contacts performed during this campaign and for the client |
| 9. | Pdays | int64 | Previous Campaign | Number of days that passed after the client was last contacted from a previous campaign (999 means client was not previously contacted) |
| 10. | Poutcome | object | Previous Campaign | Outcome of the previous marketing campaign (failure, nonexistent, success) |
| 11. | Deposit | object | Current Campaign | Has the client subscribed a term deposit? (Yes, No) |


### Categorical Features
Kita akan melihat setiap categorical features dan pengaruhnya ke keputusan untuk melakukan deposit atau tidak (yes, no).
<img src=https://github.com/Mare0102/Capstone_3/blob/main/cat_feat.png>

### Numerical Features
Kita juga akan melihat setiap numerical features dan pengaruhnya ke keputusan untuk melakukan deposit atau tidak (yes, no).
<img src=https://github.com/Mare0102/Capstone_3/blob/main/num_feat.png>

## Campaign Efficiency Analysis Based on Job, Month, Age, and Balance
Kita akan melihat lebih jauh pengaruh empat variabel (job, month, age, balance) ke keputusan deposit, dibandingkan dengan contact rate (seberapa banyak kontak dilakukan dengan customer yang ada di grup tersebut).

### By Job
<img src =https://github.com/Mare0102/Capstone_3/blob/main/job_deposit.png>

### By Month
<img src=https://github.com/Mare0102/Capstone_3/blob/main/month_deposit.png>

### By Age
<img src=https://github.com/Mare0102/Capstone_3/blob/main/age_deposit.png>

### By Balance
<img src=https://github.com/Mare0102/Capstone_3/blob/main/balance_deposit.png>


## Model Performance
Kita mencoba beberapa model klasifikasi, dimana yang terbaik adalah RandomForestClassification


| model	| roc_auc_mean | roc_auc_std |
| -- | -- | -- |
| RandomForestClassifier(random_state=42) |	0.723484 | 0.011954 |
| XGBClassifier(base_score=None, booster=None, c...	 | 0.722544 |	0.012995 |
| LogisticRegression(random_state=42)	| 0.684064 | 0.015894 |
| KNeighborsClassifier() | 0.665282	| 0.007403 |
| DecisionTreeClassifier(random_state=42) |	0.607645 | 0.006979 |

## Model Benchmarking: Test Data
Kita coba model tersebut ke data test, ternyata yang terbaik adalah XGBClassifier, Nilai kedua model tersebut di tabel atas juga tidak berbeda jauh. Oleh sebab itu kita akan menggunakan XGBClassifier untuk dituning.

| model | roc_auc |
| -- | -- |
| XGBClassifier |	0.688658 |
| RandomForestClassifier |	0.685601 |
| LogisticRegression |	0.652993 |
| KNeighborsClassifier |	0.636876 |
| DecisionTreeClassifier |	0.605800 |


## Hyperparameter Tuning
Setelah dituning kita mendapatkan bahwa nilai roc_auc naik dari 0.72 menjadi 0.74. Nilai ini bisa diperbaiki lagi dengan menambah variabel tuningnya.

Parameter terbaik adalah:
- subsample : 0.6
- reg_alpha : 0.001
- random_state : 42
- n_estimators : 200
- max_depth : 8
- learning_rate : 0.06999999999
- gamma : 9

## Classification Report

Classification Report Tuned XGB : 
|  | precision | recall | f1-score | support |
|--|--|--|--|--|
| 0 | 0.69 | 0.87 | 0.77 | 815 |
| 1 | 0.80 | 0.57 | 0.67 | 746 |
|   |      |      |      |     |
| accuracy |      | 0.73 | 1561 |
| macro avg | 0.74 |  0.72 | 0.72 | 1561 |
| weighted avg | 0.74 | 0.73 | 0.72 | 1561 |

Berdasarkan hasil classification report, model kita mampu memfilter 87% calon customer yang tidak tertarik  dan tidak akan kita approach, kita juga bisa mendapatkan 57% calon customer yang tertarik untuk membuka deposit dari keseluruhan calon customer yang tertarik.

# **Conclusion**

Tujuan utama dari project ini adalah untuk bisa lebih mendalami faktor yang mempengaruhi seorang calon customer untuk membuka deposit, sehingga meningkatkan efektifitas campaign.

Berdasarkan dari analisa dasar diatas, target customer yang paling responsif adalah yang memiliki ciri-ciri dibawah:
- Pekerjaannya student atau retired
- Berusia <30 atau >60
- Mempunyai balance diatas 1000, semakin tinggi semakin tinggi juga peluangnya

Dengan melakukan analisa lebih lanjut dengan menggunakan XGBClassifier kita bisa memprediksi peluang seorang customer akan membuka deposit atau tidak sebelum dilakukan campaignnya. Kita mendapatkan informasi tambahan, dimana housing loan dan pdays sangat mempengaruhi keputusan customer untuk membuka deposit atau tidak.
</br>
</br>

# **Recommendation**
- Perusahaan bisa melakukan campaign lebih tinggi pada bulan-bulan berikut : Maret, September, Oktober, dan Desember. Dimana rate untuk pembukaan lebih tinggi
- Perusahaan bisa mendapatkan data pribadi lebih banyak dari calon customer sebelum melakukan campaign, namun hanya data-data yang tidak mengganggu kenyamanan customer (contoh: status pernikahan, jumlah tanggungan, kendaraan pribadi, dll). Nantinya bisa dicoba lagi pada project berikutnya agar bisa mendapatkan model yang lebih baik
- Untuk memperbaiki model ini, disarankan untuk menggunakan GridSearchCV dalam melakukan Hyperparameter Tuning agar bisa mendapatkan parameter yang benar-benar terbaik (belum bisa pada project kali ini karena time constraint, untuk running >8 jam)
- Bisa mencoba dengan menggunakan model lain (contoh: SVM, Naive Bayes)


Tableau Link: https://public.tableau.com/views/BankMarketingCampaign_16571264706070/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link
