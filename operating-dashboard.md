# Operating Dashboard — CodeReview AI

<!--
KHUNG B2B/B2D — Day 26 Track 1: AI Product Handbook
Cách dùng:
  1. Thay TẤT CẢ placeholder <...> bằng nội dung thật. Còn <...> là validator FAIL.
  2. Ngày ghi theo ISO: YYYY-MM-DD.
  3. Nguồn ngưỡng: [BM] benchmark có URL+ngày | [MH] suy từ mô hình có phép tính | [TB] baseline tự đo.
  4. Chỉ sửa file này. KHÔNG sửa lab.config.json / validator / tests / example.
  5. Chạy: python3 scripts/validate_submission.py submissions/<MÃ-HỌC-VIÊN>/operating-dashboard.md
LƯU Ý: validator bám cấu trúc template gốc trong repo — nếu FAIL vì tên section,
hãy đối chiếu tiêu đề với templates/operating-dashboard-template.md.
-->

## Metadata

- Học viên: Trần Dương Tuấn
- Mã HV: 2A202601271
- Sản phẩm: CodeReview AI
- Loại mô hình: B2B
- Ngày cập nhật: 2026-08-28

## Chẩn đoán mô hình

Chúng tôi là B2B vì tiền đến từ ngân sách kỹ thuật/DevTools của các công ty công nghệ (SMB/Mid-market 50–500 nhân sự) và subscription $12/seat/tháng, người dùng thật là Software Engineers và Tech Leads, và chúng tôi có bề mặt trực tiếp với người dùng cuối qua GitHub App (PR bot) và Web Dashboard.

## North Star

- North Star: Weekly Active Reviewed PRs (WARP) — Số lượng Pull Requests được AI phân tích và có ít nhất 1 gợi ý có giá trị được developer chấp thuận mỗi tuần / Active Team.
- Vì sao chọn: Metric này phản ánh trực tiếp giá trị cốt lõi: CodeReview AI chỉ thực sự mang lại hiệu quả khi phát hiện đúng bug/code smell và được kỹ sư chấp nhận vào luồng code chính. Khi WARP tăng, năng suất review tăng từ 8-15 PRs/tuần, củng cố sự gắn kết (retention) và thúc đẩy mở rộng số seat trả phí trong tổ chức.

## Bảng đèn (6–8 metric)

| ID | Metric & định nghĩa | Công thức | Nhịp đo | Hiện tại | 🟢 Xanh | 🟡 Vàng | 🔴 Đỏ | Nguồn | Báo trước cho | Luật |
|----|----|----|----|----|----|----|----|----|----|----|
| L-01 | PR Review Acceptance Rate — % PRs có gợi ý của AI được developer chấp thuận | Gợi ý AI được accept / Tổng PRs AI review | Tuần | 68% | ≥ 70% | 55%–69% | < 55% | [BM] Benchmark CodeRabbit & GitHub Copilot PR acceptance (Lenny's Newsletter 2024-04-15: https://www.lennysnewsletter.com/p/what-is-a-good-activation-rate) | D14 & D30 Retention | R-01 |
| L-02 | D14 Team Retention — % teams/repos tiếp tục dùng bot sau 14 ngày | Active teams D14 / Tổng teams cài đặt D0 | Tuần | 42% | ≥ 45% | 30%–44% | < 30% | [BM] PLG DevTools SaaS Benchmark (OpenView Product-Led SaaS Benchmarks 2024-03: https://openviewpartners.com/benchmarks) | LTV & CAC Payback | R-02 |
| L-03 | Cost/Job AI — Chi phí inference + infra trung bình cho 1 PR hoàn thành | Tổng chi phí vận hành $1.230 / Số PR hoàn tất (380) | Tuần | $3.24 | ≤ $3.50 | $3.51–$4.80 | > $4.80 | [MH] Phép tính Unit Economics [MH] 1 từ File GTM-Model.xlsx | Gross Margin ≥ 60% | R-03 |
| O-01 | Success / Containment Rate — % PR xử lý thành công không lỗi timeout | Completed Jobs (380) / Attempted Jobs (500) | Tuần | 76% | ≥ 75% | 60%–74% | < 60% | [TB] Đo lường thực nghiệm 500 PRs từ File Day25-AI-CodeReview-One-Pager.docx | Cost/Job L-03 | R-04 |
| O-02 | Turnaround Time P95 — Thời gian từ lúc mở PR đến khi bot post comment | P95 thời gian xử lý webhook PR đến khi comment (phút) | Tuần | 3.2 phút | ≤ 4.0 phút | 4.1–7.0 phút | > 7.0 phút | [TB] Baseline APM log GitHub Webhook handler đo tháng 2026-08 | Developer Adoption | R-05 |
| G-01 | Gross Margin % — Biên lợi nhuận gộp toàn hệ thống | (Doanh thu $12.00 − Cost/Job $3.24) / $12.00 | Tháng | 73% | ≥ 70% | 60%–69% | < 60% | [MH] Phép tính Gross Margin [MH] 1 | — | — |
| G-02 | CAC Payback Period — Thời gian thu hồi chi phí chuyển đổi team trả phí | CAC $1.053 / (ARPU team/tháng × Gross Margin %) | Tháng | 4.2 tháng | ≤ 5.0 tháng | 5.1–8.0 tháng | > 8.0 tháng | [MH] Phép tính CAC Payback [MH] 2 | — | — |
| G-03 | MRR — Doanh thu định kỳ hàng tháng từ subscription & usage | (Paid Seats × $12) + (Overage PRs × $0.50) | Tháng | $1,440 | ≥ $1,800 | $1,200–$1,799 | < $1,200 | [BM] Seed DevTools SaaS Target (OpenView GTM Benchmarks 2024-03: https://openviewpartners.com/benchmarks) | — | — |

## Phụ lục [MH] — phép tính suy từ mô hình

### [MH] 1 — Ngưỡng đỏ Cost/Job và Điểm hòa vốn Containment

```
Giá gói cơ sở đề xuất (Price)  = $12.00 / seat / tháng
Gross margin mục tiêu tối thiểu = 60%
Tổng chi phí biến đổi tối đa   = $12.00 × (1 − 0.60) = $4.80 / job   ← ngưỡng đỏ L-03
Chi phí thực tế hiện tại (380/500 jobs completed):
- API LLM (Claude 3.5 Sonnet)  = $800 / 380 = $2.11 / job
- AWS Infra (2x t4g.medium+S3) = $200 / 380 = $0.53 / job
- Retry buffer (10%)           = $80 / 380 = $0.21 / job
- Overhead DevOps (20%)        = $150 / 380 = $0.39 / job
→ Tổng Cost/Job hiện tại       = $1.230 / 380 = $3.24 / job (Gross Margin = 73%)
→ Điểm hòa vốn Containment     = $1.230 / ($4.80 × 500) = 51,25% (Làm tròn an toàn 60%)
```

### [MH] 2 — Ngưỡng CAC Payback từ mô hình SMB GTM

```
CAC trung bình cho 1 team SMB   = $1.053 (kênh PLG + Content GitHub)
Quy mô team trung bình          = 25 seats
ARPU tháng của team             = 25 seats × $12.00 + 100 overage PRs × $0.50 = $350.00 / tháng
Lợi nhuận gộp ròng mỗi tháng    = $350.00 × 73% = $255.50 / tháng
→ CAC Payback thực tế           = $1.053 / $255.50 = 4,12 tháng (làm tròn 4.2 tháng)
Ngưỡng đỏ CAC Payback tối đa    = 8.0 tháng
→ Tương ứng tỷ lệ Logo Churn tối đa cho phép = 1 / 8.0 = 12,5% / tháng
```

## Luật quyết định

- **R-01** — NẾU L-01 (PR Acceptance Rate) < 55% TRONG 2 tuần liên tiếp VÀ tổng số PR review ≥ 200 PRs THÌ đóng băng việc mở rộng sang các repo/team mới và chuyển toàn bộ kỹ sư sang tối ưu lại prompt AST filtering giảm 50% false positives KHÔNG THÌ cấm đẩy tính năng tự động comment code ra production. *(luật dừng)*
- **R-02** — NẾU L-02 (D14 Team Retention) < 30% TRONG 3 tuần liên tiếp VÀ số repo kích hoạt ≥ 15 repos THÌ kích hoạt quy trình 1-on-1 interview trực tiếp với Tech Lead/Reviewer để xác định lý do tắt bot và điều chỉnh độ nhạy rule-set KHÔNG THÌ cấm tự động gửi email marketing thúc ép nâng cấp gói trả phí.
- **R-03** — NẾU L-03 (Cost/Job AI) > $4.80/job TRONG 1 tuần VÀ số PR xử lý ≥ 100 PRs THÌ giới hạn độ dài context tối đa 800 dòng diff code và tự động định tuyến các PR dưới 50 dòng sang Claude 3.5 Haiku thay vì Sonnet KHÔNG THÌ cấm bù lỗ hạ tầng bằng cách tăng giá gói seat đột ngột. *(luật dừng)*
- **R-04** — NẾU O-01 (Success / Containment Rate) < 60% TRONG 1 tuần VÀ tổng PR attempts ≥ 100 PRs THÌ chuyển hướng tự động fallback sang static analysis (linter) cho các file diff quá lớn (> 2.000 dòng) và kích hoạt alert khẩn cấp cho đội ngũ DevOps KHÔNG THÌ cấm tăng giới hạn kích thước PR tiếp nhận. *(luật dừng)*
- **R-05** — NẾU O-02 (Turnaround Time P95) > 7.0 phút TRONG 3 ngày liên tiếp VÀ concurrent PRs ≥ 20 jobs THÌ kích hoạt auto-scaling mở rộng cụm worker parser song song trên AWS ECS Fargate và tối ưu hóa hàng đợi webhook Redis KHÔNG THÌ cấm bổ sung các model phân tích bảo mật chuyên sâu nặng nề vào luồng review tức thì.

## Cổng gác 90 ngày

| Cổng | Metric gác cổng | Ngưỡng (số) | Bằng chứng vật lý | Quyết định |
|------|----|----|----|----|
| Ngày 30 | Pilot Engagement & Containment Rate | ≥ 3 pilot customers hoàn tất ≥ 500 PRs VÀ Containment Rate ≥ 70% | Báo cáo Pilot Report & GitHub App Telemetry Analytics Log | GO |
| Ngày 60 | Paying Customers & Cost/Job | ≥ 10 paying teams ($1.2K MRR) VÀ Cost/Job ≤ $3.50 | Stripe Billing Dashboard & AWS Cost Explorer Invoice Report | GO |
| Ngày 90 | MRR & CAC Payback Period | MRR ≥ $1,800 VÀ CAC Payback ≤ 5.0 tháng VÀ Gross Margin ≥ 70% | Báo cáo tài chính P&L & HubSpot CRM Customer Acquisition Report | GO |

## Kill criteria

- KILL nếu CAC Payback Period vẫn > 8.0 tháng hoặc MRR vẫn < $1,200 sau Ngày 90 dù đã thực hiện FIX prompt AST filtering và điều chỉnh hybrid pricing 1 lần ở Ngày 60.

## Chưa đo được

- Tỷ lệ phát hiện lỗi bảo mật nghiêm trọng (CVE Detection Rate) — cách đo: Chạy benchmark đối chiếu tự động 1.000 lỗ hổng đã biết trên repo test benchmark Semgrep/Snyk — ngày dự kiến có số: 2026-09-30.
- Tỷ lệ chuyển đổi tự nhiên từ Free/Pilot sang Paid qua GitHub Marketplace — cách đo: Thiết lập funnel tracking Mixpanel gắn GitHub Organization ID — ngày dự kiến có số: 2026-09-15.
