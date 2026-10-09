# Day 23 — Operating Dashboard · ReviewLens AI

- **Họ và tên:** Thân Thị Kim Chi
- **MSSV:** 2A202602797
- **Tên sản phẩm:** ReviewLens AI (Smart E-Commerce Review Inspector)
- **Loại mô hình:** **B2C** (Freemium Browser Extension)
- **Câu chốt loại:** Chúng tôi là **B2C** vì tiền đến từ người tiêu dùng cá nhân (người mua sắm online trên Shopee/Lazada chi trả gói Pro 99.000đ/tháng), chính họ là người trực tiếp cài đặt và sử dụng extension trên trình duyệt của mình để ra quyết định mua hàng, và không có bất kỳ tổ chức hay doanh nghiệp trung gian nào đứng giữa quản lý hay ăn chia doanh thu.
- **Tài liệu nộp bài:**
  - [Worksheet (Trạm 1–4)](worksheet.md)
  - [Operating Dashboard (Trạm 5)](dashboard.md)

---


## Mục tiêu học tập

Sau bài này bạn làm được:

1. Xác định đúng sản phẩm mình là **B2C, B2B hay B2B2C** theo thực tế hôm nay, và biết "đèn bật trước" của loại đó.
2. Xếp chỉ số vào 3 tầng **Leading · Operating · Lagging** và chỉ ra mỗi đèn báo sớm báo trước cho đèn nào.
3. Đặt ngưỡng có nguồn: benchmark có ngày kiểm tra **[BM]**, suy ngược từ mô hình của bạn **[MH]**, hoặc tự đo baseline **[TB]**.
4. Viết 5 luật quyết định dạng **NẾU – TRONG – (VÀ) – THÌ – KHÔNG THÌ**, trong đó ít nhất 2 luật bảo bạn **dừng** một việc.
5. Đặt 3 cổng gác ngày 30/60/90 (GO · FIX · PIVOT · KILL) và kill criteria.

**Đầu ra:** một `Operating Dashboard` **1 trang** + phụ lục ≤1 trang cho các phép tính [MH].

## Chuẩn bị

| Cần có | Dùng để |
|---|---|
| Số liệu mô hình tài chính của bạn: ARPU, gross margin, CAC, payback mục tiêu, runway | Suy ngưỡng [MH] |
| Value Metric và **Cost/Job** của sản phẩm (chi phí AI cho một việc) | Đèn chi phí AI, ngưỡng [MH] |
| Tài khoản GitHub + trình xem Markdown (VS Code, GitHub web…) | Làm và nộp bài |
| Một chatbot AI bất kỳ (tuỳ chọn) | Chạy prompt phản biện ở [HANDBOOK §5](HANDBOOK.md#5-prompts-cho-ai-english-only) |

Chưa có mô hình tài chính hoặc Cost/Job? Bạn vẫn đọc và làm được Trạm 1–2, nhưng **không đạt** Trạm 3 vì cần ít nhất 2 ngưỡng [MH] tính từ số của chính bạn.

## Bắt đầu trong 3 phút

1. Tạo repo **mới** trên GitHub tên `K4-L3-DAY23-HoVaTen-MSSV-AIProductGrowth` (quy tắc ở [SUBMISSION.md](SUBMISSION.md)).
2. Copy 2 file mẫu trong [`templates/`](templates/) vào repo của bạn:
   - [`worksheet.md`](templates/worksheet.md) — nháp làm việc cho Trạm 1–4 (thẻ đèn đầy đủ, phép tính [MH]).
   - [`dashboard.md`](templates/dashboard.md) — bản 1 trang cuối cùng (Trạm 5).
3. Đọc [HANDBOOK §2](HANDBOOK.md#2-hệ-chẩn-đoán) trong 10 phút, rồi làm lần lượt theo [CHECKPOINTS.md](CHECKPOINTS.md).

## Thời lượng — 120 phút, 5 trạm

| Trạm | Việc | Thời gian |
|---|---|---|
| 1 | Chốt loại mô hình & lấy bảng đèn | 15' |
| 2 | Dựng cây 3 tầng (6–8 thẻ đèn) | 25' |
| 3 | **Đặt ngưỡng** — mỗi đèn có nguồn và lý do | 30' ⭐ |
| 4 | **Viết 5 luật quyết định** — ≥2 luật dừng | 30' ⭐ |
| 5 | Cổng gác 90 ngày & ráp dashboard 1 trang | 20' |

Bấm giờ từng trạm. Hết giờ thì sang trạm sau, quay lại hoàn thiện ở Trạm 5.

## Tài liệu trong repo

| File | Đọc khi nào |
|---|---|
| [HANDBOOK.md](HANDBOOK.md) | Kiến thức nền, 3 bảng đèn B2C/B2B/B2B2C, prompt AI, nguồn số liệu. **Chỉ đọc phần §3 của loại mình.** |
| [CHECKPOINTS.md](CHECKPOINTS.md) | Hướng dẫn từng trạm: làm gì, ra sản phẩm gì, tự kiểm tra thế nào |
| [SUBMISSION.md](SUBMISSION.md) | Tên repo, file phải nộp, deadline, checklist trước khi nộp |
| [RUBRIC.md](RUBRIC.md) | Tiêu chí chấm 100 điểm và các điều kiện mất điểm |
| [RULES.md](RULES.md) | Quy định dùng AI, sao chép, nộp muộn, bảo mật số liệu |

> **Ba câu cần nhớ nếu quên hết mọi thứ khác:**
> **B2C** — đèn bật trước là đường cong retention có phẳng không.
> **B2B** — đèn bật trước là time-to-first-value.
> **B2B2C** — đèn bật trước là partner activation. Ký được không phải là thắng.
