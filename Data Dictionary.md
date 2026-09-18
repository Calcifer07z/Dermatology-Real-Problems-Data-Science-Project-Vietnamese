## 1. Clinical Attributes

> **Giá trị:** 0–3, trừ `family_history` (0/1) và `age` (giá trị liên tục).

| ID | Variable | Ý nghĩa |
|---:|---|---|
| 1 | `erythema` | **Ban đỏ** – mức độ da bị đỏ do giãn mạch/tình trạng viêm. |
| 2 | `scaling` | **Bong vảy** – mức độ hình thành và bong các lớp vảy trên bề mặt da. |
| 3 | `definite_borders` | **Ranh giới tổn thương rõ** – mức độ vùng da tổn thương có ranh giới rõ ràng với vùng da lành. |
| 4 | `itching` | **Ngứa** – mức độ cảm giác ngứa của bệnh nhân. |
| 5 | `koebner_phenomenon` | **Hiện tượng Koebner** – tổn thương da xuất hiện tại vị trí bị chấn thương/cọ xát. |
| 6 | `polygonal_papules` | **Sẩn đa giác** – sự hiện diện/mức độ của các sẩn có hình dạng đa giác. |
| 7 | `follicular_papules` | **Sẩn nang lông** – sự hiện diện/mức độ của các sẩn liên quan đến nang lông. |
| 8 | `oral_mucosal_involvement` | **Tổn thương niêm mạc miệng** – mức độ niêm mạc miệng bị ảnh hưởng bởi bệnh. |
| 9 | `knee_and_elbow_involvement` | **Tổn thương vùng đầu gối và khuỷu tay** – mức độ bệnh ảnh hưởng đến các vùng này. |
| 10 | `scalp_involvement` | **Tổn thương da đầu** – mức độ bệnh ảnh hưởng đến da đầu. |
| 11 | `family_history` | **Tiền sử gia đình** – `1` nếu trong gia đình từng có người mắc một trong các bệnh thuộc nhóm này; `0` nếu không. |
| 34 | `age` | **Tuổi** – tuổi của bệnh nhân, là biến số liên tục. |

## 2. Histopathological Attributes

> **Giá trị:** 0–3. Giá trị càng cao biểu thị đặc trưng mô bệnh học càng rõ/nhiều.

| ID | Variable | Ý nghĩa |
|---:|---|---|
| 12 | `melanin_incontinence` | **Sự mất kiểm soát melanin** – melanin thoát khỏi lớp biểu bì và hiện diện trong lớp bì, thường liên quan đến tổn thương lớp đáy. |
| 13 | `eosinophils_in_the_infiltrate` | **Bạch cầu ái toan trong thâm nhiễm** – sự hiện diện của eosinophil trong vùng tế bào viêm thâm nhiễm. |
| 14 | `PNL_infiltrate` | **Thâm nhiễm bạch cầu đa nhân (PNL)** – sự hiện diện của các tế bào bạch cầu đa nhân trong mô. |
| 15 | `fibrosis_of_the_papillary_dermis` | **Xơ hóa lớp bì nhú** – sự hình thành mô xơ tại lớp bì nhú. |
| 16 | `exocytosis` | **Exocytosis** – tế bào viêm di chuyển từ mô liên kết vào lớp biểu bì. |
| 17 | `acanthosis` | **Acanthosis (dày lớp gai)** – sự tăng độ dày của lớp tế bào gai của biểu bì. |
| 18 | `hyperkeratosis` | **Tăng sừng** – lớp sừng của biểu bì dày lên bất thường. |
| 19 | `parakeratosis` | **Tăng sừng á sừng** – vẫn còn nhân tế bào trong lớp sừng, phản ánh quá trình sừng hóa bất thường. |
| 20 | `clubbing_of_the_rete_ridges` | **Phì đại dạng dùi trống của các mào biểu bì (rete ridges)** – các mào biểu bì trở nên to, rộng và có dạng đầu tròn. |
| 21 | `elongation_of_the_rete_ridges` | **Kéo dài các mào biểu bì** – các mào biểu bì kéo dài xuống phía lớp bì. |
| 22 | `thinning_of_the_suprapapillary_epidermis` | **Mỏng biểu bì trên nhú bì** – phần biểu bì nằm phía trên các nhú bì bị mỏng đi. |
| 23 | `spongiform_pustule` | **Mụn mủ dạng xốp (spongiform pustule)** – ổ mụn mủ trong biểu bì hình thành do bạch cầu đa nhân tập trung giữa các tế bào biểu bì. |
| 24 | `munro_microabcess` | **Vi áp-xe Munro** – tập hợp bạch cầu đa nhân trong lớp sừng, đặc trưng thường gặp trong bệnh vảy nến. |
| 25 | `focal_hypergranulosis` | **Tăng hạt khu trú** – sự dày lên khu trú của lớp hạt trong biểu bì. |
| 26 | `disappearance_of_the_granular_layer` | **Mất lớp hạt** – lớp hạt của biểu bì biến mất hoặc giảm rõ rệt. |
| 27 | `vacuolisation_and_damage_of_basal_layer` | **Không bào hóa và tổn thương lớp đáy** – các tế bào ở lớp đáy xuất hiện không bào và bị tổn thương. |
| 28 | `spongiosis` | **Spongiosis (phù xốp biểu bì)** – dịch tích tụ giữa các tế bào biểu bì làm chúng tách xa nhau. |
| 29 | `saw_tooth_appearance_of_retes` | **Hình ảnh răng cưa của các mào biểu bì** – các mào biểu bì có hình dạng giống răng cưa. |
| 30 | `follicular_horn_plug` | **Nút sừng nang lông** – chất sừng tích tụ và bít trong nang lông. |
| 31 | `perifollicular_parakeratosis` | **Á sừng quanh nang lông** – hiện tượng parakeratosis xảy ra quanh nang lông. |
| 32 | `inflammatory_mononuclear_infiltrate` | **Thâm nhiễm tế bào viêm đơn nhân** – sự tập trung của các tế bào viêm đơn nhân như lymphocyte và monocyte trong mô. |
| 33 | `band_like_infiltrate` | **Thâm nhiễm dạng dải** – tế bào viêm tập trung thành một dải, thường nằm dọc theo vùng tiếp giáp biểu bì–bì. |

## 3. Quy ước giá trị

| Giá trị | Ý nghĩa |
|---:|---|
| `0` | Không có đặc trưng |
| `1` | Mức độ nhẹ |
| `2` | Mức độ trung gian |
| `3` | Mức độ lớn/rõ nhất |

**Lưu ý:** `family_history` chỉ nhận `0/1`; `age` là biến liên tục biểu thị tuổi bệnh nhân.
