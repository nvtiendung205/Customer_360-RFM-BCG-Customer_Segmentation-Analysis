# 📁 Data Dictionary & Guidelines

Tài liệu hướng dẫn và mô tả chi tiết tập dữ liệu sử dụng trong dự án **Customer 360 (RFM & BCG Matrix)**.

---

## 1. Cấu trúc thư mục dữ liệu

```text
data/
├── sample/
│   ├── sample_customer_registered.csv   # 1,000 dòng mẫu thông tin khách hàng
│   └── sample_customer_transaction.csv  # 1,000 dòng mẫu lịch sử giao dịch
└── README.md                            # Mô tả chi tiết dữ liệu
```

> **Lưu ý về Full Dataset (~70MB / 1M+ rows):**  
> Vì lý do tối ưu hóa dung lượng repository Git, tập dữ liệu thô đầy đủ gồm hơn 1 triệu dòng được khuyến nghị lưu trữ trên Google Drive / Cloud Storage hoặc Git LFS. Trong repository này đã đính kèm dữ liệu mẫu (`data/sample/`) để kiểm thử logic SQL và Power BI.

---

## 2. Từ điển dữ liệu (Data Dictionary)

### Bảng 1: `Customer_Registered` (Thông tin khách hàng)
Bảng chứa dữ liệu định danh và trạng thái hợp đồng của khách hàng.

| Tên cột | Kiểu dữ liệu | Ý nghĩa | Ví dụ |
|---|---|---|---|
| `ID` | INT | Mã định danh duy nhất của khách hàng (`Primary Key`) | `1`, `2`, `1327813` |
| `Contract` | VARCHAR | Mã số hợp đồng đăng ký | `SGDN00215` |
| `LocationID` | INT | Mã định danh khu vực / địa lý | `8` |
| `BranchCode` | INT | Mã chi nhánh thực hiện đăng ký | `1` |
| `Status` | INT | Trạng thái hợp đồng (0: Đang hoạt động, 2: Đã dừng...) | `0`, `2` |
| `created_date` | DATE (MM/DD/YYYY) | Ngày ký hợp đồng / đăng ký dịch vụ | `11/25/2011` |
| `stopdate` | DATE (MM/DD/YYYY) | Ngày chấm dứt dịch vụ (nếu có) | `1/5/2012` |

---

### Bảng 2: `Customer_Transaction` (Lịch sử giao dịch)
Bảng ghi nhận toàn bộ hoạt động mua sắm, phát sinh doanh thu từ khách hàng.

| Tên cột | Kiểu dữ liệu | Ý nghĩa | Ví dụ |
|---|---|---|---|
| `Transaction_ID` | BIGINT | Mã giao dịch (`Primary Key`) | `0`, `1`, `2` |
| `CustomerID` | INT | Mã khách hàng (`Foreign Key` liên kết `Customer_Registered.ID`) | `1327813` |
| `Purchase_Date` | DATE (MM/DD/YYYY) | Thời điểm phát sinh giao dịch | `6/1/2022` |
| `GMV` | DECIMAL / BIGINT | Tổng giá trị giao dịch (Gross Merchandise Value - VNĐ) | `95,000` |

---

## 3. Liên kết thực thể (Data Relationship)
Mô hình dữ liệu quan hệ 1 - Nhiều (1:N):
- `Customer_Registered.ID` (1) ───< (N) `Customer_Transaction.CustomerID`

