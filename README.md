TÊN DỰ ÁN
HotelFinder - Ứng dụng tìm kiếm và gợi ý khách sạn dùng TF-IDF (có thể mở rộng embeddings)

MÔ TẢ NGẮN
Ứng dụng Streamlit cho phép:
- Tìm kiếm khách sạn theo từ khóa và lọc cơ bản (giá, sao, tiện ích nếu có)
- Gợi ý khách sạn tương tự bằng TF-IDF similarity
- Hiển thị một số chỉ số so sánh (benchmark) và sentiment đơn giản (đếm từ tích cực / tiêu cực)

TÍNH NĂNG CHÍNH
- Tìm kiếm văn bản mô tả
- Gợi ý tương tự Top K
- Benchmark chỉ số (nếu dữ liệu có cột số)
- Sentiment thô dựa trên wordlist
- Cache model và dữ liệu
- Dễ nâng cấp sang embeddings + ANN

YÊU CẦU TỐI THIỂU
Python 3.10+
Thư viện: streamlit, pandas, numpy, gensim, joblib (pyvi tùy chọn)

CÀI ĐẶT NHANH
1. git clone <repo>
2. cd vào thư mục dự án
3. python -m venv .venv
4. Kích hoạt môi trường ảo
5. pip install -r requirements.txt
6. Chuẩn bị dữ liệu CSV trong thư mục data
7. Huấn luyện TF-IDF (nếu chưa có) để tạo model trong models/hotel_tfidf_v1
8. streamlit run Home.py

FILE requirements.txt (tối thiểu)
streamlit==1.38.0
pandas
numpy
scikit-learn
requests
datetime
json
os
re
unicodedata
typing
gensim
PyVi

DỮ LIỆU (CSV)
Thư mục: data/
Tên ví dụ: hotels_small.csv
Cột gợi ý:
- Hotel_ID (khuyến khích)
- Hotel_Name hoặc Name
- Hotel_Description hoặc Description hoặc Content hoặc Text
- star hoặc stars hoặc star_rating
(Tùy chọn cho insight) Location, Cleanliness, Facilities
(Tùy chọn cho sentiment) Review hoặc Comment


CHẠY ỨNG DỤNG
streamlit run HotelFinder.py

CẤU TRÚC THƯ MỤC
.
├── Home.py
├── pages/
│   ├── 2_About.py 
│   ├── 4_Embeddings_Test.py 
│   ├── 5_Similar_Hotels.py
├── models/
│   └── hotel_tfidf_v1/
│       ├── dictionary.dict
│       ├── tfidf.model
│       ├── similarity.index
│       ├── meta.json
│       └── hotel_id_order.pkl (nếu có)
├── data/
│   └── hotels_small.csv
├── requirements.txt
└── README.md

LUỒNG HOẠT ĐỘNG
1. Người dùng nhập từ khóa tại Home
2. Lọc tùy chọn (giá, sao...) nếu được hỗ trợ
3. Chọn hoặc bấm gợi ý → đặt similar_doc_id (session_state)
4. Trang Similar tải model + vector hóa mô tả gốc
5. Tính similarity → hiển thị Top K
6. Tùy chọn: hiển thị benchmark + sentiment thô

GIẢI THÍCH TƯƠNG TỰ (TF-IDF)
- Token hóa mô tả
- Loại từ quá hiếm/quá phổ biến
- BOW → TF-IDF
- So khớp cosine bằng SparseMatrixSimilarity

TÙY CHỈNH NHANH
- Số gợi ý tương tự: biến top_k (trong trang Similar)
- Wordlist sentiment: POS_WORDS / NEG_WORDS
- Ánh xạ tên cột mô tả: danh sách ưu tiên ["Hotel_Description","Description","Content","Text"]
- Danh sách khách sạn gợi ý ban đầu: chỉnh trong Home.py

HIỆU NĂNG CƠ BẢN
- Dùng @st.cache_resource cho model
- Dùng @st.cache_data cho CSV
- Dữ liệu lớn: chuyển sang embeddings + ANN (FAISS/hnswlib)

TROUBLESHOOTING
- similarity.index không load: sai class → đảm bảo dùng SparseMatrixSimilarity cả khi save và load
- Không có kết quả khác biệt: dictionary quá nhỏ → điều chỉnh no_below / no_above
- Insight trống: thiếu cột số → thêm Location, Cleanliness, Facilities
- Unicode lỗi: kiểm tra encoding UTF-8 (không BOM)
- Nút không hoạt động: tránh đặt st.button trong Markdown HTML
- Mất trạng thái lọc: kiểm tra khóa trong st.session_state

ROADMAP NGẮN
- Thêm sentence embeddings
- Thêm ANN (FAISS)
- Thêm lọc theo vị trí (lat/lon)
- Thêm phân tích chủ đề (LDA/BERTopic)
- API REST
- Giao diện đa ngôn ngữ

LICENSE
MIT (cập nhật nếu khác)

LIÊN HỆ
Email: (điền)
Maintainer: (điền)

GHI CHÚ
Đây là bản rút gọn, đủ để chạy và mở rộng nhanh. Có thể thêm Docker, CI, hoặc hướng dẫn deploy sau.