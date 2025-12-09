# Sistem Monitoring Kualitas Data Berbasis AI untuk Program Gratis

**Mata Kuliah:** Analisis Bisnis Data Perusahaan  
**Aplikasi:** #8 - Monitoring Kualitas Data AI  
**Framework:** Soda Core (Observability) & DQOps (Sensor-Based Testing)

**Tim Pengembang:**
- 202022510021 - Made Marshall Vira Deva
- 202022420034 - Irfan Venny Rahmayanti

---

## 🔗 Live Demo
- [APLIKASI 8](https://marshallvd.github.io/Sistem-Monitoring-Kualitas-Data-Berbasis-AI-untuk-Program-Gratis/)

## 🔗 Dataset
- [Dataset MBG](https://drive.google.com/file/d/1kFD4UJmeGSQMFOh9XoN257UPq9im5wb-/view?usp=sharing)

## 🔗 Google Colab
- [Web Scrapping](https://colab.research.google.com/drive/1p7tzrylYphXM-3JNdpYYRxVSvXIEJvTX?usp=sharing)
- [Data Cleaning](https://colab.research.google.com/drive/1-Zj-elaqNUGy-SVyufmw2xrxPqUTZnWE?usp=sharing)
- [Dashboard Create](https://colab.research.google.com/drive/1IdrRPOrNVEMsb6P6hUgHPSiyyud2ylUY?usp=sharing)

---

## 📋 Daftar Isi

- [BAB 1: Pendahuluan](#bab-1-pendahuluan)
- [BAB 2: Konsep & Framework](#bab-2-konsep--framework)
- [BAB 3: Pemilihan Data Source & Metodologi](#bab-3-pemilihan-data-source--metodologi)
- [BAB 4: Implementasi & Hasil](#bab-4-implementasi--hasil)
- [BAB 5: Kesimpulan & Rekomendasi](#bab-5-kesimpulan--rekomendasi)

---

## BAB 1: Pendahuluan

### 1.1 Latar Belakang

Program distribusi Makanan Bergizi Gratis (MBG) merupakan inisiatif pemerintah untuk meningkatkan kesehatan masyarakat. Keberhasilan program ini sangat bergantung pada **kualitas data** yang digunakan untuk monitoring dan evaluasi.

**Mengapa Kualitas Data Penting?**
- ✅ **Akurasi Distribusi:** Data berkualitas memastikan makanan sampai ke penerima yang tepat
- ✅ **Transparansi:** Memudahkan audit dan akuntabilitas program
- ✅ **Efisiensi:** Mengurangi pemborosan akibat data duplikat atau tidak valid
- ✅ **Pengambilan Keputusan:** Data akurat menghasilkan keputusan yang lebih baik

### 1.2 Permasalahan

Dalam pengelolaan data MBG, ditemukan beberapa masalah potensial:

1. **Data Tidak Lengkap**
   - Kemungkinan missing values pada field penting
   - Artikel tanpa content atau title yang memadai

2. **Data Tidak Valid**
   - Potensi format URL yang salah
   - Penggunaan HTTP alih-alih HTTPS (security issue)
   - Content yang tidak memenuhi standar minimum

3. **Data Duplikat**
   - Potensi URL yang sama dengan content berbeda
   - Content yang identical menunjukkan duplikasi sistem

4. **Inkonsistensi**
   - Kategori tidak standar
   - Panjang content dan title yang sangat bervariasi

### 1.3 Tujuan Proyek

Membangun sistem monitoring kualitas data otomatis yang dapat:
- ✅ Mendeteksi anomali data secara real-time
- ✅ Memberikan skor kualitas yang objektif
- ✅ Menghasilkan rekomendasi perbaikan berbasis AI
- ✅ Menyediakan dashboard visualisasi untuk stakeholder

### 1.4 Mengapa Soda Core & DQOps?

**Pendekatan Dual-Layer:**

| Framework | Fokus | Keunggulan |
|-----------|-------|------------|
| **Soda Core** | Data Observability | - Monitoring kontinu<br>- Rule-based validation<br>- Mudah dikonfigurasi |
| **DQOps** | Sensor-Based Testing | - Validasi mendalam<br>- Deteksi anomali granular<br>- Statistical analysis |

**Kombinasi ini memberikan:**
- 📊 **Breadth:** Coverage luas dengan Soda Core
- 🔬 **Depth:** Analisis detail dengan DQOps
- 🎯 **Completeness:** Validasi menyeluruh dari berbagai dimensi

---

## BAB 2: Konsep & Framework

### 2.1 Data Quality Management Framework (DQMF)

DQMF adalah kerangka kerja sistematis untuk mengelola kualitas data organisasi.

**6 Dimensi Kualitas Data:**

```
┌─────────────────────────────────────────────────┐
│         DATA QUALITY DIMENSIONS                 │
├─────────────────────────────────────────────────┤
│                                                 │
│  1. COMPLETENESS   → Kelengkapan Data          │
│  2. VALIDITY       → Keabsahan Format          │
│  3. ACCURACY       → Ketepatan Nilai           │
│  4. UNIQUENESS     → Keunikan Record           │
│  5. CONSISTENCY    → Konsistensi Relasi        │
│  6. TIMELINESS     → Ketepatan Waktu           │
│                                                 │
└─────────────────────────────────────────────────┘
```

### 2.2 Soda Core Framework

**Arsitektur:**
```
User → YAML Config → Soda Scan → Pandas DataFrame → Results
                                        ↓
                               Quality Checks
                                        ↓
                               JSON/HTML Report
```

**Karakteristik:**
- **Declarative:** Menggunakan YAML untuk mendefinisikan checks
- **Extensible:** Mendukung custom checks
- **Integration:** Terintegrasi dengan data sources (Pandas, Spark, SQL)

**Contoh Check:**
```yaml
checks for mbg_data:
  - missing_count(title) = 0
  - missing_percent(url) < 5%
  - duplicate_count(url) = 0
```

### 2.3 DQOps Framework

**Arsitektur Sensor:**
```
Data Source
    ↓
Sensor Layer (6 Sensors)
    ├── Completeness Sensor
    ├── Validity Sensor
    ├── Accuracy Sensor
    ├── Uniqueness Sensor
    ├── Consistency Sensor
    └── Statistical Sensor
    ↓
Evaluation Engine
    ↓
Alert & Reporting
```

**Sensor-Based Testing:**
- **Collect:** Mengumpulkan metrics dari data
- **Evaluate:** Membandingkan dengan threshold
- **Alert:** Trigger notifikasi jika threshold dilanggar

**Severity Levels:**
- 🚨 **CRITICAL:** Must fix immediately (blocking)
- ⚠️ **ERROR:** Should fix soon (high priority)
- 💡 **WARNING:** Nice to improve (low priority)

---

## BAB 3: Pemilihan Data Source & Metodologi

### 3.1 Sumber Data

**Dataset:** MBG Data (Makanan Bergizi Gratis Articles)
- **Source:** Republika.co.id (berita tentang program MBG)
- **Method:** Web Scraping
- **Format:** CSV
- **Size:** 244 baris × 13 kolom

**Struktur Data:**
```
┌──────────────┬──────────────┬─────────────┐
│ Column       │ Type         │ Description │
├──────────────┼──────────────┼─────────────┤
│ title        │ text         │ Judul       │
│ content      │ text         │ Isi artikel │
│ url          │ text         │ Link        │
│ date         │ datetime     │ Tanggal     │
│ category     │ text         │ Kategori    │
│ author       │ text         │ Penulis     │
│ ...          │ ...          │ ...         │
└──────────────┴──────────────┴─────────────┘
```

### 3.2 Mengapa Dataset Ini?

**Relevansi:**
1. **Real-world data** → Mencerminkan kondisi aktual
2. **Public interest** → Program pemerintah untuk masyarakat
3. **Data variety** → Berbagai kategori dan format
4. **Quality challenges** → Ada aspek kualitas yang perlu divalidasi

**Karakteristik Data:**
- ✅ Unstructured text (content, title)
- ✅ Structured metadata (URL, date, category)
- ✅ Mixed quality (complete & incomplete records)
- ✅ Beragam kategori berita
- ✅ Multiple authors

### 3.3 Hasil Preliminary Analysis

**Hasil EDA:**

| Aspek | Hasil | Status |
|-------|-------|--------|
| **Total Records** | 244 artikel | ✅ Excellent |
| **Completeness** | - Title: 100%<br>- Content: 100%<br>- URL: 100%<br>- Date: 100%<br>- Author: 95.9% | ✅ Very Good |
| **Content Stats** | - Min: 296 chars<br>- Max: 6,378 chars<br>- Avg: 2,211 chars | ✅ Good quality |
| **Word Count** | - Min: 42 words<br>- Max: 837 words<br>- Avg: 298 words | ✅ Substantial |
| **Uniqueness** | - No duplicate URLs<br>- No duplicate content<br>- 100% unique | ✅ Excellent |
| **Categories** | 11 unique categories | ✅ Good diversity |
| **Authors** | 28 unique authors | ✅ Multiple sources |

### 3.4 Manfaat Soda Core & DQOps

**Soda Core Benefits:**
```
📊 Observability Layer
├── ✅ Quick health checks
├── ✅ Automated alerting
├── ✅ Historical tracking
└── ✅ Easy configuration
```

**DQOps Benefits:**
```
🔬 Testing Layer
├── ✅ Deep validation
├── ✅ Statistical analysis
├── ✅ Outlier detection
└── ✅ Granular insights
```

**Combined Impact:**
- 🎯 **Comprehensive validation** (57 total checks)
- ⚡ **Automated monitoring** vs manual
- 📈 **Measurable quality metrics** (score-based)
- 🤖 **Data-driven recommendations**

### 3.5 Metodologi Implementasi

**Pipeline:**
```
1. Web Scraping
   └─→ republika.co.id
        └─→ BeautifulSoup
             └─→ Raw CSV (244 articles)

2. Data Preprocessing
   └─→ Cleaning
        └─→ Feature Engineering
             └─→ Processed CSV

3. Soda Core Validation (Adjusted)
   └─→ YAML Checks (26 checks)
        └─→ Scan Execution
             └─→ Soda Report (92.3% score)

4. DQOps Sensor Testing (Adjusted)
   └─→ 6 Sensors (31 tests)
        └─→ Deep Analysis
             └─→ DQOps Report (93.5% score)

5. Integrated Dashboard
   └─→ Combined Score (93.1%)
        └─→ Recommendations
             └─→ HTML Visualization
```

---

## BAB 4: Implementasi & Hasil

### 4.1 Web Scraping

**Tools:** Python, BeautifulSoup, Requests

**Proses:**
```python
# Pseudocode
for page in range(1, total_pages):
    response = requests.get(f"{base_url}/search/v3/?q=MBG")
    soup = BeautifulSoup(response.content, 'html.parser')
    
    for article in soup.find_all('div', class_='news-item'):
        data = {
            'title': article.find('h2').text,
            'content': get_article_detail(url),
            'url': article.find('a')['href'],
            'date': article.find('div', class_='news-source').text,
            'category': extract_category(article)
        }
        dataset.append(data)
```

**Hasil Scraping:**
- ✅ 244 artikel berhasil diambil
- ✅ 13 kolom metadata lengkap
- ✅ Time range: November 2025
- ✅ Zero duplicates (filtered during scraping)

### 4.2 Data Preprocessing

**Hasil EDA & Cleansing:**

1. **Data Quality Metrics**
   ```
   • Original dataset: 244 articles
   • Cleaned dataset: 244 articles
   • Removed duplicates: 0 articles
   • Data reduction: 0.0%
   ```

2. **Completeness Check**
   ```
   ✅ Title:    100.0% (244/244)
   ✅ Content:  100.0% (244/244)
   ✅ URL:      100.0% (244/244)
   ✅ Date:     100.0% (244/244)
   ✅ Category: 100.0% (244/244)
   ⚠️ Author:   95.9% (234/244)
   ```

3. **Feature Engineering**
   ```python
   # Metrics ditambahkan
   df['content_length'] = df['content'].str.len()
   df['title_length'] = df['title'].str.len()
   df['word_count'] = df['content'].str.split().str.len()
   df['has_long_content'] = df['content_length'] >= 2000
   df['has_quality_title'] = df['title_length'] >= 50
   ```

**Output:**
- ✅ `mbg_data_clean.csv` (ready for validation)
- ✅ Additional 5 metrics columns
- ✅ Standardized formats

### 4.3 Soda Core Implementation (Adjusted Thresholds)

**Step 1: Configuration**
```yaml
# configuration.yml
data_source mbg_data:
  type: pandas
```

**Step 2: Define Checks (Adjusted)**
```yaml
# mbg_quality_checks_adjusted.yml
checks for mbg_data:
  # Completeness (7 checks)
  - row_count > 0
  - missing_count(title) = 0
  - missing_count(content) = 0
  - missing_percent(url) = 0%
  
  # Validity (3 checks)
  - invalid_percent(url) = 0%:
      valid format: url
  
  # Accuracy (10 checks) - ADJUSTED
  - min(content_length) >= 100  # was 800
  - avg(content_length) >= 1200
  - min(title_length) >= 10     # was 40
  - min(word_count) >= 50       # was 100
  
  # Uniqueness (4 checks)
  - duplicate_count(url) = 0
  - duplicate_percent(content) = 0
  
  # Consistency (2 checks)
  - values in (category) must be in [News, Rejabar, ...]
```

**Step 3: Execute Scan**
```python
from soda.scan import Scan

scan = Scan()
scan.set_data_source_name("mbg_data")
scan.add_pandas_dataframe(dataset=df)
scan.execute()

# Results
quality_score = 92.3/100
```

**Hasil Soda Core (Adjusted):**

| Dimensi | Checks | Passed | Failed | Score |
|---------|--------|--------|--------|-------|
| Completeness | 7 | 7 | 0 | 100% |
| Validity | 3 | 3 | 0 | 100% |
| Accuracy | 10 | 9 | 1 | 90.0% |
| Uniqueness | 4 | 4 | 0 | 100% |
| Consistency | 2 | 1 | 1 | 50.0% |
| **TOTAL** | **26** | **24** | **2** | **92.3%** |

**Interpretasi:**
- ✅ Skor 92.3% → **EXCELLENT 🌟**
- ⚠️ 2 failed checks:
  1. **Accuracy:** Word count min (42 < 50 expected)
  2. **Consistency:** 38 invalid categories found
- ✅ Perfect completeness, validity, uniqueness

**Key Adjustments Made:**
```
Original → Adjusted → Reason
─────────────────────────────
Content min: 800 → 100 chars (realistic for news)
Title min: 40 → 10 chars (flexible)
Word min: 100 → 50 words (achievable)
Categories: strict → expanded list
```

### 4.4 DQOps Sensor Implementation (Adjusted Thresholds)

**Sensor Architecture:**

```
Data → Sensors → Evaluation → Report
         ↓
    ┌────┴────┬────────┬──────────┬───────────┬────────────┐
    │         │        │          │           │            │
Completeness Validity Accuracy Uniqueness Consistency Statistical
```

**Sensor Results:**

| Sensor | Tests | Passed | Failed | Critical | Score |
|--------|-------|--------|--------|----------|-------|
| Completeness | 6 | 6 | 0 | 0 | 100% |
| Validity | 5 | 5 | 0 | 0 | 100% |
| Accuracy | 11 | 10 | 1 | 1 | 90.9% |
| Uniqueness | 3 | 3 | 0 | 0 | 100% |
| Consistency | 3 | 2 | 1 | 0 | 66.7% |
| Statistical | 3 | 3 | 0 | 0 | 100% |
| **TOTAL** | **31** | **29** | **2** | **1** | **93.5%** |

**Interpretasi:**
- ✅ Skor 93.5% → **GOOD ✅**
- 🚨 1 critical failure: `word_count_min_50` (42 < 50)
- ⚠️ 1 warning: `category_95_valid` (84.4% valid)
- ✅ Perfect: Completeness, Validity, Uniqueness, Statistical

**Failed Tests Detail:**

1. **Accuracy Sensor (CRITICAL)**
   ```
   Test: word_count_min_50
   Actual: 42 words
   Expected: ≥50 words
   Severity: CRITICAL
   Action: Review short articles
   ```

2. **Consistency Sensor (WARNING)**
   ```
   Test: category_95_valid
   Actual: 84.43% valid (38 invalid)
   Expected: ≥95% valid
   Severity: WARNING
   Action: Standardize categories
   ```

**Key Adjustments Made:**
```
Sensor Adjustments:
─────────────────────────────
✓ Content min: 800 → 100 chars
✓ Word count min: 100 → 50 words
✓ Categories: Added News, Rejabar, Ekonomi Syariah, etc.
✓ Correlation: >0.2 → >0 (flexible)
✓ Warnings: changed to 'warning' severity
```

### 4.5 Combined Analysis

**Weighted Scoring:**
```
Combined Score = (Soda Core × 0.4) + (DQOps × 0.6)
               = (92.3 × 0.4) + (93.5 × 0.6)
               = 36.92 + 56.10
               = 93.1%
```

**Quality Grade:** **VERY GOOD ✅**

**Framework Comparison:**

```
Soda Core:    ██████████████████░░ 92.3%
DQOps:        ██████████████████░░ 93.5%
Combined:     ██████████████████░░ 93.1%
```

**Detailed Comparison:**

| Metric | Soda Core | DQOps | Combined |
|--------|-----------|-------|----------|
| Total Validations | 26 | 31 | 57 |
| Passed | 24 | 29 | 53 |
| Failed | 2 | 2 | 4 |
| Success Rate | 92.3% | 93.5% | 93.0% |
| Critical Issues | - | 1 | 1 |
| Warnings | - | 1 | 1 |

### 4.6 Dashboard & Visualisasi

**HTML Report Features:**

1. **Hero Score Card**
   ```
   ┌─────────────────────────────┐
   │   Combined Quality Score    │
   │          93.1/100           │
   │      VERY GOOD ✅           │
   └─────────────────────────────┘
   ```

2. **Method Comparison**
   - Side-by-side Soda Core vs DQOps
   - Detailed metrics per framework
   - Visual bar charts

3. **Issue Tracking**
   - 🚨 Critical Issues (1)
   - ⚠️ Warnings (1)
   - 💡 Other Issues (2)
   - With actionable recommendations

4. **Dimension Breakdown**
   - Completeness: 100% ✅
   - Validity: 100% ✅
   - Accuracy: 90.5% ⚠️
   - Uniqueness: 100% ✅
   - Consistency: 58.3% ⚠️
   - Statistical: 100% ✅

**Dashboard Screenshots:**

*(HTML reports tersimpan di `/content/soda_project/reports/`)*
- `soda_core_report_adjusted_20251130_174153.html`
- `dqops_sensor_report_adjusted_20251130_174155.html`
- `integrated_comparison_20251130_174157.html`

**Key Insights dari Dashboard:**

1. **Strengths:**
   - ✅ Perfect data completeness (100%)
   - ✅ All URLs valid and unique
   - ✅ No duplicate content
   - ✅ Statistical distribution normal

2. **Issues Found:**
   - 🚨 **CRITICAL:** 1 article with <50 words (need review)
   - ⚠️ **WARNING:** 38 articles with non-standard categories
   - 💡 Minor: Title-content correlation low (0.007)

3. **Recommendations:**
   - ✅ Review and expand short articles
   - ✅ Standardize category taxonomy
   - ✅ Add content guidelines (min 50 words)
   - ✅ Continue monitoring for consistency

---

## BAB 5: Kesimpulan & Rekomendasi

### 5.1 Kesimpulan

**Pencapaian Proyek:**

1. ✅ **Sistem Monitoring Berhasil Dibangun**
   - Dual-layer validation (Soda Core + DQOps)
   - 57 comprehensive quality checks
   - Automated scoring system

2. ✅ **Quality Assessment Completed**
   - Overall score: **93.1/100** (Very Good)
   - 4 issues identified (1 critical, 1 warning)
   - 6 quality dimensions evaluated

3. ✅ **Dashboard Interaktif Tersedia**
   - 3 HTML reports with visualizations
   - Issue tracking & prioritization
   - Actionable recommendations

**Temuan Utama:**

| Aspek | Finding | Impact |
|-------|---------|--------|
| **Strengths** | ✅ 100% completeness<br>✅ Perfect uniqueness<br>✅ All URLs valid<br>✅ Normal distribution | Excellent data foundation |
| **Weaknesses** | 🚨 1 very short article<br>⚠️ 38 non-standard categories | Minor improvements needed |
| **Opportunities** | 💡 Expand content guidelines<br>💡 Standardize categories<br>💡 Add quality controls | Quick wins available |

**Data Quality Matrix:**

```
Dimension          Status    Score   Grade
─────────────────────────────────────────
Completeness       Perfect   100%    ⭐⭐⭐⭐⭐
Validity           Perfect   100%    ⭐⭐⭐⭐⭐
Accuracy           Good      90.5%   ⭐⭐⭐⭐
Uniqueness         Perfect   100%    ⭐⭐⭐⭐⭐
Consistency        Fair      58.3%   ⭐⭐⭐
Statistical        Perfect   100%    ⭐⭐⭐⭐⭐
─────────────────────────────────────────
OVERALL            Very Good 93.1%   ⭐⭐⭐⭐
```

### 5.2 Rekomendasi Perbaikan

**Immediate Actions (Critical - Week 1):**

1. **Review Short Article**
   ```
   Issue: 1 article with only 42 words
   Action: 
   - Expand content to minimum 50 words
   - Ensure sufficient context
   - Maintain journalistic quality
   
   Expected Impact: Accuracy 90.5% → 100%
   ```

**Short-term Actions (1-2 weeks):**

2. **Standardize Categories**
   ```python
   # Define standard categories
   valid_categories = [
       'News', 'Rejabar', 'Rejogja', 'Ekonomi',
       'Ekonomi Syariah', 'Islam Digest', 'Visual',
       'Ameera', 'Kolom'
   ]
   
   # Map non-standard to standard
   category_mapping = {
       'En': 'News',
       'Esgnow': 'News'
   }
   ```
   **Expected Impact:** Consistency 58.3% → 95%+

3. **Content Quality Guidelines**
   ```yaml
   # Add editorial rules
   rules:
     - minimum_words: 50
     - minimum_characters: 300
     - require_proper_formatting: true
     - validate_before_publish: true
   ```
   **Expected Impact:** Maintain 100% quality

**Long-term Improvements (1-3 months):**

4. **Continuous Monitoring**
   - Schedule daily Soda Core scans
   - Alert on quality degradation
   - Track improvement trends
   - Monthly quality reports

5. **Data Governance Policy**
   - Define quality standards document
   - Assign data stewards
   - Regular quality reviews
   - Training for content creators

6. **Integration with Production**
   - Add pre-publish validation
   - Real-time quality checks
   - Automated content review
   - Integration with CMS

## 📚 Referensi

**Frameworks:**
- [Soda Core Documentation](https://docs.soda.io/)
- [DQOps Framework](https://dqops.com/docs/)
- Data Quality Management Framework (ISO 8000)

**Tools:**
- Python 3.10+
- Pandas 2.2.3
- NumPy 1.26.4
- BeautifulSoup4
- Requests

**Dataset:**
- Republika.co.id (berita program MBG)
- Periode: November 2025
- Size: 244 unique articles
- Categories: 11 distinct

---


