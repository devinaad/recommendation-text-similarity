# 📘 Sistem Rekomendasi Course Berdasarkan Job Title

## 🔍 Deskripsi Proyek
Proyek ini membangun **sistem rekomendasi course** yang dapat memberikan saran kursus online yang tepat berdasarkan **judul pekerjaan (job title)** yang diinputkan. Bayangkan seperti aplikasi yang bisa mengatakan "Jika kamu ingin jadi Data Scientist, sebaiknya ambil kursus A, B, dan C".

### ✅ Tujuan:
- Memberikan rekomendasi course yang relevan dengan pekerjaan tertentu
- Membantu pengguna menemukan course yang sesuai untuk meningkatkan skill yang dibutuhkan di bidang pekerjaan mereka

---

## 🧠 Cara Kerja Sistem (Step-by-Step)

### 1. **Pengumpulan Data**
- **Data Job**: Daftar nama pekerjaan beserta deskripsi tugasnya
- **Data Course**: Informasi kursus dari hasil scraping website Coursera (judul, deskripsi, kategori)

### 2. **Preprocessing (Pembersihan Data)**
- Membersihkan teks dari karakter tidak perlu
- Mengubah semua teks menjadi huruf kecil
- Menghapus kata-kata yang tidak bermakna (stopwords)

### 3. **Vektorisasi Teks (Mengubah Kata Jadi Angka)**
Karena komputer hanya mengerti angka, kita perlu mengubah deskripsi job dan course menjadi representasi numerik (vektor). 

**Model yang digunakan:** `sentence-transformers/paraphrase-MiniLM-L6-v2` (SBERT)

**Mengapa SBERT?**
- **Pemahaman Konteks Lebih Baik**: SBERT bisa memahami makna kalimat secara utuh, bukan hanya kata per kata
- **Contoh sederhana**: 
  - Kata "bank" dalam "bank data" vs "bank tempat menabung" akan dipahami berbeda oleh SBERT
  - Metode lama seperti TF-IDF akan menganggap kedua "bank" sama
- **Efisien**: Model ini ringan dan cepat untuk dijalankan

### 4. **Perhitungan Similarity & Pemberian Rekomendasi**
Setelah semua teks dijadikan vektor, sistem akan menghitung seberapa mirip antara job dengan course dan memberikan rekomendasi.

---

## 🔬 Eksperimen: Perbandingan 2 Metode Similarity

Dalam proyek ini, saya mencoba **2 metode perhitungan similarity** yang berbeda untuk melihat apakah ada perbedaan dalam hasil rekomendasi:

### 1. **Cosine Similarity (Manual)**
```python
from sklearn.metrics.pairwise import cosine_similarity
similarity_scores = cosine_similarity(job_vectors, course_vectors)
```

**Apa itu Cosine Similarity?**
- Metode untuk mengukur seberapa mirip dua vektor
- Nilainya 0-1, dimana 1 = sangat mirip, 0 = tidak mirip sama sekali
- Seperti mengukur sudut antara dua garis - semakin kecil sudutnya, semakin mirip

### 2. **SBERT Semantic Search (Built-in)**
```python
from sentence_transformers import util
similarity_scores = util.semantic_search(job_vectors, course_vectors)
```

**Apa itu SBERT Semantic Search?**
- Fungsi built-in dari library Sentence Transformers
- Dirancang khusus untuk mencari teks yang paling relevan secara semantik
- Sudah dioptimasi untuk kecepatan dan akurasi

### 📊 **Hasil Perbandingan:**
**kedua metode memberikan hasil yang sama**! 

**Mengapa bisa sama?**
- SBERT semantic search pada dasarnya juga menggunakan cosine similarity di belakang layar
- Perbedaannya hanya pada implementasi dan optimisasi kode
- Ini membuktikan konsistensi dari metode yang dipilih

**Kesimpulan:** Untuk proyek ini, kedua metode bisa digunakan dengan hasil identik. Dipilih cosine similarity karena lebih eksplisit dan mudah dipahami.

---

## 📝 Contoh Hasil Rekomendasi

Input: **"Machine Learning Engineer"**

```
🎯 Rekomendasi Course untuk: Machine Learning Engineer

1. Machine Learning Specialization (Similarity: 0.89)
   - Provider: Coursera
   - Kategori: Data Science

2. Deep Learning Fundamentals (Similarity: 0.84)
   - Provider: Coursera  
   - Kategori: Artificial Intelligence

3. Applied Data Science with Python (Similarity: 0.78)
   - Provider: Coursera
   - Kategori: Programming

4. Introduction to Neural Networks (Similarity: 0.75)
   - Provider: Coursera
   - Kategori: Machine Learning

5. Statistics for Data Science (Similarity: 0.72)
   - Provider: Coursera
   - Kategori: Mathematics
```

---

## 📈 Evaluasi Model

### ✅ **Kelebihan:**
- Rekomendasi cukup akurat untuk job title yang umum (Data Scientist, Software Engineer, dll)
- Sistem berjalan cepat dan responsif
- Model SBERT memberikan understanding semantik yang baik

### ⚠️ **Kendala & Keterbatasan:**
1. **Data course masih terbatas** - Hasil scraping Coursera belum mencakup semua kategori kursus
2. **Bias terhadap job populer** - Rekomendasi paling baik hanya untuk pekerjaan IT/Tech yang umum
3. **Deskripsi course kurang detail** - Beberapa course memiliki deskripsi terlalu singkat sehingga similarity kurang optimal

### 🚀 **Saran Perbaikan:**
- **Ekspansi data**: Scraping lebih banyak platform course (Udemy, edX, LinkedIn Learning)
- **Enrichment deskripsi**: Menambah informasi detail tentang course (syllabus, prerequisites)
- **Filter tambahan**: Implementasi filter berdasarkan level, durasi, dan rating
- **Feedback system**: Menambah mekanisme user rating untuk memperbaiki rekomendasi

---

## 🛠️ Tech Stack

- **Python 3.10+**
- **Sentence Transformers** - untuk text embedding
- **Scikit-learn** - untuk cosine similarity
- **Pandas** - untuk manipulasi data
- **BeautifulSoup** - untuk web scraping
- **Streamlit/Flask** - untuk web interface (opsional)

---

## 📚 Referensi & Resources

- [Sentence Transformers Documentation](https://www.sbert.net/)
- [Understanding Cosine Similarity](https://en.wikipedia.org/wiki/Cosine_similarity)
- [Course Data Source: Coursera](https://www.coursera.org/)

---

*Proyek ini dibuat sebagai bagian dari pembelajaran Machine Learning dan Natural Language Processing. Feedback dan saran sangat diterima!* 🙏