# [GROUP 05] BÀI TẬP THỰC HÀNH SỐ 01 - TIỀN XỬ LÝ DỮ LIỆU

## I. THÔNG TIN NHÓM & BẢNG PHÂN CÔNG CÔNG VIỆC

### 1. THÔNG TIN NHÓM

Nhóm số 5 gồm có 3 thành viên sau đây thực hiện bài tập thực hành số 1 về chủ đề tiền xử lý các dạng dữ liệu:

- **Thành viên 1:** 22127322 - Lê Phước Phát
- **Thành viên 2:** 23127187 - Trần Lý Nhật Hào
- **Thành viên 3:** 23127384 - Huỳnh Lê Duy Khánh

### 2. BẢNG PHÂN CÔNG VIỆC

|   MSSV   |     Họ và Tên      | Công việc                    | Mức độ hoàn thành |
|:--------:|:------------------:| ---------------------------- |:-----------------:|
| 22127322 |   Lê Phước Phát    | Tiền xử lý dữ liệu dạng bảng |       100%        |
| 23127187 |  Trần Lý Nhật Hào  | Tiền xử lý dữ liệu hình ảnh  |       100%        |
| 23127384 | Huỳnh Lê Duy Khánh | Tiền xử lý dữ liệu dạng bảng |       100%        |

## II. DATASETS

### 1. [Tiền xử lý dữ liệu hình ảnh] ĐIỀN TÊN BỘ DATASET

Điền thông tin ở đây ... (điền mô tả chi tiết về bộ dữ liệu ở đây ...)

### 2. [Tiền xử lý dữ liệu dạng bảng] ĐIỀN TÊN BỘ DATASET

Điền thông tin ở đây ... (điền mô tả chi tiết về bộ dữ liệu ở đây ...)

### 3. [Tiền xử lý dữ liệu văn bản] Rumor Detection Dataset (Twitter15 and Twitter16)

#### 3.1. Giới thiệu chung

Bộ dữ liệu [Rumor Detection Dataset (Twitter15 and Twitter16)](https://www.kaggle.com/datasets/syntheticprogrammer/rumor-detection-acl-2017?select=twitter15)này đã cung cấp đầy đủ các tweets đã được gán nhãn từ bộ dữ liệu Twitter 15 và Twitter 16, trong đó bộ dữ liệu này thường được sử dụng cho mục đích nghiên cứu phát hiện và phân loại các tin giả hay các tin không có đầy đủ thông tin. Mỗi tweet được phân loại là rumor hay non-rumor, điều này làm cho nó phù hợp để đào tạo các mô hình có giám sát cho các nhiệm vụ phân loại nhị phân.

Bộ dữ liệu tập trung vào các tweet nguồn, bỏ qua chuỗi hội thoại hoặc siêu dữ liệu bổ sung, để có thể tiếp cận trực tiếp nhằm hiểu mối quan hệ giữa nội dung tweet và phân loại tin đồn.

Bộ dữ liệu này gồm 2 tập chính là Twitter 15 và Twitter 16. Mỗi tập gồm:

- Source Tweets:
  - Nội dung văn bản của tweet gốc (source_tweets).
  - Phù hợp với các mô hình phân loại văn bản.

- Labels:
  - Mỗi event (một source tweet cùng cascade replies/retweets) được gán một trong bốn nhãn:
    - True (T) — tin đúng (true rumor / verified true)
    - False (F) — tin sai (false rumor)
    - Unverified (U) — chưa xác thực / không rõ
    - Non-rumor (NR) — không phải tin đồn (ví dụ: ordinary news / factual non-rumor)
  - Đây là kiểu nhãn “finer-grained” (4-way) thường dùng để làm bài toán phân lớp đa nhãn (multi-class)

#### 3.2. Ưu điểm vs. Nhược điểm

##### 3.2.1. Ưu điểm

- Bộ dữ liệu này có cấu trúc, mỗi event là một source tweet kèm cascade replies/retweets (tree/graph). Điều này cho phép nghiên cứu ảnh hưởng của ngữ cảnh reply/stance, luồng thông tin, và biểu hiện lan truyền — khác với tập chỉ chứa tweet rời rạc.
- Đồng thời, bộ dữ liệu này có nhãn chi tiếp 4 lớp (True / False / Unverified / Non-rumor), điều này phù hợp cho các bài toán phân lớp tinh vi (không chỉ binary), giúp đánh giá độ nhạy mô hình về nhiều dạng “tin”.
- Bộ dữ liệu này đã được dùng rộng, có baseline và splits sẵn. Điều này được minh chứng rằng nhiều công trình so sánh trên Twitter15/16 nên dễ benchmark, có các train/dev/test chuẩn để so sánh công bằng.

##### 3.2.2. Nhược điểm

- Bộ dữ liệu này có kích thước nhỏ (Twitter15 ≈ 1,490 event; Twitter16 ≈ 818 event). Điều này dễ gây biến động kết quả, overfitting, và giới hạn khả năng tổng quát hóa. Hậu quả là cần chạy nhiều seed / cross-validation; kết quả so sánh có thể không ổn định nếu chỉ một vài seed.
- Bộ dữ liệu này nhãn có thể chứa noise / định nghĩa không đồng nhất

#### 3.3 Tổng kết

**Twitter15/16** là benchmark rất hữu ích cho việc muốn thử các mô hình tận dụng cấu trúc propagation và so sánh với literature trước đây; nhưng không phù hợp nếu nhu cầu cần tập lớn, mới, hoặc muốn đảm bảo khả năng tổng quát hoá cao — vì kích thước nhỏ, vấn đề hydrate và nhiễu nhãn.

## III. CẤU TRÚC THƯ MỤC

```sh
Group_05/
├── README.md
├── requirements.txt
├── data/                               # Thư mục dùng để chứa các bộ dữ liệu
│   ├── images/                         # Thư mục chứa các dữ liệu dạng hình ảnh
│   ├── tabular/                        # Thư mục chứa các dữ liệu dạng bảng
│   │   └── cities15000.csv     
│   └── text/                           # Thư mục chứa các dữ liệu dạng văn bản
│       ├── twitter15/
│       │   ├── label.txt
│       │   └── source_tweets.txt
│       └── twitter16/
│           ├── label.txt
│           └── source_tweets.txt
├── notebooks/                          # Thư mục chứa các file notebook tiền xử lý dữ liệu
│   ├── 01_image_preprocessing.ipynb    # Tiền xử lý dữ liệu hình ảnh
│   ├── 02_tabular_preprocessing.ipynb  # Tiền xử lý dữ liệu dạng bảng
│   └── 03_text_preprocessing.ipynb     # Tiền xử lý dữ liệu dạng văn bản
└── docs/                               # Thư mục chứa báo cáo
    └── Report.pdf                      # Báo cáo của nhóm
```

## IV. HƯỚNG DẪN CÀI ĐẶT

### 1. Điều kiện tiên quyết

Để có thể sử dụng và cài đặt hoàn chỉnh source code này, bạn cần có các điều kiện tiên quyết sau:

- Phiên bản python: 3.11 hoặc mới hơn. Để kiểm tra điều này, bạn cần phải kiểm tra bằng command sau:

    ```bash
    python3 --version
    # hoặc
    python --version
    ```

- Phiên bản pip: mới nhất. Cập nhật bằng:

    ```bash
    python -m pip install --upgrade pip
    ```

### 2. Cài đặt chi tiết

Đầu tiên, cần phải cài đặt môi trường ảo bằng command sau đây:

```bash
python3 -m venv venv
```

Sau đó, bạn cần phải kích hoạt môi trường ảo bằng command sau đây:

```bash
source venv/bin/activate
```

Tiếp theo, bạn cần check xem mình đã vào đúng thư mục `Group_05` chưa. Nếu chưa bạn cần chạy lệnh sau để vào đúng thư mục gốc và cài đặt thư viện cho môi trường ảo bằng command sau đây:

```bash
cd Group_05
pip install -r requirements.txt
```

Bước tiếp theo là bạn cần cài đặt `ipykernel` trong `venv` để notebook có thể chọn kernel này

```bash
pip install --upgrade pip # kiểm tra xem pip đã được cập nhật hay chưa
pip install ipykernel # cài đặt ipykernel (nếu chưa cài đặt)
python -m ipykernel install --user --name=.venv --display-name "Python (.venv)"
```

Nếu bạn dùng VS Code: mở file `.ipynb`, click header phải trên cùng (kernel/interpreter) $\to$ chọn **Python (.venv)** (hoặc chọn interpreter: `Cmd+Shift+P` $\to$ `Python: Select Interpreter` $\to$ chọn `./.venv/bin/python`).
Nếu bạn dùng **Jupyter Notebook / JupyterLab:** mở notebook $\to$ từ menu kernel $\to$ **Change kernel** $\to$ chọn **Python (.venv)**.

Cuối cùng bạn chỉ cần chạy từng cell trong file notebook là chạy được code rồi !!!!

## V. TÀI LIỆU HỖ TRỢ

### 1. Google Drive

Trong google drive này, nhóm đã chứa các bộ dataset cho từng tác vụ cụ thể trong link sau: [LAB01-DATAMINING-DRIVE](https://drive.google.com/drive/folders/1RD1gCSLnfA4qb081HXLhLbLMinKWM0SR?usp=sharing)

### 2. Github

Toàn bộ source code được nhóm quản lý trên Github trong đường dẫn sau: [LAB01-DATAMINING-GITHUB](https://github.com/PhuocPhat1005/LAB01-DATAMINING.git)

### 3. Datasets

Toàn bộ đường dẫn để tải từng bộ dữ liệu được trình bày chi tiết sau đây:

- Dữ liệu hình ảnh: []() -> Điền link tải dữ liệu giúp anh vào đây để download được dữ liệu
- Dữ liệu dạng bảng: []() -> Điền link tải dữ liệu giúp anh vào đây để download được dữ liệu
- Dữ liệu văn bản: [Rumor Detection Dataset (Twitter15 and Twitter16)](https://www.kaggle.com/datasets/syntheticprogrammer/rumor-detection-acl-2017?select=twitter15)

### 4. Tài liệu tham khảo

Điền thông tin chi tiết về tài liệu tham khảo ở đây ... (Nếu có)
