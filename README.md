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

### 1. [Tiền xử lý dữ liệu hình ảnh] Chest X-Ray Images (Pneumonia)

#### 1.1. Giới thiệu chung

Bộ dữ liệu [**Chest X-Ray Images (Pneumonia)**](https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia/data) là tập ảnh X-quang ngực (anterior–posterior) được thu thập từ **Guangzhou Women and Children’s Medical Center**, Trung Quốc.
Tập dữ liệu được sử dụng rộng rãi trong các nghiên cứu về **phân loại bệnh viêm phổi (Pneumonia) và phổi bình thường (Normal)**, và hiện có sẵn trên Kaggle, Mendeley Data, cùng được mô tả chi tiết trong bài báo khoa học *Kermany et al., “Identifying Medical Diagnoses and Treatable Diseases by Image-Based Deep Learning”, Cell, 2018.*

Hình minh họa (Figure S6) cho thấy:

- Ảnh **Normal** (bên trái): phổi trong, không có vùng mờ bất thường.
- Ảnh **Bacterial Pneumonia** (giữa): thể hiện vùng tổn thương đặc trưng dạng đặc thù thùy (lobar consolidation) — ví dụ ở thùy trên phổi phải.
- Ảnh **Viral Pneumonia** (phải): biểu hiện dạng mờ lan tỏa (interstitial pattern) ở cả hai phổi.
*(Nguồn: [Cell, 2018](http://www.cell.com/cell/fulltext/S0092-8674(18)30154-5))*

#### 1.2. Cấu trúc và nội dung dữ liệu

Bộ dữ liệu bao gồm **5,863 ảnh X-quang (JPEG)**, được chia thành ba tập riêng biệt:

- **train/** — tập huấn luyện
- **val/** — tập kiểm định
- **test/** — tập kiểm tra

Mỗi tập con đều có hai thư mục chính:

- `NORMAL/` – ảnh X-quang phổi bình thường
- `PNEUMONIA/` – ảnh X-quang có dấu hiệu viêm phổi (bacterial hoặc viral)

Cấu trúc thư mục:

```bash
tree chest_xray -L 2
chest_xray
├── chest_xray
│   ├── test
│   ├── train
│   └── val
├── test
│   ├── NORMAL
│   └── PNEUMONIA
├── train
│   ├── NORMAL
│   └── PNEUMONIA
└── val
    ├── NORMAL
    └── PNEUMONIA
```

##### Cài đặt dữ liệu từ Kaggle

```bash
cd data/images
```

- **Với Kaggle CLI**

```bash
#!/bin/bash
kaggle datasets download paultimothymooney/chest-xray-pneumonia
```

- **Với lệnh curl**

```bash
#!/bin/bash
curl -L -o ~/Downloads/chest-xray-pneumonia.zip\
  https://www.kaggle.com/api/v1/datasets/download/paultimothymooney/chest-xray-pneumonia
```

Hoặc có thể tải file zip trực tiếp từ Kaggle đến thư mục `data/images`

##### Cấu trúc sau khi cài đặt dataset

```bash
 tree images -L3
images
└── chest_xray
    ├── chest_xray
    │   ├── test
    │   ├── train
    │   └── val
    ├── test
    │   ├── NORMAL
    │   └── PNEUMONIA
    ├── train
    │   ├── NORMAL
    │   └── PNEUMONIA
    └── val
        ├── NORMAL
        └── PNEUMONIA
```

#### 2.3. Quy trình thu thập và xử lý dữ liệu

- Ảnh X-quang được thu thập từ các bệnh nhi (1–5 tuổi) trong quá trình chăm sóc lâm sàng thông thường.
- Mọi ảnh đều được **sàng lọc chất lượng (quality control)** để loại bỏ ảnh mờ, thiếu sáng hoặc không đọc được.
- Các chẩn đoán được **đánh giá độc lập bởi hai bác sĩ chuyên khoa**, sau đó được xác nhận lại bởi một bác sĩ thứ ba để đảm bảo độ chính xác cao nhất.
- Các ảnh được lưu ở định dạng JPEG, kích thước không đồng nhất, độ phân giải trung bình cao (thường từ 1000×1000 trở lên).

#### 2.4. Ưu điểm và nhược điểm

##### Ưu điểm

- Là bộ dữ liệu **chuẩn y khoa công khai** có nguồn gốc xác thực và quy trình gán nhãn chặt chẽ.
- Cân bằng hợp lý giữa hai lớp, giúp huấn luyện và đánh giá mô hình dễ dàng.
- Có độ phân giải cao, thuận lợi cho các kỹ thuật xử lý ảnh nâng cao (enhancement, segmentation).
- Được sử dụng trong nhiều nghiên cứu Deep Learning nổi bật — giúp dễ so sánh kết quả.

##### Nhược điểm

- Ảnh chỉ bao gồm trẻ nhỏ (1–5 tuổi), do đó mô hình huấn luyện từ tập này có thể **chưa tổng quát cho người lớn**.
- Một số ảnh có **biến thiên ánh sáng, nhiễu, hoặc sai lệch tư thế** khiến cần xử lý cẩn thận (resizing, normalization).
- Không cung cấp thông tin lâm sàng chi tiết (tuổi, giới tính, triệu chứng), nên chỉ dùng cho bài toán thị giác.

#### 2.5. Giấy phép và trích dẫn

- **Nguồn dữ liệu:** [Mendeley Data – rscbjbr9sj/2](https://data.mendeley.com/datasets/rscbjbr9sj/2)
- **License:** CC BY 4.0 (Creative Commons Attribution 4.0)
- **Trích dẫn:**
  Kermany, D. S., et al. *Identifying Medical Diagnoses and Treatable Diseases by Image-Based Deep Learning.*
  *Cell*, 172(5), 1122–1131.e9, 2018. [https://doi.org/10.1016/j.cell.2018.02.010](https://doi.org/10.1016/j.cell.2018.02.010)

#### 2.6. Tổng kết

Bộ dữ liệu **Chest X-Ray Images (Pneumonia)** là nguồn dữ liệu y học công khai, chất lượng cao, phù hợp cho các nghiên cứu xử lý ảnh và học sâu trong chẩn đoán bệnh phổi.
Quy trình tiền xử lý (resizing, grayscale, normalization, enhancement, edge detection) trong phần sau sẽ giúp chuẩn hoá và nâng cao chất lượng dữ liệu, tạo nền tảng cho các mô hình khai phá và phân tích tự động.

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
