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
