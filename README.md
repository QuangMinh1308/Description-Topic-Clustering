# 🎬 Description-Topic-Clustering
*(Phân cụm chủ đề video YouTube dựa trên phần mô tả video)*

![PhoBERT](https://img.shields.io/badge/Model-PhoBERT-blue?style=for-the-badge)
![SimCSE](https://img.shields.io/badge/Model-SupSimCSE-green?style=for-the-badge)
![UMAP](https://img.shields.io/badge/Model-UMAP-orange?style=for-the-badge)
![HDBSCAN](https://img.shields.io/badge/Model-HDBSCAN-red?style=for-the-badge)
![KMeans](https://img.shields.io/badge/Model-KMeans-yellow?style=for-the-badge)
![DBSCAN](https://img.shields.io/badge/Model-DBSCAN-purple?style=for-the-badge)
![BERTopic](https://img.shields.io/badge/Model-BERTopic-9cf?style=for-the-badge)

---

## 📌 Overview  
*(Tổng quan)*  

This project analyzes **Vietnamese YouTube videos** by clustering their **descriptions** using modern NLP models and density-based clustering algorithms.  
*(Dự án phân tích video YouTube tiếng Việt bằng cách phân cụm mô tả video thông qua các mô hình NLP hiện đại và thuật toán phân cụm theo mật độ.)*

We embed descriptions using **Sup-SimCSE-PhoBERT**, reduce dimensionality using **UMAP**, and cluster using **HDBSCAN**.  
KMeans and DBSCAN are used for comparison, and **BERTopic** is applied for automatic topic labeling.  
*(Sử dụng embedding Sup-SimCSE-PhoBERT, giảm chiều bằng UMAP, phân cụm bằng HDBSCAN. KMeans và DBSCAN dùng để so sánh, cuối cùng dùng BERTopic để gán nhãn chủ đề.)*

---

## 🎯 Objectives  
*(Mục tiêu)*  

- Automatically collect YouTube video metadata.  
- Preprocess Vietnamese descriptions.  
- Create semantic embeddings (Sup-SimCSE-PhoBERT).  
- Apply clustering algorithms (KMeans, DBSCAN, HDBSCAN).  
- Use BERTopic for topic extraction & visualization.  
- Evaluate cluster quality using Silhouette Score & Noise Ratio.  

---

## 📂 Dataset  
*(Tập dữ liệu)*  

- 7,819 Vietnamese videos collected using **YouTube Data API v3**.  
- Fields include: `title`, `description`, `channel`, `views`, `likes`, `comments`, `tags`, `category_id`.  
*(Dữ liệu gồm mô tả, tiêu đề, lượt xem, like, bình luận, thẻ, danh mục...)*  

After cleaning:  
- 5,107 valid descriptions (34.6% noise removed via HDBSCAN).  

---

## 🧹 Preprocessing  
*(Tiền xử lý)*  

- Remove HTML, URLs, emojis, special characters  
- Normalize Vietnamese text  
- Lowercasing, trimming  
- Convert hashtags → meaningful tokens  
- Remove stopwords  
- Tokenize using **underthesea**  
- Save clean data → `stage1_desc_clean.csv`

---

## 🧠 Semantic Embedding  
*(Biểu diễn ngữ nghĩa)*  

Using **Sup-SimCSE-PhoBERT**, descriptions are encoded into **768-dimensional vectors**.  
*(Dùng Sup-SimCSE-PhoBERT để mã hóa mô tả thành vector 768 chiều.)*

Pipeline:  
1. Transformer Encoder  
2. Mean Pooling  
3. Normalization  

Output saved as: `embeddings_desc_phobert.npy`

---

## 🧩 Clustering Methods  
*(Các thuật toán phân cụm)*  

### 1️⃣ KMeans  
- Fast and stable  
- Requires predefining **k**  
- Poor for short noisy text  

### 2️⃣ DBSCAN  
- Density-based  
- Auto-detects noise  
- Very sensitive to `eps`  

### 3️⃣ HDBSCAN (main method)  
- No need `k`  
- Handles variable density  
- Extremely effective for YouTube descriptions  

---

## 🧠 BERTopic Pipeline  
*(Quy trình BERTopic)*  

Combines:  
- **PhoBERT/Sup-SimCSE** (embeddings)  
- **UMAP** (dimensionality reduction)  
- **HDBSCAN** (clustering)  
- **Topic labeling** using keyword extraction  

Outputs:  
- `topic_id`  
- `topic_name`  
- Keyword importance list  

---

## 📊 Algorithm Comparison  
*(Bảng so sánh các thuật toán)*  

### 📈 Evaluation Metrics  
- **Silhouette Score** (độ tách biệt cụm)  
- **Noise Ratio** (tỷ lệ nhiễu)  
- **Cluster Count** (số lượng cụm)  

### 🧮 Results from experiment  
*(Kết quả thực nghiệm từ báo cáo)*  

| Algorithm | Clusters | Noise Ratio | Silhouette Score | Strengths | Weaknesses |
|-----------|----------|-------------|------------------|-----------|------------|
| **KMeans** | 100 | – | **0.1378** | Clear boundaries | Must predefine K |
| **DBSCAN** | 3 | 8.24% | – | Removes noise well | Too few clusters |
| **HDBSCAN** (BERTopic) | 71 | 34.68% | 0.1118 | Adaptive, meaningful clusters | Higher noise |

*(HDBSCAN phù hợp nhất cho dữ liệu thật, KMeans tách cụm rõ nhưng thiếu linh hoạt.)*

---

## 📉 Visualization  
*(Trực quan hóa)*  

- **Topic Word Scores**  
- **Intertopic Distance Map** (UMAP 2D)  
- **HDBSCAN Condensed Tree**  
- **Hierarchical Topic Tree (BERTopic)**  

*(Các biểu đồ giúp quan sát từ khóa, khoảng cách chủ đề, cấu trúc phân cấp.)*

---

## 🧪 Experiments  
*(Thực nghiệm)*  

Using:  
- **UMAP:** n_neighbors=25, n_components=15  
- **HDBSCAN:** min_cluster_size=18  
- **Embedding:** Sup-SimCSE-PhoBERT  

Discovered:  
- **46 meaningful topics**  
- Major themes: entertainment, learning, tech, food, sports  

---

## 💬 Discussion  
*(Thảo luận)*  

- Sup-SimCSE-PhoBERT produces strong Vietnamese embeddings  
- HDBSCAN automatically finds natural clusters  
- KMeans scores highest in Silhouette but requires fixed K  
- DBSCAN unstable on textual data  
- BERTopic provides the best interpretability  

---

## 🚀 Applications  
*(Ứng dụng thực tế)*  

- Content recommendation systems  
- Media trend analysis  
- Social listening & sentiment tracking  
- Topic-based audience segmentation  

---

## 🧭 Future Work  
*(Hướng phát triển)*  

- Dynamic topic modeling  
- Adding comments + titles for richer signals  
- Sentiment analysis per topic  
- Using multimodal models (video + audio + text)  

---

## 👨‍💻 Authors  
Hà Thế Anh, Nguyễn Nhật Nam, **Hoàng Quang Minh**, Lê Nhật Tùng  
HUTECH University, Vietnam  

📄 Full report: :contentReference[oaicite:2]{index=2}
