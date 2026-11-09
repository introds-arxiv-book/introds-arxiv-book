# MASTER PROJECT BRIEF & HANDOVER — INTRODUCTION TO DATA SCIENCE (HCMUS)
> **Tài liệu bàn giao ngữ cảnh đồ án dành cho AI Coding Agents & Thành viên nhóm**  
> **Cập nhật mới nhất:** 09/11/2026 (Sau khi hoàn tất Benchmark Colab CPU-Only thật)  
> **Nhóm thực hiện:** Nhóm 04 — Môn Nhập môn Khoa học Dữ liệu (HCMUS)  
> **Người dùng đang làm việc:** Nguyễn Lê Anh Tuấn (MSSV: 24120153 — Phụ trách Hạ tầng Benchmark & Jupyter Book)

---

## 1. MỤC ĐÍCH VÀ NGUYÊN TẮC HOẠT ĐỘNG DÀNH CHO AI AGENT
Tài liệu này được thiết kế để **bảo lưu toàn vẹn ngữ cảnh đồ án** khi chuyển đổi qua bất kỳ phiên trò chuyện hoặc agent mới nào. Khi bắt đầu phiên làm việc mới:
1. **ĐỌC KỸ TÀI LIỆU NÀY ĐẦU TIÊN** trước khi chạm vào bất kỳ file code hoặc notebook nào.
2. **NẮM RÕ VAI TRÒ:** Bạn đang đóng vai trò trợ lý đắc lực cho **Nguyễn Lê Anh Tuấn (MSSV: 24120153)**, người chịu trách nhiệm về:
   - Công cụ đo hiệu năng chuẩn hóa (`src/benchmark.py`).
   - Toàn bộ hạ tầng xuất bản báo cáo Jupyter Book (`introds-arxiv-book`).
   - Tổng hợp, phân tích định lượng và đối soát dữ liệu của cả 5 thành viên.
3. **TUYỆT ĐỐI TUÂN THỦ RANH GIỚI REPO:**
   - **Repository Code (`introds-arxiv-project`):** Không tự ý sửa code crawler/parser của thành viên khác nếu không được giao. Working tree của repo này phải luôn sạch sẽ, không commit dữ liệu rác.
   - **Repository Book (`introds-arxiv-book`):** Mọi công việc biên tập nội dung, đồ thị, bảng biểu, cập nhật số liệu log thực nghiệm đều diễn ra tại đây.

---

## 2. NGUỒN YÊU CẦU CHÍNH THỨC & TIÊU CHÍ CHẤM ĐIỂM
1. **Lab Book chính thức môn học:** `https://hcmus-cs-ta.github.io/lab-book/intro-ds/statement/`
   - **Milestone 1 — Crawling:** `https://hcmus-cs-ta.github.io/lab-book/intro-ds/statement/crawling/`
   - **Milestone 2 — Parsing:** `https://hcmus-cs-ta.github.io/lab-book/intro-ds/statement/parsing/`
   - **Milestone 3 — Matching:** (Tiêu thụ dữ liệu đầu ra chuẩn của M2).

2. **Các ràng buộc bất biến (Zero-Tolerance Rules):**
   - **Academic Integrity:** *"Any milestone will be graded zero if there is any dishonest behaviour detected."* Không được bịa số liệu, không dùng mock data giả lập kết quả thực nghiệm thật, không hardcode số liệu sai lệch với file log CSV.
   - **Môi trường Benchmark chuẩn:** *"The testbed for the final benchmarking is a Google Colab instance running on CPU-only mode."* Bắt buộc có kết quả đo đạc từ Google Colab CPU-only (đo bằng `getrusage` trên nhân Linux).
   - **Quy định Repo Code sạch:** Repo code chỉ chứa source code (`.py`, `.ipynb`, `requirements.txt`, `README.md`), tuyệt đối cấm commit thư mục chứa dữ liệu crawl (`corpus/`, `raw/`, `logs/`, file nén `.tar.gz`, `.pdf`).
   - **Giới hạn 12h của Colab & Checkpointing:** Theo Q&A chính thức của Giảng viên: Cho phép chia tiến trình thành nhiều phiên độc lập có checkpointing tự động và lưu trữ nhật ký thời gian từng phiên để đối chiếu và tổng hợp.

---

## 3. MA TRẬN PHÂN CÔNG TRÁCH NHIỆM 5 THÀNH VIÊN (NHÓM 04)
Toàn bộ đồ án xử lý kho dữ liệu **25.000 bài báo khoa học** trên arXiv (mỗi thành viên 5.000 bài):

| STT | Thành viên | MSSV | Dải ID phân công (5.000 bài) | Vai trò Milestone 1 (Crawling) | Vai trò Milestone 2 (Parsing) |
| :---: | :--- | :---: | :---: | :--- | :--- |
| **1** | **Đặng Ngọc Hoàng** *(Nhóm trưởng)* | 24120053 | `2507.06659 – 2507.11658` | Entry Discovery (`src/discovery.py`), đối soát chéo A3 (`a3_*.csv`). | Thiết lập hợp đồng dữ liệu `schema_m2.md`, khung module `src/parser/`, đưa `book/` vào code repo, 3 hàm pure (`infer_levels`, `strip_references`, `split_sentences`), tính năng resumable. |
| **2** | **Dương Ngọc Minh Thư** | 24120144 | `2505.00666 – 2505.05665` | Tải source TeX (`src/downloader.py`), A1 Streaming Optimization (giảm 94% đĩa tạm). | Multi-file Gathering (`src/parser/gather.py`), đệ quy include, cycle detection, chuẩn bị môi trường Colab & nén mẫu 50 bài, tính năng parse song song (`--workers N`). |
| **3** | **Trần Ngọc Thanh Tú** | 24120152 | `2504.12523 – 2504.17522` | Bóc tách LaTeX sạch (`src/extractor.py`, `src/corpus_builder.py`). | Chuẩn hóa LaTeX & Math (`src/parser/normalize.py`), bộ validator 6 phép kiểm (`src/validator_m2.py`), bộ chuẩn vàng (Gold Standard 20 bài gán tay `gold_structure.csv`) để đo accuracy. |
| **4** | **Cáp Hửu Duy** | 24120177 | `2503.11914 – 2503.16913` *(hoặc 2504)* | Crawl References Semantic Scholar (`src/reference_builder.py`), Tiền quét TeX (`src/tex_prescan.py`). | Trích xuất BibTeX (`src/parser/bib.py`), gom nhóm bắc cầu (`signature`), hợp nhất trường (`merge_group`), đo accuracy trên 124 version ground-truth. |
| **5** | **Nguyễn Lê Anh Tuấn** *(Bạn / Tôi)* | **24120153** | **`24120153.txt`** | Công cụ đo hiệu năng chuẩn hóa (`src/benchmark.py`), khởi tạo và duy trì Jupyter Book (`introds-arxiv-book`), GitHub Actions, GitHub Pages, notebook M1. | Đóng tồn đọng M1, dựng `04-deep-analysis.ipynb`, Plotly interactivity, dựng khung 3 notebook M2 (`milestone2/01, 02, 03`), biểu đồ Sunburst & Mutation, cắm số tổng hợp ở Tuần 2. |

---

## 4. HIỆN TRẠNG TIẾN ĐỘ ĐỒ ÁN (TÍNH ĐẾN 09/11/2026)

### ✅ Milestone 1 (Crawling) — ĐÃ HOÀN TẤT VÀ KIỂM CHỨNG 100%
1. **Dữ liệu 25.000 bài:** Cả 5 thành viên đã hoàn tất cào dữ liệu, tổng hợp qua [aggregate_5_members.py](file:///d:/introds-arxiv-book/aggregate_5_members.py). Bảng 11 chỉ số bắt buộc trong [02-statistics.ipynb](file:///d:/introds-arxiv-book/milestone1/02-statistics.ipynb) đã thể hiện đầy đủ quy mô toàn nhóm.
2. **Benchmark Google Colab CPU-Only thật (Hoàn tất ngày 09/11):**
   * Tuấn đã chạy thực tế trên Google Colab CPU-Only instance (`Linux-6.6.159+-x86_64, Python 3.13.18, RAM 12.7GB`).
   * Số liệu đo thật qua kernel `getrusage` được cập nhật tại [data/benchmark_runs.csv](file:///d:/introds-arxiv-book/data/benchmark_runs.csv):
     * `discovery`: Wall-time **0.95 s** · Peak RAM **39.66 MB** · Disk **+0.009 MB**
     * `download`: Wall-time **503.23 s** (~8.4 phút) · Peak RAM **58.89 MB** · Disk **+33.60 MB**
     * `corpus_builder`: Wall-time **0.70 s** · Peak RAM **18.96 MB** · Disk **+2.15 MB**
     * `reference_builder`: Wall-time **3.11 s** · Peak RAM **37.00 MB** · Disk **+0.37 MB**
   * Đã re-execute cả 2 notebook [02-statistics.ipynb](file:///d:/introds-arxiv-book/milestone1/02-statistics.ipynb) và [03-performance.ipynb](file:///d:/introds-arxiv-book/milestone1/03-performance.ipynb) với mã thoát `exit code 0`, các bảng và chỉ số tự động tính toán động.

---

### ⏳ Milestone 2 (Parsing) — LỘ TRÌNH CHI TIẾT TỪNG NGÀY CHO TUẤN

> **Tài liệu tham chiếu:** [docs/milestone2_task_week1.md](file:///d:/introds-arxiv-project/docs/milestone2_task_week1.md)

#### TUẦN 1 (07/11 → 14/11): Dọn sạch tồn đọng M1 & Dựng khung M2
- [x] **09/11 (Thứ 2) — ĐÃ XONG ✅:** Chạy Colab CPU-only thật, cập nhật `benchmark_runs.csv`, re-run notebook.
- [x] **10/11 (Thứ 3) — ĐÃ XONG ✅:** 
  - Nạp trọn vẹn 2 bảng thẩm định chéo A3 trong `data/`: [a3_field_agreement.csv](file:///d:/introds-arxiv-book/data/a3_field_agreement.csv) và [a3_api_recheck.csv](file:///d:/introds-arxiv-book/data/a3_api_recheck.csv) vào [03-performance.ipynb](file:///d:/introds-arxiv-book/milestone1/03-performance.ipynb).
  - Viết thuyết minh khoa học chuyên sâu về ma trận đồng thuận metadata (API vs OAI, API vs Kaggle, OAI vs Kaggle), tác động của chuẩn hóa nâng cao và hiện tượng trễ cập nhật metadata (Snapshot Synchronization Lag & Journal Reference Propagation Delay).
  - Re-execute notebook thành công với exit code 0.
- [ ] **11/11 (Thứ 4) — VIỆC TIẾP THEO ⏳:**
  - Xây dựng notebook mới `milestone1/04-deep-analysis.ipynb` gồm 6 phân tích chuyên sâu:
    1. Phân bố số version mỗi bài (`discovery.csv` cột `n_versions` $\rightarrow$ tiền đề dedup M2).
    2. Phân bố số file `.tex` mỗi version (`duy2_prescan_versions.csv` $\rightarrow$ độ khó multi-file).
    3. Tỉ lệ version có `.bib` riêng vs `\bibitem` inline (minh chứng khối lượng bóc tách của Duy).
    4. Tỉ lệ reference có arXiv ID (matching pool ~490k ứng viên cho M3).
    5. Phân loại lỗi crawl chi tiết.
    6. Timeline request vs rate limit arXiv.
  - Cập nhật [D:\introds-arxiv-book\_toc.yml](file:///d:/introds-arxiv-book/_toc.yml) đưa `04-deep-analysis` vào part "Giai đoạn 1".
- [ ] **12/11 (Thứ 5):**
  - Cấu hình CDN `require.js` trong [D:\introds-arxiv-book\_config.yml](file:///d:/introds-arxiv-book/_config.yml).
  - Chuyển 2–3 biểu đồ sang `plotly.express` (tương tác zoom, hover).
  - Kiểm tra trên trình duyệt ẩn danh đảm bảo không trắng trang.
- [ ] **13/11 (Thứ 6):**
  - Khởi tạo 3 notebook cho Milestone 2 trong `introds-arxiv-book/milestone2/`:
    - `01-multi-file-gathering.ipynb`
    - `02-standardization.ipynb`
    - `03-performance.ipynb`
  - Viết toàn bộ phần thuyết minh lý thuyết/phương pháp.
  - Cài đặt hàm `load_or_placeholder()` để nạp placeholder an toàn mà không làm sập build.
  - Cập nhật `_toc.yml`, xóa `milestone2/reserved.md`.
  - Prototype 2 biểu đồ độc đáo: Cây tài liệu tương tác Sunburst & Biểu đồ biến thiên version mutation.
- [ ] **14/11 (Thứ 7):**
  - Rà soát toàn bộ Jupyter Book, nghiệm thu Tuần 1.

#### TUẦN 2 (15/11 → 20/11): Nhận dữ liệu, Cắm số & Bứt phá điểm số
- **15/11 – 16/11:** Nhận dữ liệu kiểm thử từ Hoàng, Thư, Tú, Duy.
- **17/11 (T3):** Thử nghiệm render Sunburst chart trên dữ liệu thật.
- **18/11 (T4 — MỐC CỨNG):** Nhận toàn bộ dữ liệu full: `gather.csv`, `parse.csv`, `bib.csv`, 3 bảng `elements`, `hierarchy`, `bibtex`.
- **19/11 (T5):** Cắm số thật vào 3 notebook M2, tính `dedup_ratio`, so khớp độ chính xác với Gold Standard của Tú và Regex benchmark của Duy.
- **20/11 (T6):** Tổng duyệt 5 tiêu chí chấm điểm, kiểm tra GitHub Pages, chốt link nộp Moodle.

---

## 5. ĐẶC TẢ HỢP ĐỒNG DỮ LIỆU MILESTONE 2 (DATA CONTRACT)
Theo thỏa thuận thiết kế kiến trúc [docs/schema_m2.md](file:///d:/introds-arxiv-project/docs/schema_m2.md):

### 1. Ba bảng đầu ra chuẩn:
* **`elements` (Kho dùng chung khử trùng lặp):**
  * Cột: `element_id` (str), `arxiv_id` (str), `kind` (str), `content` (str).
  * Quy tắc: Một dòng cho mỗi nội dung đã làm sạch khác nhau trên tất cả version.
* **`hierarchy` (Biểu diễn cây theo version):**
  * Cột: `arxiv_id` (str), `version` (int), `element_id` (str), `parent_id` (str).
  * Quy tắc: Một dòng cho mỗi cạnh node $\rightarrow$ cha của từng version.
* **`bibtex` (Bản ghi tài liệu tham khảo hợp nhất):**
  * Cột: `arxiv_id`, `citation_key`, `entry_type`, `title`, `authors`, `year`, `doi`, `journal`, `url`.

### 2. Quy tắc sinh `element_id`:
```python
import hashlib
def element_uid(pub_key: str, kind: str, clean_content: str) -> str:
    """VD: 2310-12345#s1a2b3c4d — cùng nội dung sau cleanup tự động ra cùng id."""
    h = hashlib.sha1(clean_content.encode("utf-8")).hexdigest()[:8]
    return f"{pub_key}#{kind[0]}{h}"
```
*⚠️ Lưu ý:* Băm chạy **SAU** bước chuẩn hóa (`clean_latex` + `normalize_math`) của Tú.

### 3. Bảng `kind` hợp lệ:
`document` · `section` · `subsection` · `subsubsection` · `paragraph` · `itemize` · `item` · `sentence` · `formula` · `figure` (Table gộp vào `figure`).

---

## 6. HƯỚNG DẪN DÀNH CHO AI AGENT KHI TIẾP QUẢN PHIÊN MỚI
Nếu bạn là một AI Agent mới vừa được khởi động trong phiên tiếp theo, hãy thực hiện các bước sau:
1. Chào người dùng và xác nhận: *"Tôi đã đọc `PROJECT_BRIEF.md`, nắm rõ hiện trạng nhóm đã hoàn thành benchmark Colab CPU-only ngày 09/11 và sẵn sàng thực hiện task tiếp theo cho Tuấn (Task 10/11: Nạp 2 bảng A3 vào `03-performance.ipynb`)."*
2. Khi thực thi các lệnh Python / Jupyter:
   - Sử dụng lệnh: `py -3 -m jupyter nbconvert --to notebook --execute --inplace <notebook_path>` để cập nhật notebook.
   - Thư mục làm việc của Book là `D:\introds-arxiv-book`.
   - Giữ nguyên tắc: **Không bao giờ bịa số liệu, mọi con số đều phải trích từ file CSV trong `data/`**.
