# MINI PROJECT – Phân cụm khách hàng dựa trên luật kết hợp

## 1. Giới thiệu
Mini Project này tập trung vào bài toán phân khúc khách hàng dựa trên **luật kết hợp (Association Rules)** thay vì chỉ sử dụng các đặc trưng truyền thống như RFM.  
Ý tưởng chính là biến các luật kết hợp thành **đặc trưng hành vi mua kèm**, sau đó áp dụng thuật toán phân cụm để khám phá các nhóm khách hàng có hành vi tương đồng và đề xuất chiến lược marketing phù hợp.

Dữ liệu giao dịch không có nhãn, vì vậy bài toán thuộc nhóm **Unsupervised Learning**.

---

## 2. Pipeline thực hiện
Luật kết hợp  
→ Trích xuất đặc trưng hành vi mua kèm  
→ Phân cụm khách hàng  
→ Diễn giải cụm  
→ Đề xuất chiến lược marketing

---

## 3. Luật kết hợp (Association Rules)
- Thuật toán: **Apriori** (tái sử dụng kết quả từ Lab 1)
- File luật đầu vào:
data/processed/rules_apriori_filtered.csv
Chiến lược chọn luật:
- Số lượng luật: **Top 200**
- Tiêu chí sắp xếp: **Lift**
- Lý do: Lift phản ánh mức độ liên kết thực sự giữa các sản phẩm, giúp phân biệt rõ hành vi mua kèm.

## 4. Feature Engineering
Mỗi luật kết hợp có dạng: Antecedent → Consequent
Với mỗi khách hàng:
- Nếu khách hàng đã từng mua đủ các sản phẩm trong antecedent của luật → feature = 1
- Ngược lại → feature = 0

### Các biến thể đặc trưng
- **Baseline**: Rule-only, binary (0/1)
- **Biến thể nâng cao (sử dụng trong mô hình cuối)**:
  - Feature theo luật kết hợp
  - Weighting theo **Lift**
  - Ghép thêm RFM (Recency, Frequency, Monetary)
  - RFM được scale trước khi phân cụm
Kích thước vector đặc trưng cuối cùng:
3920 khách hàng × 203 đặc trưng
## 5. Phân cụm khách hàng
- Thuật toán: **K-Means**
- Chọn số cụm K bằng **Silhouette score**
- Khảo sát K từ 2 đến 10
- K tối ưu được chọn: **K = 2**

---

## 6. Trực quan hóa
- Giảm chiều bằng **PCA**
- Vẽ scatter plot 2D, tô màu theo cluster
- Kết quả cho thấy các cụm có sự tách biệt tương đối, phù hợp cho phân khúc marketing.

---
## 7. Profiling & chiến lược marketing
Kết quả phân cụm được lưu tại: data/processed/customer_clusters_from_rules.csv
### Cluster 0 – Occasional Low-Value Customers
- Mua không thường xuyên, giá trị thấp  
- Chiến lược:
  - Bundle sản phẩm giá rẻ
  - Coupon kích hoạt mua lại
  - Chiến dịch reactivation

### Cluster 1 – Frequent High-Value Customers
- Mua thường xuyên, chi tiêu cao  
- Chiến lược:
  - Cross-sell & upsell theo luật mạnh
  - Chăm sóc VIP
  - Ưu đãi cá nhân hóa
 

## 8. Cấu trúc thư mục
shop_cluster/
├── data/
│ └── processed/
│ ├── cleaned_uk_data.csv
│ ├── rules_apriori_filtered.csv
│ └── customer_clusters_from_rules.csv
├── notebooks/
│ └── clustering_from_rules.ipynb
├── src/
│ └── cluster_library.py
├── requirements.txt
└── README.md
