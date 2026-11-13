# **BÀI TẬP: SO SÁNH TRADE-OFF CÁC GIẢI PHÁP VIẾT PROMPT**

## **Bước 1\. Đề xuất 2 Prompt mẫu**

### **Giải pháp 1: Prompt đơn giản, ngắn gọn**

Bạn là nhân viên xử lý đơn hàng của cửa hàng.

Hãy tiếp nhận thông tin đơn hàng, kiểm tra dữ liệu và tính tổng tiền.

Ví dụ:

* Sản phẩm: Chuột máy tính  
* Số lượng: 2  
* Đơn giá: 150.000 VNĐ  
* Kết quả: Tổng tiền 300.000 VNĐ.

Nếu thiếu dữ liệu, hãy ghi rõ thông tin còn thiếu.

Xuất kết quả dưới dạng JSON và bảng Table, gồm: Mã đơn, tên khách hàng, sản phẩm, số lượng, tổng tiền và trạng thái đơn hàng.

### **Giải pháp 2: Prompt chi tiết, có nhiều ràng buộc**

**VAI TRÒ (ROLE):**

Bạn là chuyên viên quản lý và xử lý đơn hàng cho cửa hàng bán lẻ.

**NHIỆM VỤ (TASK):**

Tiếp nhận, kiểm tra, tính toán và phân loại thông tin đơn hàng.

**DỮ LIỆU ĐẦU VÀO (INPUT):**

* Mã đơn hàng: DH001  
* Tên khách hàng: Nguyễn Văn An  
* Sản phẩm: Tai nghe Bluetooth  
* Số lượng: 2  
* Đơn giá: 350.000 VNĐ  
* Số điện thoại: 0901234567

**VÍ DỤ (FEW-SHOT):**

Input:

* Sản phẩm: Chuột máy tính  
* Số lượng: 2  
* Đơn giá: 100.000 VNĐ

Output:

* Tổng tiền: 200.000 VNĐ  
* Trạng thái: Hợp lệ.

**QUY TẮC XỬ LÝ (CONSTRAINTS):**

1. Tính tổng tiền bằng số lượng nhân đơn giá.  
2. Nếu thiếu dữ liệu, ghi nhận là null và nêu rõ lỗi.  
3. Nếu số lượng hoặc đơn giá nhỏ hơn hoặc bằng 0, đánh dấu cần kiểm tra.  
4. Không tự ý bổ sung hoặc thay đổi thông tin.  
5. Nếu dữ liệu không hợp lệ, không tự động xác nhận đơn hàng.  
6. Kiểm tra kết quả trước khi xuất.

**ĐỊNH DẠNG ĐẦU RA (FORMAT):**

Xuất kết quả theo hai dạng:

JSON:  
{  
"ma\_don\_hang": "",  
"ten\_khach\_hang": "",  
"san\_pham": "",  
"so\_luong": 0,  
"don\_gia": 0,  
"tong\_tien": 0,  
"trang\_thai": "",  
"loi\_du\_lieu": \[\]  
}

Table:  
Mã đơn \| Khách hàng \| Sản phẩm \| Số lượng \| Tổng tiền \| Trạng thái

Chỉ trả về dữ liệu theo yêu cầu, không tự suy đoán thông tin còn thiếu.

## **Bước 2\. Bảng so sánh Trade-off**

| Tiêu chí | Giải pháp 1 (Đơn giản) | Giải pháp 2 (Chi tiết) |
| ----- | ----- | ----- |
| Độ dễ hiểu | Cao | Trung bình |
| Tốn Token | Ít | Nhiều hơn |
| Thời gian viết Prompt | Nhanh | Lâu hơn |
| Độ chính xác | Khá tốt | Cao hơn |
| Chặn lỗi biên | Trung bình | Rất tốt |
| Khả năng xử lý dữ liệu thiếu | Cơ bản | Chi tiết |
| Khả năng tái sử dụng | Tốt | Tốt |
| Phù hợp | Đơn hàng đơn giản | Đơn hàng thực tế, nhiều trường hợp lỗi |

## **Bước 3\. Lựa chọn và chạy thử**

### **1\. Lựa chọn giải pháp**

Chọn Giải pháp 2 (Prompt chi tiết).

Lý do:

* Có đầy đủ cấu trúc Prompt tiêu chuẩn.  
* Sử dụng Few-shot để AI hiểu cách xử lý thông qua ví dụ.  
* Có điều kiện kiểm tra dữ liệu thiếu và sai.  
* Ép định dạng JSON/Table giúp kết quả rõ ràng, dễ quản lý.  
* Hạn chế việc AI tự suy đoán thông tin.

### **2\. Chạy thử Prompt**

Dữ liệu đầu vào:

* Mã đơn: DH001  
* Khách hàng: Nguyễn Văn An  
* Sản phẩm: Tai nghe Bluetooth  
* Số lượng: 2  
* Đơn giá: 350.000 VNĐ

Kết quả JSON:

```
{
  "ma_don_hang": "DH001",
  "ten_khach_hang": "Nguyễn Văn An",
  "san_pham": "Tai nghe Bluetooth",
  "so_luong": 2,
  "don_gia": 350000,
  "tong_tien": 700000,
  "trang_thai": "Hợp lệ",
  "loi_du_lieu": []
}
```

Kết quả Table:

| Mã đơn | Khách hàng | Sản phẩm | SL | Tổng tiền | Trạng thái |
| ----- | ----- | ----- | ----- | ----- | ----- |
| DH001 | Nguyễn Văn An | Tai nghe Bluetooth | 2 | 700.000đ | Hợp lệ |

### **3\. Kiểm tra dữ liệu lỗi**

Nếu số lượng bị bỏ trống, AI cần trả về:

```
{
  "ma_don_hang": "DH001",
  "so_luong": null,
  "tong_tien": null,
  "trang_thai": "Cần kiểm tra",
  "loi_du_lieu": ["Thiếu số lượng"]
}
```

## **Kết luận**

Giải pháp 1 có ưu điểm là ngắn gọn, tiết kiệm Token và dễ sử dụng. Giải pháp 2 tuy tốn nhiều Token hơn nhưng có khả năng kiểm soát dữ liệu và xử lý lỗi tốt hơn. Vì vậy, em lựa chọn Giải pháp 2 để áp dụng vào quy trình xử lý đơn hàng thực tế, giúp tăng độ chính xác và hạn chế sai sót.

