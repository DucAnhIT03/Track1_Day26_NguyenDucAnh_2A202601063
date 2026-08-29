# Operating Dashboard — Oce

> Đây là **worksheet nguồn** để validator và rubric truy vết evidence. Sau khi
> hoàn tất, rút gọn phần vận hành sang
> `templates/one-page-dashboard-template.md`; không ép bảng 12 cột này lên một trang.

- Học viên: Nguyễn Đức Anh
- Mã học viên: 2A202601063
- Mô hình: B2B
- Cập nhật: 2026-08-29
- North Star: Time-to-first-value dưới 7 ngày

## Chẩn đoán mô hình

Chúng tôi là B2B vì tiền đến từ công ty dịch vụ kế toán (firm ký hợp đồng, giám đốc duyệt thanh toán $29/tháng + $6/report), người dùng thật là kế toán viên trong firm đó vận hành Oce trực tiếp qua web app, và chúng tôi có bề mặt trực tiếp với KTV qua PLG self-serve (KTV tự đăng ký, upload file, nhận báo cáo) nhưng không chạm được người dùng cuối là doanh nghiệp SME khách hàng của firm.

| Dữ liệu đầu vào | Trạng thái | Nằm ở đâu hoặc cần gì để đo | Ngày có số |
|---|---|---|---|
| Unit economics Day 24 | TRONG 2 TUẦN | Suy từ Day 25: ARPU $167, GM 75,1%, CAC PLG ước $300, CAC payback 2,4 tháng, LTV ước $1.755 (giả định vòng đời 14 tháng). File mô hình tài chính 24 tháng đang xây | 2026-09-15 |
| Value Metric và Cost/Job Day 25 | ĐO ĐƯỢC | File Oce_Day25_model.md: Cost/Job $1,845/completed; GM 75,1%; ARPU $167/firm/tháng; kênh PLG; giá $6/report + $29 base | 2026-08-28 |

## Kiểm kê đèn ứng viên

| Đèn ứng viên từ handbook | Tầng | Trạng thái | Bằng chứng hiện có hoặc kế hoạch đo |
|---|---|---|---|
| Time-to-first-value (TTFV) | L | 🔧 | Internal testing trên 200 bộ sổ mẫu: xử lý trung bình 2,5 phút/bộ. Ước TTFV 5 ngày (gồm signup + làm quen UI + upload + AI xử lý + KTV review). Event log sẵn sàng đo từ ngày go-live |
| Pipeline coverage | L | ❌ | Oce dùng PLG, không có pipeline sales truyền thống. Không áp dụng; thay bằng signup-to-activation |
| % deal chết ở khâu security/procurement | L | ❌ | PLG self-serve không có procurement process. Không áp dụng cho giai đoạn này |
| POC → paid | O | 🔧 | Tương đương free trial → paid. Cần billing system ghi nhận chuyển đổi. Dự kiến đo từ tháng 2 |
| Sales cycle (ngày) | O | ❌ | PLG không có sales cycle truyền thống. Thay bằng time-to-first-paid (signup → thanh toán đầu tiên) |
| Usage depth trong tài khoản | O | 🔧 | Cần event tracking: số KTV active / tổng KTV được invite trong mỗi firm. Thêm vào product analytics trước 2026-10-15 |
| Chi phí triển khai ÷ ACV | O | ✅ | Oce self-serve, chi phí triển khai gần $0. ACV = $167 × 12 = $2.004. Tỷ lệ < 1% |
| Tập trung doanh thu | O | 🔧 | Cần ≥5 firms paying. Billing export grouped theo firm. Dự kiến tháng 3 (2027-01-31) |
| NRR | G | ❌ | Cần ≥2 quý có khách trả phí. Sớm nhất 2027-06-30 |
| Gross Margin | G | ✅ | Tính được từ Day 25: 75,1%. Billing export + token cost log |
| CAC payback | G | ✅ | CAC PLG ước $300 (content marketing + freemium); ARPU $167, GM 75,1%, GP $125,48/firm/tháng → payback = $300 ÷ $125,48 = 2,4 tháng. Ad spend report xác nhận từ tháng 2 |

## Đèn báo sớm

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| L-01 | Time-to-first-value | Số ngày từ signup firm đến khi KTV upload bộ sổ đầu tiên VÀ nhận completed report đạt QA hoặc không cần chỉnh sửa trong 48h; median theo cohort tháng | Tuần · Founder | 5 ngày | ≤7 ngày | 8–14 ngày | >14 ngày | [TB] Ước từ internal testing trên 200 bộ sổ: AI xử lý 2,5 phút/bộ, KTV review trung bình 30 phút, thêm 4 ngày friction đăng ký và làm quen UI; đo 4 cohort rồi chốt baseline vào 2027-01-31 | 2026-08-29 | Trial-to-paid và retention | R-01 |
| L-02 | Activation rate | Số firm mới có ≥1 completed report trong 14 ngày đầu chia tổng firm đăng ký mới trong cùng cohort; không tính firm chỉ tạo tài khoản mà không upload | Tuần · Growth | 75% | ≥50% | 30–49% | <30% | [MH] MH-02 suy từ breakeven containment 42% của Day 25; activation ≥42% để GM ≥60%, đặt xanh từ 50% để có headroom 8pp; hiện tại 75% từ containment rate testing nội bộ 200 bộ sổ | 2026-08-29 | Trial-to-paid | R-02 |

## Đèn vận hành

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| O-01 | Trial-to-paid | Số firm dùng thử chuyển sang trả phí chia tổng firm kết thúc trial 14 ngày trong kỳ; chỉ tính firm đã activate (có ≥1 completed report) | Tháng · Growth | 45% | ≥50% | 35–49% | <35% | [BM] ICONIQ State of Go-to-Market 2026 https://www.iconiq.com/growth/reports/state-of-go-to-market-2026; POC/free-trial → paid ~50% (2026), tăng từ ~36% (2025); Oce ước 45% dựa trên giá trị tiết kiệm 53% so với lương KTV và trial ngắn 14 ngày | 2026-08-29 | MRR và gross margin | R-03 |
| O-02 | Chi phí AI trên mỗi completed report | Tổng chi phí token Haiku 4.5 + Sonnet 5 (bao gồm cache read/write) chia số report đạt QA hoặc được KTV chấp nhận không cần sửa trong 48h | Tuần · FinOps | $0,69 | ≤$0,69 | $0,70–$0,92 | >$0,92 | [MH] MH-01 suy từ GM mục tiêu 60% và giá bán $6/report; trần AI cost = $1,245/completed; đặt xanh tại $0,69 (hiện tại) và đỏ tại $0,92 (75% trần) để có thời gian phản ứng trước khi phá GM | 2026-08-29 | Gross margin | R-04 |
| O-03 | Usage depth trong firm | Số KTV trong firm có ≥1 completed report trong 7 ngày gần nhất chia tổng KTV đã được invite vào tài khoản firm đó; tính trung vị across firms | Tuần · Customer Success | 40% | ≥60% | 30–59% | <30% | [TB] Ước từ mô hình adoption B2B: firm trung bình 5 KTV, giai đoạn đầu 2/5 KTV dùng thử = 40%; đo 4 tuần sau khi có ≥3 firms paying rồi chốt baseline; ngưỡng 60% tham khảo handbook B2B §3.2 | 2026-08-29 | Retention và renewal | R-05 |

## Đèn kết quả

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| G-01 | Gross margin | (Tổng revenue − tổng COGS) ÷ tổng revenue; COGS gồm LLM token, infra, retry, QA nội bộ; không gồm overhead R&D/sales/admin | Tháng · Finance | 75,1% | ≥60% | 45–59% | <45% | [BM] ICONIQ State of AI 2026 https://www.iconiq.com/growth/reports/state-of-ai-2026; AI-native GM 45% (2025) → 53% (2026E); Benchmarkit SaaS trung vị 77%; mục tiêu Oce ≥60% nằm giữa AI-native và SaaS truyền thống | 2026-08-29 | Runway và khả năng gọi vốn | R-04 |
| G-02 | CAC payback | CAC chia gross profit trên mỗi firm mỗi tháng; CAC = tổng chi phí marketing và sales trong kỳ chia số firm mới trả phí | Quý · Founder | 2,4 tháng | <12 tháng | 12–18 tháng | >18 tháng | [BM] Bessemer Scaling to $100 Million https://www.bvp.com/atlas/scaling-to-100-million; SMB payback <12 tháng. CAC PLG $300 (content marketing + freemium conversion) ÷ GP $125,48/firm/tháng = 2,4 tháng | 2026-08-29 | Khả năng scale acquisition | R-03 |

## Luật quyết định

| ID | NẾU | TRONG | VÀ | THÌ | KHÔNG THÌ | Luật dừng? |
|---|---|---|---|---|---|---|
| R-01 | Median TTFV >14 ngày | 2 cohort tháng liên tiếp | Mỗi cohort có ≥5 firm mới signup | Đóng băng marketing acquisition trong 14 ngày và founder dành toàn bộ thời gian làm onboarding tay cho 3 firm tiếp theo để xác định bottleneck cụ thể | Không đổ thêm tiền quảng cáo hoặc mở rộng free tier để bù chậm thấy giá trị | CÓ |
| R-02 | Activation rate <30% | 3 tuần liên tiếp | Có ≥10 firm mới đăng ký tổng trong 3 tuần đó | Rút gọn trải nghiệm trial còn đúng 1 workflow (phân loại giao dịch) và thêm guided onboarding bằng video walkthrough trong 1 sprint | Không mở rộng tính năng free tier hoặc thêm workflow mới để tăng signup | KHÔNG |
| R-03 | Trial-to-paid <35% | 2 tháng liên tiếp | Có ≥15 trial kết thúc tổng trong 2 tháng đó | Đóng băng chi tiêu acquisition trong 2 tuần và phỏng vấn 5 firm churned gần nhất để lập danh sách 3 lý do chính không chuyển đổi | Không giảm giá để tăng conversion vì vấn đề nằm ở giá trị cảm nhận không nằm ở mức giá | CÓ |
| R-04 | AI cost/completed report >$0,92 | 2 tuần liên tiếp | Có ≥100 completed reports đạt QA trong 2 tuần đó | Giới hạn context window cho phase 1–3 xuống 3.000 token và chuyển phase 4 (phát hiện bất thường) từ Sonnet 5 sang Haiku 4.5 cho các bộ sổ dưới 200 giao dịch | Không bỏ bước QA spot check hoặc hạ tiêu chuẩn QA để làm cost/report trông thấp hơn | KHÔNG |
| R-05 | Usage depth <30% | Sau 60 ngày kể từ firm bắt đầu trả phí | Firm đó có ≥3 KTV đã được invite vào tài khoản | Cử founder shadow 3 phiên làm việc thật với KTV chưa dùng Oce trong firm đó và xây 1 template sổ sách theo đúng ngành của firm trong 1 sprint | Không bán thêm gói hoặc đề xuất upgrade cho firm đó vì firm chưa dùng hết sẽ churn | KHÔNG |

## Cổng gác 90 ngày

| Ngày | Metric gác cổng | Ngưỡng | Bằng chứng vật lý | Nếu đạt | Nếu trượt |
|---:|---|---|---|---|---|
| 30 | Eval accuracy trên data khách thật | Phân loại đúng ≥85% và đối soát match ≥90% trên ≥3 bộ sổ thật từ 3 firm khác nhau | File eval report có confusion matrix và sample size cho từng firm | GO | FIX |
| 60 | Trial-to-paid | ≥35% trên ≥10 firm kết thúc trial | Billing export với timestamp signup và timestamp thanh toán đầu tiên cho từng firm | GO | PIVOT |
| 90 | Gross margin sau AI cost | ≥45% với ≥150 completed reports tổng | Billing export ghép với token cost report từ Anthropic API dashboard | GO | KILL |

## Kill criteria

KILL hướng sản phẩm vào ngày 90 nếu gross margin vẫn dưới 45% sau hai vòng tối ưu model (giảm context, đổi model tier) VÀ không có firm nào chấp nhận mức giá sàn $5,54/report tính từ 3× Cost/Job Day 25.

## Chưa đo được

| Đèn hoặc giả định | Cần gì để đo | Ai chịu trách nhiệm | Ngày có số |
|---|---|---|---|
| Containment rate trên data khách thật — hiện 75% trên 200 bộ sổ mẫu nội bộ, cần xác nhận trên data production | Eval trên data thật từ ≥3 firm với confusion matrix và human review để xác nhận con số 75% | Founder | 2026-10-31 |
| Retention M6 — giả định vòng đời 14 tháng từ benchmark FinTech B2C, cần xác nhận bằng cohort thật | Đo M3 và M6 retention trên 2 cohort quý đầu tiên có khách trả phí | Finance | 2027-06-30 |
| p95 cost/report — trung bình $0,69 nhưng phân phối chi phí theo bộ sổ có thể lệch do bộ sổ phức tạp | Logging chi tiết token consumption per job gồm input/output/cache cho từng phase 1–6 | FinOps | 2026-10-15 |

## Phụ lục ngưỡng suy từ mô hình

| ID | Metric | Input Day 24–25 | Phép tính | Kết quả và ngưỡng áp dụng |
|---|---|---|---|---|
| MH-01 | AI cost tối đa mỗi completed report | Giá bán $6/report (Day 25); GM mục tiêu tối thiểu 60%; non-AI COGS per completed = ($0,767 infra + $0,063 QA + $0,036 retry) ÷ 75% containment = $1,155 | $6 × (1 − 0,60) − $1,155 = $2,40 − $1,155 = $1,245 max total variable cost per completed; AI cost max = $1,245 − $0 (không có chi phí biến đổi khác) = $1,245; hiện tại LLM $0,518/attempted ÷ 75% = $0,691/completed | Xanh khi AI cost ≤$0,69/completed (hiện tại); đỏ trên $0,92 (75% trần $1,245) để có buffer phản ứng trước khi phá GM; áp dụng cho O-02 |
| MH-02 | Activation rate tối thiểu để breakeven GM 60% | Revenue per firm = $29 + 30 × r × $6 (r = tỷ lệ job thành công = containment = activation proxy); COGS ≈ $41,52/firm/tháng cố định (Day 25: 300 jobs × $1,384) | ($29 + $180r − $41,52) ÷ ($29 + $180r) ≥ 0,60 → $180r − $12,52 ≥ $17,40 + $108r → $72r ≥ $29,92 → r ≥ 0,416 = 41,6% | Activation/containment phải ≥42% để GM ≥60%; xanh từ 50% (headroom 8pp); đỏ dưới 30% (không còn đường breakeven kể cả khi recovery); áp dụng cho L-02 |

## Ghi nhận AI critique

| Phản biện | Chấp nhận hay bác bỏ | Thay đổi đã thực hiện | Lý do |
|---|---|---|---|
| TTFV cần định nghĩa chặt hơn vì hai người có thể đo khác nhau nếu không thống nhất thế nào là completed report | Chấp nhận | Chốt định nghĩa completed = report đạt QA spot check HOẶC KTV không yêu cầu chỉnh sửa trong 48h sau khi nhận | Hai tiêu chí rõ ràng cho phép đo tự động bằng event log mà không cần phán đoán chủ quan |
| Pipeline coverage không áp dụng cho PLG nhưng cần một đèn leading thay thế đo funnel phía trên activation | Chấp nhận | Thay pipeline coverage bằng activation rate (L-02) đo % firm có ≥1 completed report trong 14 ngày đầu | PLG funnel đo signup → activation → paid; activation rate là đèn leading tự nhiên nhất cho PLG B2B |
| Trial-to-paid 50% có thể quá cao vì ICONIQ benchmark là cho B2B có sales assist còn Oce là pure PLG self-serve | Bác bỏ | Giữ ngưỡng xanh 50% nhưng đặt đỏ tại 35% thay vì thấp hơn | Oce tuy PLG nhưng ACV thấp ($2.004/năm) và trial ngắn (14 ngày) nên friction chuyển đổi thấp hơn enterprise B2B; 50% là tham vọng hợp lý không phải mục tiêu mặc định |
