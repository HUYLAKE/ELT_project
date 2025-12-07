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

models/
│
├── staging/ ← chuyển đổi nhẹ từ raw
└── marts/ ← mô hình hóa dữ liệu phân tích

## 🧰 2. dbt Project Structure

Bên trong thư mục `models/` của dbt, dự án được chia rõ thành:

### 📁 **staging/**
- Chứa các mô hình `stg_*`
- Dùng để lấy dữ liệu từ database raw và materialize dưới view trong database analytics
- Các bảng này **đọc trực tiếp từ database raw**


stg_order model như sau:
```sql
select
    id as order_id,
    user_id as customer_id,
    order_date,
    status

from {{source('raw', 'orders')}}
```
strg_customer model như sau:
```sql
select
    id as customer_id,
    first_name,
    last_name

from {{source('raw', 'customers')}}
```
source.schema.yml chứa file dạng yaml về database raw nhằm giúp các stg model ghi tên tiện lợi hơn
```yaml
version: 2

sources:
  - name: raw
    database: raw
    schema: raw
    tables:
      - name: customers
      - name: orders
```

### 📁 **marts/**
- Chứa model được làm sạch và tính dựa trên 2 staging model


 dim_customer model như sau
```sql
with customers as (

     select * from {{ ref('stg_customers') }}

),

orders as ( 

    select * from {{ ref('stg_orders') }}

),

customer_orders as (

    select
        customer_id,

        min(order_date) as first_order_date,
        max(order_date) as most_recent_order_date,
        count(order_id) as number_of_orders

    from orders

    group by 1

),

final as (

    select
        customers.customer_id,
        customers.first_name,
        customers.last_name,
        customer_orders.first_order_date,
        customer_orders.most_recent_order_date,
        coalesce (customer_orders.number_of_orders, 0) 
        as number_of_orders

    from customers

    left join customer_orders using (customer_id)

)

select * from final
```
  


