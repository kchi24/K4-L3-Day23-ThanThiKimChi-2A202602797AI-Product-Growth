# Worksheet — ReviewLens AI (Smart E-Commerce Review Inspector)

Họ tên: Thân Thị Kim Chi · MSSV: 2A202602797 

## 0. Số liệu đầu vào từ Mô hình tài chính & Cost/Job

- **Sản phẩm:** ReviewLens AI — Trợ lý AI tiện ích mở rộng trình duyệt (Browser Extension) tự động trích xuất, thẩm định review thật/ảo, tóm tắt ưu/nhược điểm và cảnh báo seeding khi mua sắm trên Shopee/TMĐT.
- **Mô hình kinh doanh:** B2C Freemium (bản miễn phí giới hạn 5 lần/ngày; gói Pro trả phí tháng).
- **ARPU:** 99.000 VNĐ / user Pro / tháng.
- **Gross Margin mục tiêu (GM):** 65% (tương đương COGS mục tiêu ≤ 35% ARPU = 34.650 VNĐ/tháng).
- **CAC:** 120.000 VNĐ / paying customer (kênh chính: short-form video demo TikTok/Reels và SEO Chrome Web Store).
- **CAC Payback mục tiêu:** ≤ 2,5 tháng (tương đương ~75 ngày).
- **Runway:** 12 tháng (ngân sách 240.000.000 VNĐ).
- **Value Metric:** Lượt thẩm định & tóm tắt sản phẩm hoàn chỉnh (Completed Product Review Audit).
- **Cost/Job (Chi phí AI cho 1 lượt tóm tắt sản phẩm):** 450 VNĐ / job (Pipeline: Rule-based client lọc sạch text rác + mô hình nhỏ phân loại + LLM trích xuất & tổng hợp ~1.500 input tokens, 300 output tokens).

---

## Trạm 1 — Loại mô hình & Bảng đèn (15')

### 1. Trả lời 3 câu hỏi theo thực tế hôm nay:
- **Câu hỏi 1: Ai trả tiền cho bạn?**  
  → **Cá nhân (Người tiêu dùng).** Người mua sắm trực tuyến tự thanh toán gói thuê bao Pro cá nhân (99.000 VNĐ/tháng) qua tài khoản ngân hàng / ví điện tử (PayOS). Không có doanh nghiệp B2B hay tổ chức nào trả tiền.
- **Câu hỏi 2: Ai dùng sản phẩm?**  
  → **Chính người trả tiền.** Người mua sắm cài extension ReviewLens AI trực tiếp lên trình duyệt Chrome/Edge của họ để đọc tóm tắt và soi review ảo mỗi khi lướt xem hàng hóa trên Shopee.
- **Câu hỏi 3: Nếu có bên trung gian: bạn có chạm được người dùng cuối không?**  
  → **Không có bên trung gian.** Sản phẩm phát hành trực tiếp tới người dùng qua Chrome Web Store, giao tiếp trực diện với người dùng trên giao diện web của sàn TMĐT. Chúng tôi sở hữu trực tiếp tài khoản, event tracking và tương tác của người dùng cuối.

**Câu chốt loại:** Chúng tôi là **B2C** vì tiền đến từ người tiêu dùng cá nhân (người mua sắm online trên Shopee/Lazada chi trả gói Pro 99.000đ/tháng), chính họ là người trực tiếp cài đặt và sử dụng extension trên trình duyệt của mình để ra quyết định mua hàng, và không có bất kỳ tổ chức hay doanh nghiệp trung gian nào đứng giữa quản lý hay ăn chia doanh thu.

---

### 2. Bảng đèn §3 của loại mình (B2C — §3.1 HANDBOOK):

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| **Đường cong retention có phẳng không** ⭐ | ✅ | Đo được hôm nay. Nằm trong Mixpanel cohort retention report (theo dõi cohort D1, D7, D14, D30, D60). |
| **Activation rate** | ✅ | Đo được hôm nay. Database event log: % user hoàn thành ≥1 lần tóm tắt review (aha moment) trong 24h đầu sau khi cài extension. |
| **p95 cost/user/tháng ÷ ARPU** | 🔧 | Đo được trong 2 tuần. Cần viết cron job tổng hợp log token inference theo User ID từ Langfuse sang Google Sheet tài chính mỗi sáng thứ Hai. |
| **Trial → paid** | ✅ | Đo được hôm nay. Báo cáo conversion rate từ webhook cổng thanh toán PayOS / database subscription. |
| **Retention M12** | ❌ | Chưa đo được. Sản phẩm mới ra mắt 3 tháng, chưa có cohort nào đạt vòng đời 12 tháng; cần thêm 9 tháng nữa để có số thực tế. |
| **Chi phí free tier ÷ tổng COGS** | ✅ | Đo được hôm nay. Dashboard quản lý chi phí API LLM (phân tách rõ tag traffic: user_free vs user_pro). |
| **Tỷ lệ refund** | ✅ | Đo được hôm nay. Báo cáo giao dịch hoàn tiền hàng tháng từ cổng thanh toán PayOS. |
| **LTV/CAC · CAC payback · GM** | 🔧 | Đo được trong 2 tuần. Hiện đang tính thủ công trên bảng Excel mô hình tài chính Day 24; cần 2 tuần để tích hợp pipeline tự động kéo số doanh thu và chi phí ads. |


---

## Trạm 2 — Thẻ đèn (Cây 3 tầng)

**North Star:** Số lượt thẩm định sản phẩm có giá trị hoàn thành mỗi tuần (Weekly Active Audits - WAA) — hiện tại: 1.850 lượt/tuần — mục tiêu: 5.000 lượt/tuần.

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | L | **Đường cong retention D30** | Đếm % user trong cohort còn quay lại thực hiện ≥1 lần audit ở ngày thứ 30. **Không** đếm user chỉ mở trình duyệt nhưng không bấm audit sản phẩm. | (Active users ngày 30 của cohort) ÷ (Tổng user cài extension ở D0 của cohort) | Hằng tuần · Product Data Lead | Retention M12 & LTV (Tầng O/G) |
| 2 | L | **Activation Rate (24h)** | Đếm % user mới thực hiện thành công lần audit đầu tiên (aha moment) trong vòng 24 giờ kể từ lúc cài extension. **Không** đếm lượt cài đặt rồi để đó hoặc lỗi trích xuất DOM. | (Số user thực hiện ≥1 audit thành công trong 24h) ÷ (Tổng số user mới cài đặt) | Hằng ngày · Growth Lead | Trial → Paid & Retention D30 (Tầng O) |
| 3 | O | **p95 cost/user/tháng ÷ ARPU** ⭐ *(Đèn chi phí AI)* | Đếm tỷ lệ chi phí token suy luận AI của user ở phân vị 95 (user dùng nặng nhất) so với mức ARPU 99.000đ. **Không** lấy mức chi phí trung bình (mean) vì bị lệch mẫu. | (Chi phí inference tháng của user tại p95) ÷ ARPU | Hằng tuần · Tech Lead / AI Eng | Gross Margin (Tầng G) |
| 4 | O | **Trial → Paid conversion rate** | Đếm % user kích hoạt dùng thử 7 ngày chuyển đổi thành công sang thanh toán chu kỳ đầu tiên. **Không** đếm các giao dịch thanh toán thất bại hoặc thẻ bị từ chối. | (Số user trả tiền chu kỳ 1) ÷ (Số user đăng ký trial 7 ngày) | Hằng tháng · Growth Lead | Doanh thu & CAC Payback (Tầng G) |
| 5 | O | **Chi phí Free tier ÷ Tổng COGS** ⭐ *(Đèn chi phí AI)* | Đếm tỷ lệ chi phí API token tiêu tốn bởi nhóm người dùng miễn phí trên tổng chi phí biến đổi (COGS). **Không** tính chi phí server cố định. | (Tổng chi phí token của Free users) ÷ (Tổng COGS AI + server trong kỳ) | Hằng tháng · Tech Lead / Finance | Gross Margin & Runway (Tầng G) |
| 6 | G | **Gross Margin (Biên lợi nhuận gộp)** | Đếm tỷ lệ doanh thu thuần còn lại sau khi trừ toàn bộ chi phí biến đổi (token AI, hosting, phí cổng thanh toán). **Không** trừ chi phí lương cố định và marketing. | (Doanh thu thuần − COGS) ÷ Doanh thu thuần | Hằng quý · CEO / Founder | Khả năng tự chủ tài chính & Runway |
| 7 | G | **CAC Payback (thời gian thu hồi vốn CAC)** | Đếm số tháng cần thiết để lãi gộp tạo ra từ một khách hàng trả phí bù đắp đủ chi phí thu hút khách hàng đó (CAC). **Không** tính doanh thu gộp chưa trừ chi phí inference. | CAC ÷ (ARPU × Gross Margin %) | Hằng quý · CEO / Growth Lead | Runway & Hiệu quả vốn |

**Đèn chi phí AI là đèn số:** Đèn 3 (p95 cost/user/tháng ÷ ARPU) và Đèn 5 (Chi phí Free tier ÷ Tổng COGS).

---

## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn [BM]/[MH]/[TB] | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | Đường cong retention D30 | Phẳng từ D30 (≥18%) | Dốc giảm chậm (12–18%) | Tiếp tục dốc xuống (<12%) | [TB] | Baseline nội bộ đo sau 3 cohort; đường cong phải phẳng ra thì LTV mới hội tụ và mô hình mới có nghĩa. (Dự kiến chốt baseline 30/11/2026). |
| 2 | Activation Rate (24h) | ≥ 45% | 30% – 45% | < 30% | [TB] | Đo trên 1.200 lượt cài đặt gần nhất, tỷ lệ kích hoạt phải trên 45% để đảm bảo người dùng trải nghiệm giá trị trước khi quên extension. |
| 3 | p95 cost/user/tháng ÷ ARPU | < 30% (< 29.700đ) | 30% – 60% (29.700đ – 59.400đ) | > 60% (> 59.400đ) | [MH] | Suy từ mục tiêu Gross Margin 65%: nhóm 5% dùng nặng nhất không được ngốn quá 60% ARPU để tránh nuốt trọn biên lợi nhuận của toàn bộ tệp khách hàng. |
| 4 | Trial → Paid conversion rate | ≥ 8,5% | 5,0% – 8,5% | < 5,0% | [BM] | Theo báo cáo RevenueCat State of Subscription Apps 2026, benchmark chuyển đổi của ứng dụng AI tiêu dùng là 8,5% (kiểm tra ngày 27/08/2026). |
| 5 | Chi phí Free tier ÷ Tổng COGS | < 20% | 20% – 40% | > 40% | [MH] | Suy từ mô hình chi phí: nếu tệp miễn phí chiếm trên 40% tổng COGS thì Gross Margin sẽ tụt xuống dưới 50%, đe dọa trực tiếp runway của startup. |
| 6 | Gross Margin | ≥ 65% | 50% – 65% | < 50% | [MH] | Suy từ mô hình tài chính Day 24: Gross Margin phải đạt tối thiểu 65% để trang trải chi phí vận hành và giữ CAC Payback dưới 2,5 tháng. |
| 7 | CAC Payback | < 2,0 tháng | 2,0 – 2,5 tháng | > 2,5 tháng | [MH] | Suy từ mô hình dòng tiền với runway 12 tháng: thời gian thu hồi vốn marketing phải dưới 2,5 tháng để kịp tái đầu tư vòng quay tăng trưởng mà không cạn vốn. |

### Phụ lục [MH] — phép tính (≥2)

**[MH] 1 — p95 cost/user/tháng ÷ ARPU (Đèn số 3)**

```
Đầu vào (từ mô hình tài chính & Cost/Job):
- ARPU = 99.000 VNĐ / tháng
- Gross Margin mục tiêu = 65% → Tổng COGS mục tiêu cho 1 user trung bình = 99.000 × (1 − 0,65) = 34.650 VNĐ/tháng (~35% ARPU).
- Cost/Job = 450 VNĐ / lượt audit sản phẩm.
- Trung bình 1 user Pro thực hiện ~40 lượt audit/tháng = 18.000 VNĐ chi phí token (chiếm 18,2% ARPU).

Phép tính:
- Phân phối sử dụng AI luôn bị lệch đuôi (power users): 5% người dùng nặng nhất (p95) audit với tần suất cao gấp 3-4 lần.
- Ngưỡng nguy hiểm (🔴): Nếu p95 cost vượt quá 60% ARPU = 99.000 × 60% = 59.400 VNĐ/tháng (tương đương >132 lượt audit/tháng), nhóm này sẽ nuốt sạch biên lợi nhuận đóng góp.
- Ngưỡng an toàn (🟢): Chi phí p95 duy trì dưới 30% ARPU = 29.700 VNĐ/tháng (≤66 lượt audit/tháng).
- Ngưỡng cảnh báo (🟡): Chi phí p95 dao động từ 30% đến 60% ARPU (29.700 VNĐ – 59.400 VNĐ).

Kết quả → 🟢 < 30% (< 29.700 VNĐ) · 🟡 30% – 60% (29.700 – 59.400 VNĐ) · 🔴 > 60% (> 59.400 VNĐ).
```

**[MH] 2 — Chi phí Free tier ÷ Tổng COGS (Đèn số 5)**

```
Đầu vào (từ mô hình tài chính Day 24 & Cost/Job):
- Doanh thu hàng tháng giả định cho cohort 500 paying users = 500 × 99.000 = 49.500.000 VNĐ.
- Với GM mục tiêu 65%, tổng COGS tối đa cho phép = 49.500.000 × 35% = 17.325.000 VNĐ.
- Chi phí COGS phục vụ trực tiếp cho paying users = 500 × 18.000 VNĐ = 9.000.000 VNĐ.
- Ngân sách COGS còn lại tối đa dành cho Free tier = 17.325.000 − 9.000.000 = 8.325.000 VNĐ (tương đương 48,0% tổng COGS).

Phép tính:
- Để bảo vệ Gross Margin luôn ≥60%, chi phí tài trợ cho user miễn phí không được vượt quá 40% tổng COGS:
  17.325.000 × 40% = 6.930.000 VNĐ (~15.400 lượt audit free/tháng).
- Nếu Chi phí Free tier > 40% Tổng COGS: Gross Margin thực tế sẽ rơi tự do xuống dưới 50%.
- Nếu Chi phí Free tier < 20% Tổng COGS: Hoàn toàn an toàn, biên gộp được bảo toàn >65%.

Kết quả → 🟢 < 20% · 🟡 20% – 40% · 🔴 > 40%.
```

**[MH] 3 — CAC Payback (Đèn số 7)**

```
Đầu vào (từ mô hình tài chính Day 24):
- ARPU = 99.000 VNĐ/tháng.
- Gross Margin = 65% → Lãi gộp mỗi user mỗi tháng = 99.000 × 65% = 64.350 VNĐ.
- CAC mục tiêu = 120.000 VNĐ.

Phép tính:
- Thời gian hoàn vốn chuẩn = CAC ÷ Lãi gộp tháng = 120.000 ÷ 64.350 = 1,86 tháng (~56 ngày).
- Với điều kiện Runway 12 tháng, startup chỉ có thể tái đầu tư an toàn nếu dòng tiền từ CAC quay vòng trong vòng tối đa 2,5 tháng:
  CAC trần cho phép = 2,5 × 64.350 = 160.875 VNĐ.
- Vượt quá 2,5 tháng: Tốc độ đốt tiền marketing nhanh hơn tốc độ hoàn vốn, gây rủi ro đứt gãy dòng tiền trước tháng thứ 8.

Kết quả → 🟢 < 2,0 tháng · 🟡 2,0 – 2,5 tháng · 🔴 > 2,5 tháng.
```

---

## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng (bắt buộc ≥2 luật dừng).

1. ⏹ **NẾU** đường cong retention chưa phẳng sau D30 (tỷ lệ duy trì < 12%) **TRONG** 2 cohort liên tiếp **VÀ** mỗi cohort có quy mô ≥ 200 user mới **THÌ** đóng băng 100% ngân sách quảng cáo acquisition trong 3 tuần và chuyển toàn bộ đội ngũ kỹ thuật sang tối ưu luồng onboarding và độ chính xác của tóm tắt review **KHÔNG THÌ** tuyệt đối không được tăng tiền chạy ads TikTok/Facebook để bù đắp lượng user rời bỏ.
2. ⏹ **NẾU** chi phí Free tier vượt quá 40% tổng COGS **TRONG** 2 tuần liên tiếp **THÌ** hạ ngay hạn mức dùng thử miễn phí từ 5 lượt/ngày xuống 2 lượt/ngày và bắt buộc đăng nhập tài khoản để giới hạn abuse token **KHÔNG THÌ** không được nới rộng quota free hoặc thả nổi truy cập ẩn danh nhằm mục đích tăng trưởng ảo số lượng người dùng.
3. **NẾU** p95 cost/user/tháng vượt quá 60% ARPU (> 59.400đ) **TRONG** 1 tháng thanh toán **THÌ** áp dụng chính sách sử dụng hợp lý (FUP trần 100 lượt audit/ngày) và triển khai gói cước Pro Max riêng biệt cho power user **KHÔNG THÌ** không được tăng giá gói Pro cơ bản lên toàn bộ người dùng thông thường vì sẽ làm sụp đổ tỷ lệ giữ chân của 90% khách hàng dùng ít.
4. **NẾU** tỷ lệ Trial → Paid giảm xuống dưới 5,0% **TRÊN** 3 đợt thử nghiệm giao diện paywall liên tiếp **VÀ** mẫu thử nghiệm đạt ≥ 150 trial users **THÌ** quay lại tái cấu trúc value metric và định vị lại tính năng cốt lõi kích hoạt nhu cầu (soát review seeding vs tóm tắt thông số) **KHÔNG THÌ** không được tự ý giảm giá gói cước dưới mức sàn 79.000đ/tháng làm phá vỡ neo giá thương hiệu.
5. ⏹ **NẾU** CAC Payback vượt quá 2,5 tháng **TRONG** 2 tháng liên tiếp **THÌ** cắt giảm ngay 50% ngân sách chiến dịch quảng cáo trả phí kém hiệu quả và chuyển trọng tâm sang kênh organic SEO Chrome Web Store và chương trình giới thiệu người dùng (Referral) **KHÔNG THÌ** không được duy trì chi tiêu marketing theo kế hoạch cũ với hy vọng LTV dài hạn sẽ tự động bù đắp lại.

