# OPERATING DASHBOARD — ReviewLens AI

**Loại mô hình:** B2C (Freemium Browser Extension) · **Cập nhật:** 09/10/2026 · Thân Thị Kim Chi – 2A202602797  
**NORTH STAR:** Weekly Active Audits (WAA — lượt audit hoàn chỉnh/tuần) — hiện tại 1.850 — mục tiêu 5.000


### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **Đường cong retention D30** | 14,2% | 🟢 Phẳng D30 (≥18%) · 🟡 Dốc chậm (12–18%) · 🔴 Dốc tiếp (<12%) | [TB] 30/11/2026 | Retention M12 & LTV |
| **Activation Rate (24h)** | 38,5% | 🟢 ≥45% · 🟡 30%–45% · 🔴 <30% | [TB] 1.200 users | Trial → Paid & Retention D30 |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **p95 cost/user/tháng ÷ ARPU** ⭐ *(AI)* | 42,0% | 🟢 <30% (<29.7k) · 🟡 30%–60% · 🔴 >60% (>59.4k) | [MH] GM 65% | Gross Margin |
| **Trial → Paid conversion** | 6,8% | 🟢 ≥8,5% · 🟡 5,0%–8,5% · 🔴 <5,0% | [BM] 27/08/2026 | Doanh thu & Payback |
| **Chi phí Free tier ÷ Tổng COGS** ⭐ *(AI)* | 28,4% | 🟢 <20% · 🟡 20%–40% · 🔴 >40% | [MH] GM 65% | Gross Margin & Runway |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| **Gross Margin (Biên gộp)** | 58,2% | 🟢 ≥65% · 🟡 50%–65% · 🔴 <50% | [MH] Mô hình tài chính Day 24 |
| **CAC Payback (tháng)** | 2,2 tháng | 🟢 <2,0 tháng · 🟡 2,0–2,5 tháng · 🔴 >2,5 tháng | [MH] Runway 12 tháng, ARPU 99k |

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** retention D30 < 12% **TRONG** 2 cohort liên tiếp **VÀ** mỗi cohort ≥ 200 user **THÌ** đóng băng 100% ngân sách ads acquisition trong 3 tuần, chuyển toàn đội sang tối ưu onboarding và độ chuẩn tóm tắt review **KHÔNG THÌ** không được tăng ngân sách ads để bù churn.
2. ⏹ **NẾU** chi phí Free tier > 40% tổng COGS **TRONG** 2 tuần liên tiếp **THÌ** hạ hạn mức miễn phí từ 5 xuống 2 lượt audit/ngày và bắt buộc đăng nhập tài khoản **KHÔNG THÌ** không nới quota free để thổi phồng user ảo.
3. **NẾU** p95 cost/user > 60% ARPU (> 59.400đ) **TRONG** 1 tháng **THÌ** áp trần FUP 100 lượt/ngày cho gói Pro và ra mắt gói Pro Max cho power user **KHÔNG THÌ** không tăng giá gói Pro cơ bản lên 90% user dùng nhẹ.
4. **NẾU** Trial → Paid < 5,0% **TRÊN** 3 đợt thử paywall liên tiếp **VÀ** mẫu ≥ 150 trial users **THÌ** cấu trúc lại value metric và tính năng kích hoạt cốt lõi **KHÔNG THÌ** không giảm giá dưới sàn 79.000đ/tháng làm hỏng neo giá.
5. ⏹ **NẾU** CAC Payback > 2,5 tháng **TRONG** 2 tháng liên tiếp **THÌ** cắt 50% ngân sách ads trả phí kém hiệu quả, dồn lực vào SEO Chrome Web Store và kênh giới thiệu Referral **KHÔNG THÌ** không duy trì chi tiêu marketing cũ để chờ LTV cứu vớt.

### Cổng gác 90 ngày

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| **30** | Baseline Activation Rate (24h) | ≥ 45% trên cohort ≥ 500 user cài mới | Báo cáo Mixpanel cohort funnel log | FIX (sửa onboarding 30 ngày) |
| **60** | Đường cong retention D30 | Phẳng ngang từ D30 (tỷ lệ giữ chân ≥ 18%) | Đồ thị retention cohort xuất từ Mixpanel | PIVOT (thu hẹp use case chỉ thẩm định hàng điện tử) |
| **90** | Trial → Paid conversion rate | ≥ 8,0% và Gross Margin ≥ 60% | Báo cáo doanh thu PayOS & bảng đối soát COGS | KILL (đóng dự án, hoàn trả vốn còn lại) |

**KILL CRITERIA:** Nếu sau 90 ngày (09/01/2027) tỷ lệ Trial → Paid < 4,0% hoặc Runway còn < 4 tháng mà chưa đạt Gross Margin 50%, dừng toàn bộ dự án, hoàn trả tiền cho khách hàng Pro còn hạn và bảo lưu vốn còn lại.

**CHƯA ĐO ĐƯỢC:** Retention M12 (Cần 9 tháng nữa để cohort tháng đầu tiên hoàn thành đủ chu kỳ 12 tháng; hiện lấy tạm benchmark ngành 21,1% theo RevenueCat để mô hình hóa LTV; dự kiến có số thực tế vào ngày 31/07/2027).

