# ELT_mock projet
![images](ELT_datapipeline.png)

# 🛠️ Data Pipeline Project — dbt + MySQL

Dự án này mô phỏng một quy trình **Data Pipeline** sử dụng **MySQL** làm hệ quản trị cơ sở dữ liệu và **dbt** làm công cụ chuyển đổi dữ liệu (T → trong ELT).  
Mục tiêu là tái hiện kiến trúc hiện đại của các doanh nghiệp khi xây dựng data warehouse nhỏ, tách biệt dữ liệu thô và dữ liệu đã được xử lý.

---

## 📐 Kiến Trúc Tổng Quan

Pipeline bao gồm 2 tầng dữ liệu chính trong MySQL:

- **raw** — lưu trữ dữ liệu gốc lấy trực tiếp từ file CSV  
- **analytics** — lưu trữ dữ liệu đã được làm sạch / biến đổi từ dbt

Toàn bộ thao tác transform được thực hiện bằng **dbt**, với cấu trúc chuẩn gồm:

