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

#### 1.1. Giới thiệu chung về dữ liệu hình ảnh

- Bộ dữ liệu [**Chest X-Ray Images (Pneumonia)**](https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia/data) là tập ảnh X-quang ngực (anterior–posterior) được thu thập từ **Guangzhou Women and Children’s Medical Center**, Trung Quốc.
Tập dữ liệu được sử dụng rộng rãi trong các nghiên cứu về **phân loại bệnh viêm phổi (Pneumonia) và phổi bình thường (Normal)**, và hiện có sẵn trên Kaggle, Mendeley Data, cùng được mô tả chi tiết trong bài báo khoa học *Kermany et al., “Identifying Medical Diagnoses and Treatable Diseases by Image-Based Deep Learning”, Cell, 2018.*
- Hình minh họa (Figure S6) cho thấy:
  - Ảnh **Normal** (bên trái): phổi trong, không có vùng mờ bất thường.
  - Ảnh **Bacterial Pneumonia** (giữa): thể hiện vùng tổn thương đặc trưng dạng đặc thù thùy (lobar consolidation) — ví dụ ở thùy trên phổi phải.
  - Ảnh **Viral Pneumonia** (phải): biểu hiện dạng mờ lan tỏa (interstitial pattern) ở cả hai phổi.
  *(Nguồn: [Cell, 2018](http://www.cell.com/cell/fulltext/S0092-8674(18)30154-5))*

#### 1.2. Cấu trúc và nội dung dữ liệu hình ảnh

- Bộ dữ liệu bao gồm **5,863 ảnh X-quang (JPEG)**, được chia thành ba tập riêng biệt:
  - **train/** — tập huấn luyện
  - **val/** — tập kiểm định
  - **test/** — tập kiểm tra
- Mỗi tập con đều có hai thư mục chính:
  - `NORMAL/` – ảnh X-quang phổi bình thường
  - `PNEUMONIA/` – ảnh X-quang có dấu hiệu viêm phổi (bacterial hoặc viral)
- Cấu trúc thư mục:

```bash
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

#### 2.3. Quy trình thu thập và xử lý dữ liệu hình ảnh

- Ảnh X-quang được thu thập từ các bệnh nhi (1–5 tuổi) trong quá trình chăm sóc lâm sàng thông thường.
- Mọi ảnh đều được **sàng lọc chất lượng (quality control)** để loại bỏ ảnh mờ, thiếu sáng hoặc không đọc được.
- Các chẩn đoán được **đánh giá độc lập bởi hai bác sĩ chuyên khoa**, sau đó được xác nhận lại bởi một bác sĩ thứ ba để đảm bảo độ chính xác cao nhất.
- Các ảnh được lưu ở định dạng JPEG, kích thước không đồng nhất, độ phân giải trung bình cao (thường từ 1000×1000 trở lên).

#### 2.4. Ưu điểm & nhược điểm về dữ liệu hình ảnh

##### Ưu điểm về dữ liệu hình ảnh

- Là bộ dữ liệu **chuẩn y khoa công khai** có nguồn gốc xác thực và quy trình gán nhãn chặt chẽ.
- Cân bằng hợp lý giữa hai lớp, giúp huấn luyện và đánh giá mô hình dễ dàng.
- Có độ phân giải cao, thuận lợi cho các kỹ thuật xử lý ảnh nâng cao (enhancement, segmentation).
- Được sử dụng trong nhiều nghiên cứu Deep Learning nổi bật — giúp dễ so sánh kết quả.

##### Nhược điểm về dữ liệu hình ảnh

- Ảnh chỉ bao gồm trẻ nhỏ (1–5 tuổi), do đó mô hình huấn luyện từ tập này có thể **chưa tổng quát cho người lớn**.
- Một số ảnh có **biến thiên ánh sáng, nhiễu, hoặc sai lệch tư thế** khiến cần xử lý cẩn thận (resizing, normalization).
- Không cung cấp thông tin lâm sàng chi tiết (tuổi, giới tính, triệu chứng), nên chỉ dùng cho bài toán thị giác.

#### 2.5. Giấy phép và trích dẫn

- **Nguồn dữ liệu:** [Mendeley Data – rscbjbr9sj/2](https://data.mendeley.com/datasets/rscbjbr9sj/2)
- **License:** CC BY 4.0 (Creative Commons Attribution 4.0)
- **Trích dẫn:**
  Kermany, D. S., et al. *Identifying Medical Diagnoses and Treatable Diseases by Image-Based Deep Learning.*
  *Cell*, 172(5), 1122–1131.e9, 2018. [https://doi.org/10.1016/j.cell.2018.02.010](https://doi.org/10.1016/j.cell.2018.02.010)

#### 2.6. Tổng kết về bộ dữ liệu hình ảnh

Bộ dữ liệu **Chest X-Ray Images (Pneumonia)** là nguồn dữ liệu y học công khai, chất lượng cao, phù hợp cho các nghiên cứu xử lý ảnh và học sâu trong chẩn đoán bệnh phổi.
Quy trình tiền xử lý (resizing, grayscale, normalization, enhancement, edge detection) trong phần sau sẽ giúp chuẩn hoá và nâng cao chất lượng dữ liệu, tạo nền tảng cho các mô hình khai phá và phân tích tự động.

### 2. [Tiền xử lý dữ liệu dạng bảng] Cities15000

#### 2.1. Giới thiệu chung về dữ liệu dạng bảng

- Bộ dữ liệu [**Cities15000**](https://download.geonames.org/export/dump/) được trích xuất từ cơ sở dữ liệu **GeoNames**, một nguồn dữ liệu mã nguồn mở toàn cầu cung cấp thông tin địa lý của hơn 11 triệu địa điểm trên thế giới.
- Dữ liệu có thể được tải qua link kaggle: [tại đây](https://www.kaggle.com/datasets/i2i2i2/cities-of-the-world/code)
- Tập dữ liệu này chứa các thông tin chi tiết về hơn **15.000 thành phố có dân số lớn nhất**, bao gồm các thuộc tính như **tên địa danh, vĩ độ, kinh độ, quốc gia, mã hành chính, dân số, múi giờ, độ cao**, và các mã phân loại địa lý khác.  
- Bộ dữ liệu **Cities15000** thường được sử dụng trong các bài toán **phân tích không gian địa lý**, **mô hình hoá dân cư**, **hệ thống gợi ý dựa trên vị trí**, cũng như trong các ứng dụng **học máy (Machine Learning)** để huấn luyện mô hình dự đoán hoặc phân cụm theo đặc trưng địa lý.

|     **Tên biến**      | **Mô tả**                                                                                |
|:---------------------:| ---------------------------------------------------------------------------------------- |
|     **geonameid**     | Mã định danh duy nhất cho mỗi thành phố trong cơ sở dữ liệu GeoNames.                    |
|       **name**        | Tên chính thức của thành phố.                                                            |
|     **asciiname**     | Tên thành phố được chuyển sang dạng ASCII (loại bỏ dấu và ký tự đặc biệt).               |
|  **alternatenames**   | Danh sách các tên khác hoặc tên thay thế của thành phố.                                  |
|     **latitude**      | Vĩ độ của thành phố, tính bằng độ (°).                                                   |
|     **longitude**     | Kinh độ của thành phố, tính bằng độ (°).                                                 |
|   **feature class**   | Nhóm đặc trưng tổng quát của thực thể địa lý (ví dụ: dân cư, sông, núi, v.v.).           |
|   **feature code**    | Mã chi tiết hơn mô tả loại địa lý cụ thể (ví dụ: thủ đô, thành phố, làng).               |
|   **country code**    | Mã quốc gia theo chuẩn ISO.                                                              |
|        **cc2**        | Mã quốc gia phụ, dùng khi một địa danh thuộc nhiều quốc gia.                             |
|    **admin1 code**    | Mã cấp hành chính 1 (ví dụ: bang, tỉnh, tiểu bang).                                      |
|    **admin2 code**    | Mã cấp hành chính 2 (ví dụ: quận, huyện).                                                |
|    **admin3 code**    | Mã cấp hành chính 3 (ví dụ: xã, phường).                                                 |
|    **admin4 code**    | Mã cấp hành chính 4 (đơn vị hành chính nhỏ nhất, nếu có).                                |
|    **population**     | Dân số ước tính của thành phố (tính bằng người).                                         |
|     **elevation**     | Độ cao của thành phố so với mực nước biển (mét).                                         |
|        **dem**        | Độ cao trung bình từ mô hình số địa hình (Digital Elevation Model).                      |
|     **timezone**      | Múi giờ của thành phố (ví dụ: `Asia/Ho_Chi_Minh`).                                       |
| **modification date** | Ngày cập nhật cuối cùng của bản ghi trong cơ sở dữ liệu GeoNames (định dạng YYYY-MM-DD). |

#### 2.2. Cấu trúc và nội dung dữ liệu dạng bảng

- Bộ dữ liệu **Cities15000** là một tập con của cơ sở dữ liệu **GeoNames**, bao gồm thông tin về **hơn 23.000 thành phố trên toàn thế giới** có dân số từ **15.000 người trở lên**.  
Mỗi dòng (record) trong bộ dữ liệu biểu diễn **một thành phố hoặc khu dân cư**, kèm theo các đặc trưng về **địa lý, hành chính, dân số, và thời gian**.
- **Cấu trúc tổng quát**
  - **Số dòng (rows):** 23,468  
  - **Số cột (columns):** 19  
  - **Định dạng tệp:** CSV (`cities15000.csv`)  
  - **Bảng mã (encoding):** `latin1`  

#### 2.3. Quy trình thu thập và xử lý dữ liệu dạng bảng

Bộ dữ liệu **Cities15000** được trích xuất từ cơ sở dữ liệu **GeoNames** – một dự án mã nguồn mở tổng hợp thông tin địa lý toàn cầu.

#### 2.4. Ưu điểm & nhược điểm của bộ dữ liệu dạng bảng

##### Ưu điểm của bộ dữ liệu dạng bảng

1. **Nguồn dữ liệu chính thống và công khai**  
   - Được tổng hợp từ **GeoNames**, một cơ sở dữ liệu địa lý uy tín toàn cầu.  
   - Có giấy phép **mở (CC-BY)**, cho phép sử dụng tự do cho nghiên cứu và ứng dụng.

2. **Cập nhật thường xuyên**  
   - GeoNames được cập nhật **hàng ngày**, giúp đảm bảo độ mới và chính xác cao.  
   - Mỗi bản ghi có trường `modification date` ghi nhận ngày cập nhật cuối cùng.

3. **Phạm vi toàn cầu**  
   - Bao phủ **hơn 23.000 thành phố** trên toàn thế giới có dân số ≥ 15.000 người.  
   - Dữ liệu bao gồm đầy đủ **tọa độ, dân số, mã hành chính, múi giờ**, v.v.

4. **Định dạng dễ sử dụng**  
   - Dữ liệu dạng **tab-separated (TSV)** hoặc **CSV**, dễ dàng đọc bằng Python, R, SQL, Excel.  
   - Các cột được mô tả rõ ràng và thống nhất.

5. **Tính đa dạng và mở rộng**  
   - Bao gồm nhiều loại thông tin: địa lý, hành chính, dân số, thời gian.  
   - Có thể dễ dàng kết hợp với các tập dữ liệu khác (như khí hậu, kinh tế, hoặc bản đồ GIS).

##### Nhược điểm của dữ liệu dạng bảng

1. **Dữ liệu thiếu hoặc không đồng nhất**  
   - Một số trường có **tỷ lệ thiếu dữ liệu cao** (ví dụ: `admin4 code`, `elevation`, `cc2`).  
   - Dữ liệu có thể **không đồng nhất giữa các quốc gia**, do sự khác biệt trong hệ thống hành chính.

2. **Độ chính xác chưa tuyệt đối**  
   - Vị trí địa lý (`latitude`, `longitude`) và dân số có thể **chênh lệch** so với số liệu thống kê mới nhất.  
   - Không có cam kết chính thức về **độ chính xác tuyệt đối** của dữ liệu.

3. **Thiếu dữ liệu mở rộng**  
   - Không bao gồm thông tin về **kinh tế, diện tích, GDP, ngôn ngữ, khí hậu**,…  
   - Không có **mối quan hệ giữa các thành phố** (ví dụ: khoảng cách, kết nối giao thông).

4. **Không có siêu dữ liệu (metadata) chi tiết**  
   - GeoNames chỉ mô tả cột dữ liệu, **không kèm thông tin về nguồn gốc từng bản ghi**.  
   - Việc truy vết hoặc xác thực nguồn dữ liệu gặp khó khăn.

5. **Khó xử lý khi tích hợp hệ thống lớn**  
   - Dữ liệu dạng text thô, chưa chuẩn hóa thành cơ sở dữ liệu quan hệ (relational schema).  
   - Cần tiền xử lý kỹ nếu dùng trong các ứng dụng học máy hoặc GIS phức tạp.

#### 2.5. Giấy phép và trích dẫn về dữ liệu dạng bảng

- **Nguồn dữ liệu:** [GeoName](https://download.geonames.org/export/dump/)
- **License:** Creative Commons Attribution 4.0 License (CC-BY 4.0)

#### 2.6. Tổng kết về bộ dữ liệu dạng bảng

Bộ dữ liệu **Cities15000** là một nguồn dữ liệu địa lý có giá trị cao, chứa thông tin chi tiết về hơn **15.000 thành phố và khu dân cư lớn trên toàn cầu**, bao gồm vị trí địa lý, mã hành chính, dân số, độ cao và múi giờ.  

Trong nghiên cứu này, dữ liệu được sử dụng cho mục tiêu **tiền xử lý, phân tích, và huấn luyện mô hình học máy**. Qua quá trình xử lý gồm loại bỏ cột không cần thiết, điền giá trị thiếu, chuẩn hóa và mã hóa dữ liệu, tập dữ liệu đã trở nên **sạch, cân bằng và có cấu trúc tốt hơn**, phù hợp cho các bài toán như **phân loại, hồi quy, hoặc phân cụm địa lý**.

Nhìn chung:

- Dữ liệu có **độ bao phủ toàn cầu**, **dễ truy cập**, và **cập nhật thường xuyên**, rất phù hợp cho mục đích học thuật và thực hành ML.  
- Tuy nhiên, vẫn tồn tại một số **thiếu sót nhỏ** như giá trị thiếu trong các trường hành chính hoặc sai lệch về độ cao ở một số khu vực.

### 3. [Tiền xử lý dữ liệu văn bản] Rumor Detection Dataset (Twitter15 and Twitter16)

#### 3.1. Giới thiệu chung về dữ liệu văn bản

Bộ dữ liệu [Rumor Detection Dataset (Twitter15 and Twitter16)](https://www.kaggle.com/datasets/syntheticprogrammer/rumor-detection-acl-2017?select=twitter15)này đã cung cấp đầy đủ các tweets đã được gán nhãn từ bộ dữ liệu Twitter 15 và Twitter 16, trong đó bộ dữ liệu này thường được sử dụng cho mục đích nghiên cứu phát hiện và phân loại các tin giả hay các tin không có đầy đủ thông tin. Mỗi tweet được phân loại là rumor hay non-rumor, điều này làm cho nó phù hợp để đào tạo các mô hình có giám sát cho các nhiệm vụ phân loại nhị phân.

Bộ dữ liệu tập trung vào các tweet nguồn, bỏ qua chuỗi hội thoại hoặc siêu dữ liệu bổ sung, để có thể tiếp cận trực tiếp nhằm hiểu mối quan hệ giữa nội dung tweet và phân loại tin đồn.

#### 3.2. Cấu trúc và nội dung dữ liệu văn bản

- Bộ dữ liệu này gồm 2 tập chính là Twitter 15 và Twitter 16. Mỗi tập gồm:
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

#### 3.3. Ưu điểm & nhược điểm của dữ liệu văn bản

##### 3.3.1. Ưu điểm của dữ liệu văn bản

- Bộ dữ liệu này có cấu trúc, mỗi event là một source tweet kèm cascade replies/retweets (tree/graph). Điều này cho phép nghiên cứu ảnh hưởng của ngữ cảnh reply/stance, luồng thông tin, và biểu hiện lan truyền — khác với tập chỉ chứa tweet rời rạc.
- Đồng thời, bộ dữ liệu này có nhãn chi tiếp 4 lớp (True / False / Unverified / Non-rumor), điều này phù hợp cho các bài toán phân lớp tinh vi (không chỉ binary), giúp đánh giá độ nhạy mô hình về nhiều dạng “tin”.
- Bộ dữ liệu này đã được dùng rộng, có baseline và splits sẵn. Điều này được minh chứng rằng nhiều công trình so sánh trên Twitter15/16 nên dễ benchmark, có các train/dev/test chuẩn để so sánh công bằng.

##### 3.3.2. Nhược điểm của dữ liệu văn bản

- Bộ dữ liệu này có kích thước nhỏ (Twitter15 ≈ 1,490 event; Twitter16 ≈ 818 event). Điều này dễ gây biến động kết quả, overfitting, và giới hạn khả năng tổng quát hóa. Hậu quả là cần chạy nhiều seed / cross-validation; kết quả so sánh có thể không ổn định nếu chỉ một vài seed.
- Bộ dữ liệu này nhãn có thể chứa noise / định nghĩa không đồng nhất

#### 3.4. Giấy phép và trích dẫn của dữ liệu văn bản

- **Nguồn dữ liệu:** [Rumor Detection Dataset (Twitter15 and Twitter16)](https://www.kaggle.com/datasets/syntheticprogrammer/rumor-detection-acl-2017?select=twitter15)
- **Trích dẫn:** Chunyuan Yuan, Qianwen Ma, Wei Zhou, Jizhong Han, Songlin Hu. Jointly embedding the local and global relations of heterogeneous graph for rumor detection. In 19th IEEE International Conference on Data Mining, IEEE ICDM 2019.

#### 3.5. Tổng kết về dữ liệu văn bản

**Twitter15/16** là benchmark rất hữu ích cho việc muốn thử các mô hình tận dụng cấu trúc propagation và so sánh với literature trước đây; nhưng không phù hợp nếu nhu cầu cần tập lớn, mới, hoặc muốn đảm bảo khả năng tổng quát hoá cao — vì kích thước nhỏ, vấn đề hydrate và nhiễu nhãn.

## III. CẤU TRÚC THƯ MỤC

```sh
Group_05/
├── README.md
├── requirements.txt
├── data/                               # Thư mục dùng để chứa các bộ dữ liệu
│   ├── images/                         # Thư mục chứa các dữ liệu dạng hình ảnh
│   │    └── chest_xray
│   │        ├── chest_xray
│   │        │   ├── test
│   │        │   ├── train
│   │        │   └── val
│   │        ├── test
│   │        │   ├── NORMAL
│   │        │   └── PNEUMONIA
│   │        ├── train
│   │        │   ├── NORMAL
│   │        │   └── PNEUMONIA
│   │        └── val
│   │             ├── NORMAL
│   │             └── PNEUMONIA
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
source .venv/bin/activate
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

- Toàn bộ đường dẫn để tải từng bộ dữ liệu được trình bày chi tiết sau đây:
  - Dữ liệu hình ảnh: [**Mendeley Data – rscbjbr9sj/2**](https://data.mendeley.com/datasets/rscbjbr9sj/2)
  - Dữ liệu dạng bảng: [**Cities15000**](https://www.kaggle.com/datasets/i2i2i2/cities-of-the-world/code)
  - Dữ liệu văn bản: [**Rumor Detection Dataset (Twitter15 and Twitter16)**](https://www.kaggle.com/datasets/syntheticprogrammer/rumor-detection-acl-2017?select=twitter15)
- Do dữ liệu đã vượt quá hạn mức dung lượng 25MB, nên nhóm chỉ nộp source code và report trên moodle. Còn các file ảnh, bộ dữ liệu sẽ được nhóm đưa lên [Drive](https://drive.google.com/drive/folders/1RD1gCSLnfA4qb081HXLhLbLMinKWM0SR?usp=sharing) và chia sẻ link trong file README.md.
