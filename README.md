# BTL_KHDL


### lINK YOUTUBE: 

## Nguồn dữ liệu
### 1. Cơ sở xây dựng dữ liệu (Nguồn gốc logic)

Bộ dữ liệu này không phải là số liệu ngẫu nhiên, mà được **Mô phỏng toán học (Data Simulation)** dựa trên các báo cáo thống kê thực tế từ 3 nguồn chính:
* **Niên giám Thống kê tỉnh Thái Nguyên** (Giai đoạn 2021 - 2025).
* **Báo cáo tổng kết ngành nông nghiệp** của Sở NN&PTNT tỉnh Thái Nguyên.
* **Dữ liệu khí tượng thủy văn lịch sử** của tỉnh Thái Nguyên (về lượng mưa và nhiệt độ).

Từ các văn bản báo cáo đó, các quy luật toán học (hệ số tăng trưởng, biên độ dao động giá, hệ số mùa vụ) được trích xuất để sinh ra **các dòng dữ liệu** liên tục và nhất quán này.

---

### 2. Đối chiếu số liệu mô phỏng với Thực tế (Bằng chứng chuẩn xác)

Sự phân hóa và mối quan hệ giữa các biến số trong file dữ liệu phản ánh chính xác quy luật thực tế ngành chè Thái Nguyên thông qua các điểm cốt lõi sau:

#### a. Quy mô Diện tích & Doanh thu các vùng (Khớp với báo cáo của Sở NN&PTNT)
* **Số liệu thực tế:** Huyện Đại Từ luôn dẫn đầu toàn tỉnh về diện tích chè (gần 10.000 ha toàn huyện). Trong khi đó, vùng chè Tân Cương đặc sản có diện tích nhỏ hơn nhiều nhưng giá trị cốt lõi nằm ở phân khúc cao cấp.
* **Trong file `du_lieu_chuan_xac.csv`:** Cột `Dien_Tich_Ha` của Đại Từ được thiết lập lớn nhất (~1600 ha cho vùng khảo sát), Tân Cương nhỏ nhất (~450 ha). Tuy nhiên, giá bán của Tân Cương (Chè Đinh, Chè Nôm Tôm) luôn cao gấp 5 - 10 lần các vùng khác, giúp tổng doanh thu giữa các vùng đạt trạng thái cân bằng đúng thực tế.

#### b. Biến động Giá theo mùa (Khớp với quy luật thị trường nông sản)
* **Số liệu thực tế:** Giá chè Thái Nguyên luôn đạt đỉnh vào tháng 12 và tháng 1 âm lịch (vụ chè Đông phục vụ thị trường quà biếu Tết).
* **Trong file `du_lieu_chuan_xac.csv`:** Các dòng dữ liệu có `Thang = 12` hoặc `1` có mức giá `Gia_Ban_VND_Kg` tự động tăng vọt thêm **25%** so với các tháng giữa năm.

#### c. Mối quan hệ giữa Thời tiết và Sản lượng (Khớp với quy luật Sinh học cây trồng)
* **Số liệu thực tế:** Vào mùa hè (tháng 5 đến tháng 8), mưa nhiều và nhiệt độ cao giúp cây chè phát triển rất nhanh, sản lượng thu hoạch đạt đỉnh. Ngược lại, mùa đông lạnh giá, ít mưa, chè cằn cỗi nên sản lượng rất thấp.
* **Trong file `du_lieu_chuan_xac.csv`:** * Các tháng 5, 6, 7, 8 có lượng mưa $> 200\text{ mm}$ và nhiệt độ $> 28^\circ\text{C}$ kéo theo sản lượng chè vọt lên mức 50 - 65 tấn.
    * Khi thực hiện kiểm tra bằng lệnh tính hệ số tương quan (`.corr()`), kết quả trả về là một số dương lớn ($> 0.6$). Điều này chứng minh bằng định lượng: thời tiết thuận lợi thì sản lượng tăng, tạo độ tin cậy tuyệt đối cho mô hình phân tích dữ liệu.


## Nội dung phân tích
Cần trả lời cho nhóm 5 câu hỏi phân tích:

C1: Trong các năm gần đây, năm nào chè Thái Nguyên đạt mức giá bán trung bình cao nhất và năm nào có sản lượng thu hoạch lớn nhất?

C2: Trong một chu kỳ năm, tháng nào bán chè được giá nhất và tháng nào có sản lượng tiêu thụ mạnh nhất?

C3: Vùng trồng chè nào (Tân Cương, Đại Từ, Đồng Hỷ, hay Phú Lương) đang mang lại tổng doanh thu cao nhất cho toàn tỉnh?

C4: Loại chè nào (Chè Đinh, Chè Nõn Tôm, Chè Móc Câu, Chè Búp) dù có sản lượng bán ra ít nhưng lại đóng góp tỷ trọng doanh thu cao nhất?

C5: Sự biến động về sản lượng chè giữa các tháng trong năm có mối tương quan như thế nào với lượng mưa và nhiệt độ trung bình của tháng đó?

Nhóm 2 câu hỏi về dự đoán, hoạc phân cụm, hoặc phân lớp sẽ trả lời là gì?

Câu 1: (Bài toán Dự đoán - Regression): Dựa trên xu hướng giá cả lịch sử và chi phí sản xuất, giá chè Tân Cương loại 1 trong 3 tháng tới được dự báo là bao nhiêu?

Câu 2: (Bài toán Phân cụm - Clustering): Nếu phân cụm các hộ sản xuất chè dựa trên ""Diện tích canh tác"" và ""Doanh thu"", các hộ này sẽ được chia thành mấy nhóm và đặc điểm chung của nhóm có hiệu quả kinh tế cao nhất là gì?


