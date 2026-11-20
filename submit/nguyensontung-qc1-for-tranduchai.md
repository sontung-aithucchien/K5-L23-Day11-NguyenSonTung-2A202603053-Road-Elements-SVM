# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): Trần Đức Hải        Người kiểm (học viên): Nguyễn Sơn Tùng  
Ngày: 21-11-2026                               Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `tranduchai-front.zip` và `tranduchai-fisheye.zip` · revision 1  
Số ảnh kiểm: 6 (front: A01, A02, A08; fisheye: B03, B05, B06)   Số nhãn đã kiểm (shape + tag): 63 shape + 12 tag = **75**

> ⚠️ **Đánh giá tổng quan:**
> - **Task front:** Toàn bộ 16 ảnh bị thiếu hoàn toàn 4 tag ảnh và không có polyline `lane` nào.
> - **Task fisheye:** Đã có đủ 4 tag ảnh trên 12 ảnh. Tuy nhiên, mắc lỗi phân loại làn ở camera hai bên (B05, B06, B07 đặt `position = L1` thay vì `other` theo quy định an toàn fisheye) và polyline vẽ quá ít điểm (B03 chỉ có 3-4 điểm, chưa bám sát độ cong fisheye).

## Tổng hợp theo mã

| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 | **3** | **nặng** |
| QC1-02 đủ L1, R1 | bỏ sót | 6 | **3** | **nặng** |
| QC1-03 position | sai thuộc tính / thừa | 6 | **2** | **nặng** |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 4 | 0 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 4 | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 4 | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 6 | 0 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 6 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 6 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 6 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 6 | **2** | **chính** |
| QC1-12 file | lỗi file | 2 | 1 | nhẹ |

*Chi tiết:*
- **QC1-01 (3 lỗi nặng):** 3 ảnh front được kiểm (A01, A02, A08) đều thiếu toàn bộ 4 tag ảnh (trong đó thiếu tag `camera` là lỗi nặng).
- **QC1-02 (3 lỗi nặng):** 3 ảnh front được kiểm (A01, A02, A08) có vạch làn nhìn rõ nhưng không có polyline `lane` L1/R1 nào.
- **QC1-03 (2 lỗi nặng):** Ảnh B05 và B06 (camera hông `fisheye_left`) gán `position = L1`. Quy định Road v1.0 bắt buộc camera hai bên phải dùng `position = other` vì không thể xác định hướng làn L1 của xe từ camera hông.
- **QC1-11 (2 lỗi chính):** Ảnh B03 polyline làn chỉ có 3 điểm (vạch trái) và 4 điểm (vạch phải), không uốn lượn theo cung méo fisheye. Ngoài ra các biển báo sát mép (như B03, B04) chưa bật `edge_zone = true`.
- **QC1-12 (1 lỗi nhẹ):** Cả 2 file zip đều nén kèm thư mục `images/` (`contains_images: true`), cần export lại với Save images = off.

## Danh sách lỗi (mỗi lỗi một dòng)

| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-01 | A01 | tag camera | nặng | Thiếu tag camera (và timeofday, weather, source) |
| 2 | QC1-02 | A01 | lane L1 | nặng | Không vẽ polyline lane L1 dù vạch kẻ rõ ràng |
| 3 | QC1-02 | A01 | lane R1 | nặng | Không vẽ polyline lane R1 dù vạch kẻ rõ ràng |
| 4 | QC1-01 | A02 | tag camera | nặng | Thiếu tag camera (và 3 tag còn lại) |
| 5 | QC1-02 | A02 | lane L1 | nặng | Bỏ sót polyline lane L1 |
| 6 | QC1-02 | A02 | lane R1 | nặng | Bỏ sót polyline lane R1 |
| 7 | QC1-01 | A08 | tag camera | nặng | Thiếu tag camera (và 3 tag còn lại) |
| 8 | QC1-02 | A08 | lane R1 | nặng | Bỏ sót polyline lane R1 |
| 9 | QC1-03 | B05 | lane#2 | nặng | Camera hông (MVL) gán `position = L1`, bắt buộc phải là `other` |
| 10 | QC1-03 | B06 | lane#0 | nặng | Camera hông (MVL) gán `position = L1`, bắt buộc phải là `other` |
| 11 | QC1-11 | B03 | lane#2 | chính | Polyline làn fisheye chỉ có 3 điểm, không vẽ bám theo cung cong |
| 12 | QC1-11 | B03 | traffic_sign#7 | chính | Biển báo ở tọa độ ngoài 80% bán kính mép ảnh nhưng chưa đánh dấu `edge_zone` |
| 13 | QC1-12 | file | zip front & fisheye | nhẹ | Zip còn chứa ảnh thực tế (`contains_images: true`) |

## Ca mơ hồ

| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A05 | Ảnh này bạn không vẽ lane nào: có phải do mặt đường xe che khuất / không thấy rõ vạch kẻ? (Nếu coach xác nhận thì miễn QC1-02). | _(Chờ xác nhận)_ |
| B04 | Vạch kẻ phía sau xe ở ảnh RV bị đứt quãng nhẹ, bạn gán 1 polyline L1 7 điểm đã hợp lệ hay cần bổ sung thêm curb? | _(Chờ xác nhận)_ |

## Quyết định

Lỗi nặng: **8** (QC1-01: 3, QC1-02: 3, QC1-03: 2)   Lỗi chính: **2** / 75 nhãn = 2.67 %   Lỗi nhẹ: 1   Ca chờ lead: 2  
Quyết định (Release / Hold / Rework): **Rework**  
Lý do: Lô có lỗi nặng (thiếu tag camera, bỏ sót L1/R1 trên front, sai position camera hông trên fisheye). Theo quy tắc kiểm định Road v1.0, có ≥ 1 lỗi nặng bắt buộc phải Rework.

---

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](docs/qc_severity_road_v1.0.md).
