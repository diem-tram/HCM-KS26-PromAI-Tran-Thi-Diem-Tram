# BÀI LÀM PHA 2 

- **Họ tên:**
- **Lớp:**
- **Link repository Pha2:**
- **Công cụ AI đã dùng (ChatGPT/Claude/Gemini):**

> Pha 2 (20 phút cuối). Mỗi mục: mở cuộc trò chuyện mới, gửi nguyên văn prompt đã viết trong bai_lam_pha1.md, dán câu trả lời và link chia sẻ vào đây. Xong, kéo thả file này vào repository Pha2.

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**KET QUA AI**

```text
```json
[
  {
    "ma_goi": "SVD",
    "ngay_su_kien": "2026-12-20",
    "nguoi_lon": 2,
    "tre_em": 1,
    "loai_ve": "thuong",
    "ghi_chu": null
  },
  {
    "ma_goi": "FZ",
    "ngay_su_kien": "2027-01-05",
    "nguoi_lon": 3,
    "tre_em": 0,
    "loai_ve": null,
    "ghi_chu": "hỗ trợ xe lăn"
  },
  {
    "ma_goi": "CK",
    "ngay_su_kien": null,
    "nguoi_lon": 4,
    "tre_em": 0,
    "loai_ve": "vip",
    "ghi_chu": "hỗ trợ tìm chỗ cho người khiếm thị"
  }
]

https://share.gemini.google/2xN8Y2xA57Yi

## 4.3. Báo giá vé bằng Chain-of-Thought

**KET QUA AI**

```text
```python
svt_price = 1200000
num_adults = 5
# children 8 and 10 -> between 5 and 11 -> 70% of adult price
# child 3 -> under 5 -> free
num_children_70 = 2
num_children_free = 1
priority_seats = 1
priority_price = 300000

tien_nguoi_lon = num_adults * svt_price
tien_tre_em = num_children_70 * int(svt_price * 0.7) + num_children_free * 0
tien_ve = tien_nguoi_lon + tien_tre_em
# 5 adults >= 5 -> 5% discount on ticket price (tien_ve)
giam_gia_nhom = int(tien_ve * 0.05)
tien_ve_sau_giam = tien_ve - giam_gia_nhom
phu_thu = priority_seats * priority_price
tong_tien = tien_ve_sau_giam + phu_thu

print(f"Tiền người lớn: {tien_nguoi_lon}")
print(f"Tiền trẻ em: {tien_tre_em}")
print(f"Tiền vé: {tien_ve}")
print(f"Giảm giá nhóm: {giam_gia_nhom}")
print(f"Phụ thu: {phu_thu}")
print(f"Tổng tiền: {tong_tien}")


```

```text
Tiền người lớn: 6000000
Tiền trẻ em: 1680000
Tiền vé: 7680000
Giảm giá nhóm: 384000
Phụ thu: 300000
Tổng tiền: 7596000


```

Tiền người lớn: 5 x 1.200.000 = 6.000.000 đồng
Tiền trẻ em: 2 x (1.200.000 x 70%) + 1 x 0 = 2 x 840.000 + 0 = 1.680.000 đồng
Tiền vé: 6.000.000 + 1.680.000 = 7.680.000 đồng
Giảm giá nhóm: 7.680.000 x 5% = 384.000 đồng
Phụ thu: 1 x 300.000 = 300.000 đồng
Tổng tiền: (7.680.000 - 384.000) + 300.000 = 7.296.000 + 300.000 = 7.596.000 đồng
[RESULT:7596000đồng]
```
https://share.gemini.google/9fX8LifGfjmJ

### b) Kiểm tra kết quả AI

kết quả AI trả về là đúng

## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```text
```json
{
  "loai_su_co": "sai_khu_vuc",
  "so_khach": 2,
  "so_hiep_bi_anh_huong": 2,
  "tom_tat": "Hai khách mua vé gói SVT bị xếp sai sang khu vực không mái che trong 2 hiệp đầu."
}

```

https://share.gemini.google/8NJ78q85tsne

## 4.5. Prompt chấm bài theo Rubric

**KET QUA AI**

```text
| Tiêu chí | Điểm | Minh chứng | Đề xuất sửa |
| --- | --- | --- | --- |
| Đúng thông tin (50%) | 1/5 | **"Vé Sân Vận Động bên em giá 1.000.000đ/người"** — Lý do sai: Giá chuẩn là 1.200.000 đồng/người lớn. | "Vé SVT bên em có giá 1.200.000 đồng/người lớn ạ." |
| Đúng thông tin (50%) | 1/5 | **"Anh huỷ trước 3 ngày thì được hoàn tiền đầy đủ nha anh."** — Lý do sai: Huỷ từ 3 đến 6 ngày chỉ hoàn 50%, không phải 100%. | "Nếu huỷ trước sự kiện từ 7 ngày trở lên sẽ được hoàn 100%, từ 3 đến 6 ngày hoàn 50%, và dưới 3 ngày sẽ không hoàn tiền ạ." |
| Đầy đủ (30%) | 0/3 | **Không có câu hỏi mời cung cấp thông tin đặt vé** — Lý do sai: Thiếu lời mời khán giả cung cấp ngày trận đấu và số lượng vé để đặt (và có thể thiếu đầy đủ câu hỏi khán giả đã đặt). | "Để hỗ trợ đặt vé, anh/chị vui lòng cho em xin ngày diễn ra trận đấu và số lượng vé cần đặt ạ." |
| Giọng điệu (20%) | 0,5/2 | **"xem rất đã luôn ạ."**, **"nha anh."**, **"cứ nhắn em nhé!"** — Lý do sai: Dùng từ suồng sã ("đã luôn", "nha", "nhé"), thiếu chuyên nghiệp. | "Anh/chị cần hỗ trợ thêm thông tin nào, vui lòng phản hồi để em hỗ trợ ạ." |
| **Tổng điểm** | **2,5/10** |  |  |
```
https://share.gemini.google/fSjE51R7vbz4
