# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600080
**Name:** Hồ Trần Đình Nguyên
**Date:** 2026-04-15

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200. | 9 | Dữ liệu sau ETL chỉ còn 3 bản ghi hợp lệ, `category` được chuẩn hóa và không có giá trị lỗi. Agent chọn đúng sản phẩm điện tử có giá cao nhất. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Agent bị dẫn hướng bởi giá trị ngoại lai quá lớn. Bộ dữ liệu cũng có ID trùng, giá ở dạng chữ và giá trị null làm giảm độ tin cậy. |

---

## 2. Phan tich & nhận xét

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Agent trả lời sai vì garbage data làm hỏng ngữ cảnh mà agent dùng để suy luận. Trong file `garbage_data.csv`, có duplicate ID `1` cho hai sản phẩm khác nhau nên tính duy nhất của bản ghi không còn được đảm bảo. Trường `price` của `Broken Chair` là chữ `ten dollars` thay vì số, cho thấy wrong data type và có thể gây lỗi khi xử lý thống kê hoặc sắp xếp. Bản ghi `Nuclear Reactor` có giá `999999`, là một outlier rất lớn, nên logic "chọn món electronics giá cao nhất" bị đánh lừa và đưa ra kết quả vô lý. Ngoài ra, bản ghi cuối có `id` rỗng, `price = 0` và `category` null, cho thấy dữ liệu thiếu và không hợp lệ. Khi dữ liệu đầu vào không được validate, agent vẫn có thể trả lời rất tự tin, nhưng câu trả lời dựa trên nền tảng dữ liệu sai lệch. Điều này chứng minh rằng chất lượng dữ liệu ảnh hưởng trực tiếp đến độ chính xác của hệ thống AI, kể cả khi prompt không đổi.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** Đồng ý.

Trong bài thí nghiệm này, prompt giữ nguyên ở cả hai trường hợp, nhưng kết quả thay đổi rất lớn chỉ vì chất lượng dữ liệu đầu vào. Prompt tốt chỉ giúp agent đặt câu hỏi đúng cách, còn dữ liệu sạch mới giúp agent suy luận đúng. Vì vậy, quality data có tác động nền tảng và quan trọng hơn quality prompt trong nhiều bài toán AI ứng dụng.
