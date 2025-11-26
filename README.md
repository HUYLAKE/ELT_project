# ELT_mock projet
![images](ELT_datapipeline.png)

Đây là 1 mock project về một data pipeline cùng công cụ dbt và CSDL MySQL

Các bước của dự án

- trong MySQL sẽ có 2 database là raw và analytics
+ database raw dùng để chứa data từ file csv nguyên dạng không chỉnh sửa
+ database analytics dùng để nhận dữ liệu đã làm sạch từ database raw
- sử dụng dbt để biến đổi/làm sạch dữ liệu
+ trong models sẽ có 2 folder con gồm staging và marts
+ folder staging chứ các model từ các bảng chứa dữ liệu thô từ database raw cùng một chút biến đổi
+ folder marts chứ model đã biến đổi dữ và tính toán dữ liệu lấy từ các model staging bằng phương pháp jinja ref của dbt
  


