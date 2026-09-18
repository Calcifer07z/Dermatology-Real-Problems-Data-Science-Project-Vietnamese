# Dermatology Real Problems

**Portfolio Data Science Project 3**

Dự án xây dựng một bài toán **Multi-class Classification** nhằm phân loại 6 nhóm bệnh da thuộc nhóm **erythemato-squamous** dựa trên các đặc trưng lâm sàng và mô bệnh học.

Mục tiêu của project không chỉ dừng lại ở việc xây dựng một mô hình có hiệu năng cao, mà tập trung vào toàn bộ quy trình Data Science: từ hiểu bài toán, khám phá dữ liệu, Data Cleaning, EDA, Feature Engineering, xây dựng Modeling Pipeline, Cross-Validation, Hyperparameter Tuning cho đến Model Interpretability bằng SHAP.

> **Lưu ý:** Đây là một project Machine Learning mang tính nghiên cứu và học thuật. Mô hình không được sử dụng như một công cụ chẩn đoán y khoa độc lập.

---

## 1. Bài toán đặt ra

Chẩn đoán phân biệt các bệnh **erythemato-squamous** là một bài toán khó trong da liễu vì nhiều bệnh có những biểu hiện lâm sàng tương đồng, đặc biệt là **ban đỏ (erythema)** và **bong vảy (scaling)**.

Project tập trung phân loại 6 nhóm bệnh:

* Psoriasis
* Seborrheic dermatitis
* Lichen planus
* Pityriasis rosea
* Chronic dermatitis
* Pityriasis rubra pilaris

Dữ liệu bao gồm:

* **12 clinical features** được thu thập từ quá trình khám lâm sàng.
* **22 histopathological features** được xác định thông qua phân tích mẫu da dưới kính hiển vi.
* `age`: tuổi bệnh nhân.
* `family_history`: tiền sử gia đình.

Các đặc trưng lâm sàng và mô bệnh học chủ yếu được biểu diễn dưới dạng mức độ từ **0 đến 3**:

| Giá trị | Ý nghĩa                   |
| ------: | ------------------------- |
|       0 | Đặc trưng không xuất hiện |
|       1 | Mức độ thấp               |
|       2 | Mức độ trung gian         |
|       3 | Mức độ cao nhất           |

Chi tiết ý nghĩa của từng feature được mô tả trong file `Data Dictionary.md`.

---

## 2. Mục tiêu của Project

Project được xây dựng với các mục tiêu chính:

1. Hiểu cấu trúc và đặc điểm của bộ dữ liệu dermatology.
2. Làm sạch dữ liệu và xử lý missing values.
3. Sử dụng EDA để tìm ra các pattern và signal giữa features với target.
4. Kiểm tra mối quan hệ giữa các đặc trưng mô bệnh học.
5. Xây dựng các feature mới dựa trên những pattern được phát hiện trong EDA.
6. So sánh hiệu năng của nhiều thuật toán Machine Learning.
7. Đánh giá tính ổn định của mô hình thông qua **Stratified 5-Fold Cross-Validation**.
8. Kiểm tra tác động của các phương pháp Oversampling.
9. Tối ưu Hyperparameter cho mô hình được lựa chọn.
10. Đánh giá mô hình cuối cùng trên Test Set.
11. Sử dụng SHAP để giải thích cách mô hình đưa ra dự đoán.

---

# 3. Dataset Overview

Bộ dữ liệu ban đầu có:

* **366 samples**
* **35 columns**
* **12,810 cells**
* Không có duplicated rows.
* Các feature chủ yếu là numerical/discrete values.
* `age` ban đầu có datatype dạng string.
* Có **8 giá trị `?`** trong `age`.

Phân phối target:

|   Class | Số lượng |
| ------: | -------: |
| Class 1 |      112 |
| Class 2 |       72 |
| Class 3 |       61 |
| Class 4 |       52 |
| Class 5 |       49 |
| Class 6 |       20 |

Dataset có sự mất cân bằng class tương đối nhẹ, trong đó `Class 6` là minority class rõ rệt nhất.

---

# 4. Data Science Workflow

Toàn bộ project được triển khai theo workflow:

```text
Problem Understanding
        ↓
Data Exploration
        ↓
Basic Data Cleaning
        ↓
Exploratory Data Analysis
        ↓
Feature Engineering
        ↓
Train-Test Split
        ↓
Preprocessing Pipeline
        ↓
Baseline & Model Comparison
        ↓
Oversampling Experiment
        ↓
Hyperparameter Tuning
        ↓
Final Test Evaluation
        ↓
Model Interpretability with SHAP
        ↓
Executive Summary
```

---

# 5. Data Cleaning

Các bước xử lý dữ liệu chính:

* Kiểm tra kích thước dataset.
* Kiểm tra datatype.
* Kiểm tra missing values.
* Kiểm tra duplicated rows.
* Chuyển giá trị `?` trong `age` thành `NaN`.
* Loại bỏ các sample có missing `age`.
* Chuyển `age` từ `str` sang kiểu số.

Do chỉ có 8/366 sample chứa missing value ở `age`, việc loại bỏ các sample này không làm mất quá nhiều dữ liệu.

---

# 6. Exploratory Data Analysis

EDA được chia thành hai hướng chính:

### 6.1. Phân tích đơn biến

Phân tích phân phối của các feature bằng:

* Bar Plot
* Histogram
* Box Plot

Một số pattern được quan sát:

* Ban đỏ và bong vảy chủ yếu xuất hiện ở mức độ trung gian.
* Ranh giới giữa vùng da tổn thương và da lành thường có mức độ tương đối rõ.
* Phần lớn bệnh nhân không có tiền sử gia đình mắc các bệnh thuộc nhóm này.
* Phân phối tuổi cho thấy bệnh nhân trong dataset tập trung ở một số độ tuổi nhất định.

### 6.2. Phân tích đa biến

Phân tích:

* Correlation Matrix.
* Quan hệ giữa histopathological features.
* Quan hệ giữa features và target `class`.
* Phân phối class theo từng mức độ của feature.

Một số nhóm feature có tương quan thuận chiều mạnh với nhau, đặc biệt là nhóm liên quan đến:

```text
band_like_infiltrate
polygonal_papules
oral_mucosal_involvement
melanin_incontinence
focal_hypergranulosis
vacuolisation_damage_basal_layer
saw_tooth_appearance_retes
```

EDA cũng cho thấy một số histopathological features có signal đáng chú ý đối với target.

Ví dụ:

| Feature                             | Pearson correlation với `class` |
| ----------------------------------- | ------------------------------: |
| `follicular_papules`                |                            0.49 |
| `fibrosis_papillary_dermis`         |                            0.51 |
| `follicular_horn_plug`              |                            0.44 |
| `perifollicular_parakeratosis`      |                            0.47 |
| `scalp_involvement`                 |                           -0.53 |
| `PNL_infiltrate`                    |                           -0.54 |
| `clubbing_rete_ridges`              |                           -0.68 |
| `thinning_suprapapillary_epidermis` |                           -0.68 |
| `munro_microabcess`                 |                           -0.52 |

Những kết quả này được sử dụng làm cơ sở để đặt giả thuyết cho bước Feature Engineering tiếp theo.

---

# 7. Feature Engineering

Thay vì chỉ đưa từng feature riêng lẻ vào model, project thử xây dựng các feature đại diện cho **pattern tổng thể của một nhóm đặc trưng**.

Một trong những nhóm được khai thác gồm:

```text
band_like_infiltrate
polygonal_papules
oral_mucosal_involvement
melanin_incontinence
focal_hypergranulosis
vacuolisation_damage_basal_layer
saw_tooth_appearance_retes
```

## 7.1. Group Score

Một `Group Score` được tạo bằng cách lấy trung bình các feature trong cùng nhóm:

$$
GroupScore =
\frac{
x_1+x_2+\dots+x_n
}{n}
$$

Ví dụ:

```text
band_like_infiltrate = 3
polygonal_papules = 2
oral_mucosal_involvement = 2
melanin_incontinence = 3
focal_hypergranulosis = 1
vacuolisation_damage_basal_layer = 3
saw_tooth_appearance_retes = 2
```

Khi đó:

$$
GroupScore = \frac{3+2+2+3+1+3+2}{7}=2.29
$$

Ý nghĩa:

* `Group Score ≈ 0`: phần lớn feature trong nhóm không xuất hiện.
* `Group Score ≈ 1`: mức độ thấp.
* `Group Score ≈ 2`: mức độ tương đối cao.
* `Group Score ≈ 3`: phần lớn feature trong nhóm xuất hiện rõ.

Ngoài `Group Score`, project cũng thử nghiệm `count_high`, biểu diễn số lượng feature trong nhóm có mức độ cao.

Các feature được tạo ra không được xem là một quy luật y khoa. Chúng được xem như những giả thuyết từ EDA và được kiểm chứng lại thông qua Cross-Validation.

---

# 8. So sánh DataFrame trước và sau Feature Engineering

Project chủ động tạo hai phiên bản dữ liệu:

### Original DataFrame

Chứa toàn bộ feature ban đầu và không chứa các feature được tạo thêm.

### Engineered DataFrame

Chứa cả feature gốc và các feature được tạo trong Feature Engineering.

Hai DataFrame được đưa vào cùng một quy trình Modeling để kiểm tra liệu các feature mới có thực sự tạo thêm signal cho model hay không.

---

# 9. Modeling Pipeline

Project sử dụng `Pipeline` để kết hợp preprocessing và model.

Quy trình:

```text
Input Data
    ↓
StandardScaler
    ↓
Machine Learning Model
```

Train/Test Split được thực hiện với:

```python
test_size = 0.2
stratify = y
random_state = 42
```

Việc sử dụng `stratify` giúp duy trì phân phối class giữa Train Set và Test Set.

---

# 10. Machine Learning Models

Các model được thử nghiệm:

| Model                     | Vai trò                   |
| ------------------------- | ------------------------- |
| Dummy Classifier          | Baseline                  |
| Logistic Regression       | Linear classification     |
| Decision Tree             | Tree-based model          |
| Random Forest             | Ensemble tree-based model |
| KNN                       | Distance-based model      |
| Support Vector Classifier | Margin-based classifier   |

Do dataset có class imbalance nhẹ, các model phù hợp được thử nghiệm với `class_weight='balanced'`.

---

# 11. Cross-Validation & Evaluation Metrics

Project sử dụng:

```text
StratifiedKFold
n_splits = 5
shuffle = True
random_state = 42
```

Các metrics được theo dõi:

* Accuracy
* Precision Macro
* Recall Macro
* F1 Macro
* ROC-AUC Macro
* Precision Weighted
* Recall Weighted
* F1 Weighted
* ROC-AUC Weighted

## Vì sao sử dụng F1 Macro?

Đây là bài toán multi-class và các class không hoàn toàn cân bằng.

`F1 Macro` tính F1 riêng cho từng class rồi lấy trung bình, do đó mỗi class có trọng số ngang nhau trong metric tổng thể.

Điều này giúp tránh việc class có nhiều sample chi phối quá mạnh kết quả đánh giá.

`Weighted Average` được sử dụng bổ sung để đánh giá hiệu năng theo đúng phân bố sample của dataset.

---

# 12. Model Comparison

Trên DataFrame gốc, các model có kết quả Cross-Validation:

| Model               | Accuracy Mean | F1 Macro Mean | F1 Macro Std |
| ------------------- | ------------: | ------------: | -----------: |
| Random Forest       |        0.9720 |        0.9685 |       0.0341 |
| SVC                 |        0.9686 |        0.9669 |       0.0219 |
| Logistic Regression |        0.9685 |        0.9652 |       0.0148 |
| KNN                 |        0.9650 |        0.9614 |       0.0125 |
| Decision Tree       |        0.9509 |        0.9496 |       0.0445 |
| Dummy Classifier    |        0.3112 |        0.0791 |       0.0013 |

Một điểm đáng chú ý là Random Forest và SVC đạt F1 Macro cao nhưng có độ lệch chuẩn lớn hơn Logistic Regression và KNN.

Điều này cho thấy cần xem xét không chỉ mean score mà cả **độ ổn định giữa các fold**.

Sau khi thực hiện Feature Engineering, Logistic Regression tiếp tục được xem xét kỹ về tính ổn định. Một số độ lệch chuẩn của ROC-AUC được cải thiện khi sử dụng DataFrame đã qua Feature Engineering.

Do đó, project tiếp tục với DataFrame đã được Feature Engineering và Logistic Regression.

---

# 13. Experiment với Oversampling

Do dataset có minority class, project tiếp tục thử nghiệm:

* Không Oversampling
* SMOTE
* Random Over Sampling

Các phương pháp được đặt trực tiếp bên trong `imblearn Pipeline` để quá trình sampling được thực hiện trong từng training fold.

Kết quả:

| Pipeline             | Accuracy Mean | F1 Macro Mean | F1 Macro Std |
| -------------------- | ------------: | ------------: | -----------: |
| SMOTE                |        0.9650 |        0.9625 |       0.0230 |
| Without Oversampling |        0.9650 |        0.9623 |       0.0129 |
| ROS                  |        0.9510 |        0.9467 |       0.0242 |

Trong dataset hiện tại, Oversampling không tạo ra cải thiện đáng kể và làm tăng độ biến động của model qua các fold.

Project vì vậy tiếp tục với pipeline **không Oversampling**.

Một giả thuyết được đặt ra là dataset có kích thước tương đối nhỏ, khiến việc tạo thêm sample bằng SMOTE hoặc ROS có thể tạo noise và làm decision boundary kém ổn định.

---

# 14. Hyperparameter Tuning

Sau Model Comparison, Logistic Regression được đưa vào quá trình Hyperparameter Tuning.

Project sử dụng hai giai đoạn:

```text
RandomizedSearchCV
        ↓
GridSearchCV
```

### RandomizedSearchCV

Tìm kiếm trên:

```python
C:
    loguniform(1e-4, 1e4)

solver:
    ["lbfgs", "saga", "newton-cg"]

class_weight:
    [None, "balanced"]
```

Thiết lập:

```text
n_iter = 100
cv = Stratified 5-Fold
scoring = F1 Macro
```

Kết quả:

```text
Best F1 Macro = 0.9700906
```

Best parameters:

```text
C = 0.099156
solver = "newton-cg"
class_weight = None
```

Sau đó, `GridSearchCV` được sử dụng để tìm kiếm tinh hơn quanh giá trị `C` tốt nhất.

Kết quả cuối cùng:

```text
C = 0.099155
solver = "newton-cg"
class_weight = None
F1 Macro CV = 0.9700906
```

---

# 15. Final Test Evaluation

Sau khi hoàn thành Hyperparameter Tuning, mô hình cuối cùng được đánh giá trên Test Set.

Kết quả:

| Metric    | Macro Average | Weighted Average |
| --------- | ------------: | ---------------: |
| Precision |          0.98 |             0.99 |
| Recall    |          0.99 |             0.99 |
| F1-score  |          0.98 |             0.99 |

Accuracy:

```text
0.99
```

Kết quả cho thấy mô hình duy trì hiệu năng cao trên Test Set và sự chênh lệch giữa Macro và Weighted Average là tương đối nhỏ.

Ngoài các metrics tổng thể, project còn sử dụng:

* Classification Report
* Per-class metrics
* Confusion Matrix
* ROC-AUC
* ROC Curve
* Precision-Recall Curve
* Learning Curve
* Repeated K-Fold evaluation

để đánh giá mô hình ở nhiều khía cạnh khác nhau.

---

# 16. Kiểm tra Overfitting

Do dataset có kích thước nhỏ, project đặc biệt quan tâm đến khả năng Overfitting.

Learning Curve được sử dụng để theo dõi:

```text
Training Score
Validation Score
```

khi số lượng sample training tăng dần.

Kết quả cho thấy validation score có xu hướng hội tụ với training score khi số lượng sample tăng lên, đồng thời độ biến động của F1 Score giảm.

Project tiếp tục kiểm tra bằng Repeated K-Fold để quan sát tính ổn định khi quá trình chia fold được lặp lại với các lần shuffle khác nhau.

---

# 17. Model Interpretability với SHAP

Sau khi xây dựng model cuối cùng, project sử dụng **SHAP** để giải thích cách model đưa ra prediction.

Phân tích gồm hai cấp độ:

### Global Explainability

Xác định những feature có ảnh hưởng lớn đến prediction của từng class.

### Local Explainability

Phân tích đóng góp của từng feature đối với một sample cụ thể.

Một điểm quan trọng từ SHAP là **không có một tập feature duy nhất quyết định toàn bộ 6 class**. Importance của feature thay đổi theo từng class.

---

# 18. SHAP Key Findings

Một số feature nổi bật theo từng class:

| Class   | Một số feature nổi bật                                                                                           |
| ------- | ---------------------------------------------------------------------------------------------------------------- |
| Class 1 | `thinning_suprapapillary_epidermis`, `clubbing_rete_ridges`, `spongiosis`, `exocytosis_group_score`              |
| Class 2 | `spongiosis`, `PNL_infiltrate`, `koebner_phenomenon`, `eosinophils_infiltrate`                                   |
| Class 3 | `PNL_infiltrate` và các pattern liên quan đến feature value thấp                                                 |
| Class 4 | `koebner_phenomenon`, `itching`, `PNL_infiltrate`, `fibrosis_papillary_dermis`                                   |
| Class 5 | `fibrosis_papillary_dermis` và một số feature được tạo bởi Feature Engineering                                   |
| Class 6 | `follicular_papules`, `follicular_pap_group_scores`, `perifollicular_parakeratosis`, `follicular_pap_high_count` |

Một pattern đáng chú ý là **histopathological features xuất hiện rất nhiều trong nhóm feature quan trọng** của các class.

Điều này cũng tương đối nhất quán với những signal đã được quan sát trong EDA.

---

# 19. Executive Summary

Project giải quyết bài toán phân loại 6 nhóm bệnh erythemato-squamous dựa trên clinical features và histopathological features.

Quy trình chính:

```text
Data Cleaning
→ EDA
→ Feature Engineering
→ Pipeline
→ 5-Fold Cross-Validation
→ Model Comparison
→ Oversampling Experiment
→ Hyperparameter Tuning
→ Final Test Evaluation
→ SHAP
```

Mô hình cuối cùng:

```text
Logistic Regression
```

Hyperparameters:

```text
C = 0.099155
solver = "newton-cg"
class_weight = None
```

Cross-Validation:

```text
F1 Macro = 0.9701
```

Test Set:

```text
Accuracy = 0.99

Precision Macro   = 0.98
Recall Macro      = 0.99
F1 Macro          = 0.98

Precision Weighted = 0.99
Recall Weighted    = 0.99
F1 Weighted        = 0.99
```

---

# 20. Medical / Clinical Considerations

Kết quả của project có thể được định hướng như một **Clinical Decision Support Tool**, hỗ trợ quá trình phân loại ban đầu.

Tuy nhiên, prediction của model không nên được sử dụng như một chẩn đoán độc lập.

Trong thực tế, quyết định lâm sàng cần kết hợp:

```text
Clinical Examination
        +
Patient History
        +
Professional Medical Assessment
        +
Histopathological Examination khi cần thiết
```

Đặc biệt, do dataset chỉ có 366 samples và bài toán liên quan đến chẩn đoán y khoa, kết quả Machine Learning cần được kiểm chứng trên dữ liệu lớn hơn và dữ liệu thực tế trước khi có thể xem xét khả năng ứng dụng lâm sàng.

---

# 21. Công nghệ sử dụng

### Programming

* Python
* Jupyter Notebook

### Data Processing

* NumPy
* Pandas

### Visualization

* Matplotlib
* Seaborn

### Machine Learning

* Scikit-learn
* Imbalanced-learn

### Model Interpretability

* SHAP

### Statistical / Model Evaluation

* Stratified K-Fold Cross-Validation
* Repeated K-Fold
* Classification Report
* Confusion Matrix
* ROC-AUC
* Precision-Recall
* Learning Curve

---

# 22. Cấu trúc Project

```text
Portfolio Project #3/
│
├── Portfolio Project#3.ipynb
├── Data Dictionary.md
└── README.md
```

`Portfolio Project#3.ipynb` chứa toàn bộ quá trình phân tích và xây dựng mô hình.

`Data Dictionary.md` chứa thông tin chi tiết hơn về ý nghĩa của các feature trong dataset.

---

# 23. Những kiến thức thể hiện qua Project

Project tập trung thể hiện các kỹ năng:

* Problem Understanding
* Data Exploration
* Data Cleaning
* Exploratory Data Analysis
* Correlation Analysis
* Feature Engineering
* Train-Test Split
* Scikit-learn Pipeline
* ColumnTransformer
* Stratified Cross-Validation
* Multi-class Classification
* Macro / Weighted Metrics
* Model Comparison
* Class Imbalance Handling
* SMOTE / Random Over Sampling
* Hyperparameter Tuning
* RandomizedSearchCV
* GridSearchCV
* Learning Curve
* Repeated K-Fold
* SHAP
* Global Explainability
* Local Explainability
* Model Evaluation
* Medical Machine Learning Considerations

---

# 24. Kết luận

Project không chỉ tập trung vào việc tìm ra một model có score cao mà xây dựng một quy trình Machine Learning tương đối đầy đủ:

```text
Hiểu bài toán
    ↓
Hiểu dữ liệu
    ↓
Tìm signal bằng EDA
    ↓
Đặt giả thuyết Feature Engineering
    ↓
Kiểm chứng feature bằng Cross-Validation
    ↓
So sánh nhiều model
    ↓
Kiểm tra class imbalance
    ↓
Tối ưu Hyperparameter
    ↓
Đánh giá trên Test Set
    ↓
Giải thích model bằng SHAP
```

Kết quả cuối cùng cho thấy **Logistic Regression** với các feature được Feature Engineering đạt `F1 Macro = 0.9701` trên Cross-Validation và `Accuracy = 0.99` trên Test Set.

Quan trọng hơn, SHAP cho thấy các **histopathological features** đóng vai trò nổi bật trong việc phân biệt các class, đồng thời một số feature được tạo ra từ Feature Engineering cũng cung cấp thêm signal cho mô hình.
