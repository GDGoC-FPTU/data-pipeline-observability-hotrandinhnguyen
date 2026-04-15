[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23572397&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student ID:** AI20K-0080
**Student Email:** de180372hotrandinhnguyen@gmail.com
**Name:** Hồ Trần Đình Nguyên

---

## Mô tả

Trong bài lab này, tôi xây dựng một ETL pipeline đơn giản bằng Python và Pandas để đọc dữ liệu từ `raw_data.json`, kiểm tra tính hợp lệ của từng record, chuẩn hóa dữ liệu, sau đó lưu kết quả ra `processed_data.csv`. Pipeline loại bỏ các record có `price <= 0` hoặc `category` rỗng, tạo cột `discounted_price` giảm 10%, chuyển `category` sang Title Case và thêm cột `processed_at` để tăng tính observability.

Ngoài pipeline, tôi thực hiện stress test với agent mô phỏng trong `agent_simulation.py` bằng hai bộ dữ liệu: dữ liệu sạch sau ETL và dữ liệu rác trong `garbage_data.csv`. Kết quả cho thấy khi dữ liệu đầu vào có outlier, duplicate ID, sai kiểu dữ liệu và giá trị thiếu, agent vẫn trả lời được nhưng kết quả lại sai lệch và kém tin cậy.

---

## Cách chạy (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chạy ETL Pipeline
```bash
python solution.py
```

### Chạy Agent Simulation (Stress Test)
```bash
python agent_simulation.py
```

Lệnh trên sẽ so sánh cùng một truy vấn trên:
- `processed_data.csv` để đại diện cho clean data
- `garbage_data.csv` để đại diện cho garbage data

---

## Cấu trúc thư mục

```text
|-- solution.py              # ETL Pipeline script
|-- raw_data.json            # Dữ liệu đầu vào
|-- processed_data.csv       # Output sau khi làm sạch và biến đổi
|-- garbage_data.csv         # Dữ liệu rác dùng cho stress test
|-- agent_simulation.py      # Mô phỏng agent đọc dữ liệu
|-- experiment_report.md     # Báo cáo thí nghiệm
`-- README.md                # File mô tả bài lab
```

---

## Kết quả

- Tổng số record đầu vào: 5
- Số record hợp lệ sau validation: 3
- Số record bị loại: 2
- Lý do bị loại: 1 record có `price <= 0`, 1 record có `category` rỗng
- Kết quả clean data: Agent chọn `Laptop` là sản phẩm electronics tốt nhất với giá `$1200`
- Kết quả garbage data: Agent chọn sai `Nuclear Reactor` với giá `$999999` do bị ảnh hưởng bởi outlier và dữ liệu không hợp lệ

## Nhận xét

Bài lab cho thấy observability không chỉ là ghi log hay thêm timestamp, mà còn là khả năng nhìn thấy chất lượng dữ liệu đang ảnh hưởng đến hệ thống ra sao. Khi pipeline có bước validation rõ ràng, kết quả agent ổn định hơn và dễ giải thích hơn. Nếu bỏ qua data quality, model hoặc agent có thể đưa ra câu trả lời sai nhưng vẫn rất tự tin, gây rủi ro cho các bài toán thực tế.
