# Worksheet — AI dựng concept nội thất cho Freelance Interior Designer

Họ tên: Phan Duy Thành · MSSV: 2A202602930 · Ngày làm: 09/10/2026

Sản phẩm (tiếp nối Lab20 Metrics Pack và Lab22 Monetization Model): designer chụp ảnh hiện trạng phòng, AI dựng ≥2 góc phối cảnh 2K kèm moodboard vật liệu, xuất **Design Pack** gửi chủ nhà qua Zalo ngay trong buổi tư vấn đầu tiên, thay cho 3 ngày dựng 3ds Max.

Trạng thái hôm nay: **chưa có dữ liệu thật**. Pilot 10 designer bắt đầu **13/10/2026**, nên mọi số "hiện tại" đều là 🔧 hoặc ❌.

## Số liệu đầu vào — Mô hình tài chính và Value Metric

Nguồn: file `Day25-AI-Product-GTM-Monetization-Model.xlsx` của Lab22 (ô Excel ghi ở cột cuối). Số **chưa scale**; đây là số mô hình, không phải số công ty mật.

| Chỉ số | Giá trị | Ghi chú / nguồn |
|---|---:|---|
| Giá gói | 499.000 ₫/tháng gồm 4 Design Pack; vượt gói 129.000 ₫/pack | `3_Value_Metric!B32` |
| ARPU | **$19,12/tháng** (499.000 ₫ ÷ 26.100) | `4_Channel_Fit!B5` |
| Gross margin | **75,8%** (51% nếu tính overhead $250/tháng) | `2_Pricing!B21` |
| CAC | **chưa đo** (PLG, chưa chạy). Ngân sách CAC tối đa của Lab22: **$173,8** | `4_Channel_Fit!B9` = ARPU × GM × 12 |
| CAC payback mục tiêu | **12 tháng** (Bessemer, SMB) → siết lại còn **7,5 tháng** ở [MH-5] | `4_Channel_Fit!B8` |
| Runway | **18 tháng** | số tự khai |
| Value Metric | **Hybrid**: gói tháng + Design Pack (1 job = 1 Design Pack đạt chuẩn của 1 phòng) | `3_Value_Metric!B30` |
| Cost/Job | **$1,16/pack** (≈ 30.239 ₫); có overhead: $2,35 | `1_Cost_Job!B66`, `B67` |
| Thành phần Cost/Job | 5 render/pack · $0,103/render (ảnh 2K Gemini 3.1 Flash Image) · retry 10% · infra $0,232/phòng · QA $0,0125/phòng · containment 70% (ước tính) | `1_Cost_Job!B19…B47` |
| Giá hiệu dụng / pack | $4,78 | `2_Pricing!B19` |
| Breakeven containment (GM 60%) | 42,4% | `2_Pricing!B33` |
| Số pack / designer / tháng | 3,5 | `2_Pricing!B11` |

> Ghi chú sửa số: bản nháp trước ghi ARPU $20, GM 70%, CAC $50, payback 5 tháng và value metric "lượt gửi tin nhắn". Các số này không khớp với sản phẩm và không truy được về file mô hình, nên đã thay bằng số Lab22. Riêng runway 18 tháng giữ nguyên.

---

## Trạm 1 — Loại mô hình

**Ba câu hỏi (theo thực tế hôm nay):**

1. *Ai trả tiền?* Freelance interior designer. Họ tự đăng ký và tự trả 499.000 ₫/tháng bằng tiền cá nhân qua luồng tự phục vụ (PLG). Không có hợp đồng doanh nghiệp, không qua bộ phận mua hàng.
2. *Ai dùng?* Cũng chính designer đó: chụp ảnh, chỉnh concept, bấm Xuất.
3. *Có bên trung gian không?* Không. Chủ nhà chỉ **xem** Design Pack qua link Zalo designer gửi. Họ không đăng nhập, không trả tiền và không phải người dùng của mình.

**Câu chốt loại:** Chúng tôi là **B2C** vì người trả 499.000 ₫/tháng và người dùng là **cùng một người**: freelance designer tự đăng ký, tự trả bằng tiền cá nhân qua Zalo Mini App, không có hợp đồng công ty. Chủ nhà chỉ xem Design Pack qua link, nên đây không phải B2B2C. Mình cũng không bán cho studio hay doanh nghiệp, nên không phải B2B. Gói Team cho studio là kế hoạch tháng 4+, không phải thực tế hôm nay.

**Bảng đèn §3.1 B2C** (đủ 8 đèn trong sổ tay, cộng 4 đèn riêng của sản phẩm):

| # | Đèn (HANDBOOK §3.1) | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|---|
| 1 | Đường cong retention có phẳng không ⭐ | 🔧 | Event `design_concept_exported` đã thiết kế ở Lab20 nhưng chưa chạy production. Cần bật event từ ngày pilot 13/10 và gắn `plan_status` (trial/paid). Điểm M1 đầu tiên khoảng 12/12/2026; muốn thấy hình dạng cong thì phải có ≥2 cohort |
| 2 | Activation rate | 🔧 | Cần `account_created` và `design_concept_exported` đầu tiên trong 48h (định nghĩa ở Lab20). Đo được từ 13/10; baseline sau 2 cohort, khoảng 14/12/2026 |
| 3 | p95 cost/user/tháng ÷ ARPU | 🔧 | Hiện chỉ có Cost/Job **ước tính**. Cần log `usage_metadata` (token) của mỗi lần gọi Gemini theo `render_id` → `user_id`, cộng thêm doanh thu thực thu theo user. Dự kiến có trước 27/10/2026 |
| 4 | Trial → paid | 🔧 | Luồng "dùng thử 2 pack" trên Zalo Mini App chỉ có từ tháng 2 (Lab22 tab 5). Tháng 1 thì đếm tay trên 10 designer pilot. Cần event `trial_started` và `subscription_paid` |
| 5 | Retention M12 | ❌ | Về bản chất phải 12 tháng sau cohort đầu (≈ 11/2027) mới có số. Không có cách đo sớm hơn |
| 6 | Chi phí free tier ÷ tổng COGS | 🔧 | Cần gắn `plan_status` vào mọi `ai_render_completed` để tách token của trial. Đưa vào cùng đợt log chi phí ở #3 |
| 7 | Tỷ lệ refund | ❌ | Chưa có cổng thanh toán (pilot thu chuyển khoản tay) và **chưa có chính sách hoàn tiền**. Phải chốt chính sách trước khi đo được |
| 8 | LTV/CAC · CAC payback · GM | 🔧 / ❌ | GM: hiện chỉ có số mô hình 75,8%, số thật cần log chi phí ở #3. CAC: cần sổ chi tiêu theo kênh và log giờ onboarding tay. LTV: ❌ vì phụ thuộc #1 |
| 9 | *(thêm)* Render trung bình / phòng | 🔧 | Đếm `ai_render_completed` theo `room_id` (event Lab20). Là đèn chi phí AI đổi theo ngày |
| 10 | *(thêm)* Containment: phòng ra pack đạt chuẩn | 🔧 | `design_concept_exported` ÷ `room_photo_uploaded`. Đo thật bằng eval 50 phòng, hạn 31/10/2026 |
| 11 | *(thêm)* Multi-room project rate (Lab20) | 🔧 | Đếm `room_id` theo `project_id` trong tuần đầu |
| 12 | *(thêm)* Regeneration rate >5 render/phòng (counter-metric Lab20) | 🔧 | Cùng nguồn với #9 |

---

## Trạm 2 — Thẻ đèn

**North Star:** *Tỷ lệ giữ chân tháng-qua-tháng của designer trả phí*, đọc trên đường cong cohort M1→M3.
**Hiện tại:** chưa có; điểm đầu tiên khoảng 12/12/2026. **Mục tiêu:** ≥ 91,7%/tháng và đường cong phẳng dần.

> Vì sao không lấy MQEC (NSM ở Lab20)? MQEC vẫn tăng được khi designer mới vào bù cho designer rời đi. Với sản phẩm AI B2C, mỗi designer ở lại phải tự trả tiền render của mình. Đường cong không phẳng thì LTV trong Lab22 là số tự nghĩ ra. MQEC vẫn được theo dõi như số đếm khối lượng, nhưng không phải đèn để lái.

| # | Tầng | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | **L** | Activation 48h | Đếm designer mới có **`design_concept_exported` đạt chuẩn đầu tiên** (≥2 góc 2K + moodboard có mã vật liệu) trong 48h kể từ `account_created`. **Không** đếm: tài khoản nội bộ/test, pack xuất từ dự án mẫu do onboarding tạo sẵn, xuất lỗi/timeout | Số designer có export đạt chuẩn ≤48h ÷ số `account_created` của cohort tuần | Xem hằng ngày, chốt theo tuần (cohort tuần) · Thành, query event | Trial→paid (#6) → Retention (#2) |
| 2 | **L** ⭐ | Retention tháng-qua-tháng của designer trả phí (đường cong cohort) | Cohort = tháng designer **thanh toán lần đầu**. Designer được tính "còn" ở tháng Mk khi **vừa còn trả phí vừa có ≥1 `design_concept_exported` đạt chuẩn** trong tháng đó. **Không** đếm: tài khoản test, designer đã hoàn tiền trong 7 ngày đầu (loại khỏi cohort), xuất lại cùng `design_id`, người trả phí nhưng không xuất pack nào | S(Mk) ÷ S(Mk−1), với S = % cohort còn lại; vẽ M0→M3 | Theo tháng, xem lại mỗi tuần · Thành | CAC payback (#7), LTV |
| 3 | **L** · AI cost | Render trung bình / phòng | Số `ai_render_completed` (render trả ảnh thành công, **gồm** cả phòng không xuất được) ÷ số phòng bắt đầu trong tuần. **Không** đếm: render của tài khoản nội bộ/QA, ảnh 1K nháp nếu đã tách giá riêng (ghi rõ khi bật), render bị lỗi API (đã nằm trong retry 10%) | Σ `ai_render_completed` ÷ Σ `room_photo_uploaded` (theo tuần tạo phòng) | Hằng ngày · log tự động | p95 cost ÷ doanh thu (#4) → GM |
| 4 | **O** · AI cost | p95 chi phí AI / designer / tháng ÷ doanh thu của designer đó | Lấy designer trả phí ở **phân vị 95** về chi phí API (ảnh + LLM + retry) trong tháng, chia cho doanh thu thực thu từ chính người đó (gói + vượt gói). **Không** đếm: user trial (chi phí trial nằm trong CAC #7), tài khoản nội bộ. **Không** dùng trung bình | cost_p95 ÷ revenue của cùng designer | Hằng tháng · log token theo `render_id` | GM · CAC payback (#7) |
| 5 | **O** | Containment | Phòng bắt đầu trong tuần T có **`design_concept_exported` đạt chuẩn trong vòng 7 ngày**. **Không** đếm: phòng của tài khoản test, phòng mẫu onboarding, phòng bị xoá trong 10 phút đầu (tải nhầm ảnh) | Số phòng có pack đạt chuẩn ≤7 ngày ÷ số `room_photo_uploaded` | Hằng tuần (trễ 7 ngày) · Thành | Activation (#1), Retention (#2), GM |
| 6 | **O** | Trial → paid | Người dùng thử được tính "chuyển" khi **thanh toán gói 499k trong 14 ngày** kể từ `trial_started`. **Không** đếm: người được tặng gói (đối tác, bạn bè), người trả rồi hoàn tiền trong 7 ngày | Số `subscription_paid` ≤14 ngày ÷ số `trial_started` của cohort | Hằng tháng (cohort 2 tuần) · Thành | Retention (#2) → CAC payback (#7) |
| 7 | **G** | CAC payback có tính churn | CAC = (chi tiêu thu hút + **chi phí API của mọi người dùng thử** + giờ onboarding tay × $5/h) ÷ số designer trả phí mới trong quý. Payback = số tháng đến khi **lãi gộp cộng dồn, đã nhân với tỷ lệ còn lại**, bù được CAC. **Không** dùng công thức không churn của Lab22 | Tháng m nhỏ nhất sao cho $14,49 × Σₖ₌₀^{m−1} S(Mk) ≥ CAC | Hằng quý · Thành | — (đèn bảng điểm) |

Kiểm tra cấu trúc: **3 đèn L** (#1, #2, #3) · **3 đèn O** · **1 đèn G** (14%, dưới mức trần 50%). **Đèn chi phí AI là #3 và #4**: #3 bắt sớm theo ngày, #4 bắt đuôi p95 theo tháng.

Đèn đã bỏ: *Retention M12* (❌, 12 tháng mới có số, và #2 đã báo trước cho nó). *Refund* (chưa có cổng thanh toán). *Chi phí free tier ÷ COGS* (gộp vào CAC #7 vì trial chỉ có 2 pack, có trần). *Multi-room* và *Regeneration* (trùng vai trò với #1 và #3).

---

## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | Activation 48h | ≥ 60% | 30–59% | < 30% | [TB] | Chưa có chuẩn: đo 2 cohort (pilot 13/10 và cohort tự phục vụ tháng 2), có baseline khoảng **14/12/2026**, sau đó đổi thành 🟢 ≥ baseline × 1,3 · 🔴 < baseline. Trước đó tạm dùng KPI tháng 1 đã cam kết (6/10, `5_90Day_Plan!B17`). 🔴 < 30% vì với mẫu 10 người, kết quả 2/10 cho cận trên Wilson 95% ≈ 51% < 60%, tức trượt thật chứ không phải nhiễu (3/10 vẫn chạm 60%) |
| 2 | Retention tháng-qua-tháng (designer trả phí) | ≥ 91,7% | 87,8–91,7% | < 87,8% | [MH] + [BM] | 91,7% là mức để designer ở lại trung bình ≥12 tháng, đủ hoàn vốn ngân sách CAC $174 ([MH-4]). 87,8% là mức suy ra từ retention M12 trung vị của app AI là 21,1% (RevenueCat qua TechCrunch 10/03/2026, **kiểm tra 09/10/2026**); thấp hơn mức này thì LTV < $119 |
| 3 | Render trung bình / phòng | ≤ 6,7 | 6,8–9,6 | > 9,6 | [MH] | Với giá $4,78/pack và containment 70%: > 9,6 render/phòng thì GM < 60% (mục tiêu GM của Lab22); ≤ 6,7 giữ GM ≥ 70% ([MH-1]) |
| 4 | p95 chi phí AI ÷ doanh thu designer | ≤ 0,24 | 0,24–0,48 | > 0,48 | [MH] | 0,24 = chi phí của designer dùng đủ 4 pack ở 5 render/pack. 0,48 = mức tối đa mà trần 12 render/pack cho phép. Vượt 0,48 nghĩa là đuôi đang **vượt mọi giới hạn sản phẩm đã thiết kế** ([MH-3]) |
| 5 | Containment | ≥ 70% | 42,4–69% | < 42,4% | [MH] | Dưới 42,4% thì GM < 60% theo công thức breakeven `R ≥ (v+q+e)/(P(1−GM)+e)`; 70% là giả định của mô hình Lab22, dưới mức đó thì Cost/Job $1,16 sai ([MH-2]) |
| 6 | Trial → paid | ≥ 8,5% | 5,6–8,4% | < 5,6% | [BM] + [MH] | 8,5% là trung vị app AI, 5,6% là trung vị app không-AI (RevenueCat qua TechCrunch 10/03/2026, **kiểm tra 09/10/2026**). Dưới 5,84% thì riêng chi phí 2 pack dùng thử cho mỗi khách trả phí đã vượt trần CAC $39,7 ([MH-4]). Kế hoạch Lab22 giả định 15%, tức gấp 1,8 lần trung vị: chưa có lý do, nên **không** dùng làm ngưỡng |
| 7 | CAC payback có tính churn | ≤ 7,5 tháng | 7,5–12 tháng | > 12 tháng hoặc không bao giờ hoàn vốn | [MH] + [BM] | 7,5 = (runway 18 − 3 tháng chưa có khách trả phí) ÷ 2 vòng hoàn vốn ([MH-5]); 12 tháng là trần payback SMB của Bessemer (qua HANDBOOK §8.2 mục 11, sổ tay kiểm tra 27/08/2026; **mình đối chiếu lại 09/10/2026**, chưa mở bản gốc) |

### Phụ lục [MH] — phép tính

**[MH-1] Render trung bình / phòng → GM** (đèn #3)

```
Đầu vào (1_Cost_Job, 2_Pricing):
  LLM ảnh / job = $0,515 cho 5 render  → $0,103 / render
  Retry 10% của LLM                     → $0,103 × 1,1 = $0,1133 / render
  Chi phí cố định / phòng = infra $0,232 + QA $0,0125 = $0,2445
  Containment R = 70% · Giá / pack P = $4,78
Phép tính:
  Cost/Job(n) = (0,1133 × n + 0,2445) ÷ 0,7       (kiểm tra n = 5 → $1,1586 ✓ khớp B66)
  GM ≥ 70% ⇔ Cost ≤ 0,30 × 4,78 = $1,434 ⇔ 0,1133n ≤ 1,434×0,7 − 0,2445 = 0,7593 ⇔ n ≤ 6,70
  GM ≥ 60% ⇔ Cost ≤ 0,40 × 4,78 = $1,912 ⇔ 0,1133n ≤ 1,912×0,7 − 0,2445 = 1,0939 ⇔ n ≤ 9,65
  (Đối chiếu: n ≥ 13 → GM < 50%, khớp kết luận "mô hình gãy" của Lab22)
Kết quả → 🟢 ≤ 6,7 · 🟡 6,8–9,6 · 🔴 > 9,6 render/phòng
```

**[MH-2] Containment tối thiểu** (đèn #5)

```
Đầu vào: v = $0,7985 / phòng thử · q = $0,0125 · P = $4,78 · GM mục tiêu = 60%
Biến thể A (designer tự xử lý ca AI trượt, e = 0):
  R ≥ (v + q + e) ÷ (P × (1 − GM) + e) = 0,811 ÷ 1,912 = 42,4%
Nếu đổi sang Biến thể B (mình dựng tay hộ: 45 phút × $5/h → e = $3,75):
  R ≥ (0,811 + 3,75) ÷ (1,912 + 3,75) = 4,561 ÷ 5,662 = 80,6%   ← dùng cho vế KHÔNG THÌ của Luật 5
Kết quả → 🟢 ≥ 70% (giả định mô hình) · 🟡 42,4–69% · 🔴 < 42,4%
```

**[MH-3] p95 chi phí AI ÷ doanh thu** (đèn #4)

```
Đầu vào: ARPU = $19,12 · gói gồm 4 pack · trần dự kiến 12 render/pack (Lab22 tab 5)
Designer dùng đủ 4 pack, 5 render/pack:  4 × $1,1586 = $4,63 → 4,63 ÷ 19,12 = 0,24
Designer dùng đủ 4 pack, chạm trần 12:   Cost(12) = (0,1133×12 + 0,2445) ÷ 0,7 = $2,29
                                          4 × 2,29 = $9,17 → 9,17 ÷ 19,12 = 0,48
(Designer dùng > 4 pack trả thêm 129.000 ₫ ≈ $4,94/pack > Cost/Job → tỷ lệ không xấu đi; vì vậy mẫu số là doanh thu thực thu)
Kết quả → 🟢 ≤ 0,24 · 🟡 0,24–0,48 · 🔴 > 0,48 (đuôi đã vượt mức tối đa mà sản phẩm cho phép → có rò rỉ)
```

**[MH-4] Retention cần thiết và trần CAC thật** (đèn #2, #6)

```
Lãi gộp / designer / tháng = 19,12 × 75,76% = $14,49 (≈ 378.000 ₫)
Lab22 tính ngân sách CAC = 14,49 × 12 = $173,8: công thức này NGẦM GIẢ ĐỊNH KHÔNG AI RỜI ĐI.
Giả định đường cong hình học, giữ chân r mỗi tháng → số tháng ở lại kỳ vọng = 1 ÷ (1 − r)
(a) Muốn hoàn vốn $173,8: cần ≥ 12 tháng → r ≥ 1 − 1/12 = 91,7%  (⇔ M12 ≥ 35,2%)
(b) Nếu chỉ bằng trung vị app AI, M12 = 21,1% [BM]: r = 0,211^(1/12) = 87,8%
    → ở lại 1 ÷ 0,122 = 8,2 tháng → LTV = 14,49 × 8,2 = $119
    → CAC tối đa để LTV/CAC ≥ 3: 119 ÷ 3 = $39,7
Chi phí dùng thử / người = 2 pack × $1,1586 = $2,32
    → chi phí trial / khách trả phí = 2,32 ÷ tỷ lệ chuyển đổi
    → vượt $39,7 khi trial→paid < 2,32 ÷ 39,7 = 5,84%
Kết quả → Retention 🟢 ≥ 91,7% · 🟡 87,8–91,7% · 🔴 < 87,8%
          Trial→paid 🔴 < 5,6% (BM, và < 5,84% thì riêng chi phí trial đã vượt trần CAC)
Hệ quả: ngân sách CAC $174 của Lab22 chỉ đúng khi mình giữ chân tốt hơn cả trung vị app KHÔNG-AI (35,2% > 30,7%).
```

**[MH-5] CAC payback tối đa theo runway** (đèn #7)

```
Đầu vào: runway = 18 tháng · khách trả phí đầu tiên đến khoảng tháng 2–3 (Lab22: 60 khách cuối tháng 3)
Thời gian còn lại để chứng minh vòng "chi CAC → hoàn vốn → chi lại" = 18 − 3 = 15 tháng
Cần ≥ 2 vòng (vòng 1 chứng minh hoàn vốn được, vòng 2 chứng minh lặp lại được trước khi hết tiền)
Payback tối đa = 15 ÷ 2 = 7,5 tháng
Ở r = 91,7%: lãi gộp cộng dồn 7 tháng = 14,49 × (1 − 0,917^7) ÷ (1 − 0,917) = 14,49 × 5,47 = $79
  → CAC ≤ $79 để payback ≤ 7,5 tháng; CAC ≤ $113 để payback ≤ 12 tháng (không phải $174)
Kết quả → 🟢 ≤ 7,5 tháng · 🟡 7,5–12 · 🔴 > 12 tháng [BM Bessemer SMB] hoặc không hoàn vốn
```

---

## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng: có **3 luật dừng** (Luật 1, 2, 4). Người làm mọi vế THÌ: Phan Duy Thành, bắt đầu **thứ Hai ngay sau** buổi họp tuần mà đèn đỏ.

**Luật 1 ⏹ — Retention**
- **NẾU** retention tháng-qua-tháng của designer trả phí < 87,8%
- **TRONG** 2 cohort tháng liên tiếp
- **VÀ** gộp 2 cohort ≥ 40 designer trả phí (với số nhỏ hơn thì 1–2 người rời đã đủ làm số nhảy 5–10 điểm)
- **THÌ** đóng băng toàn bộ chi tiêu thu hút mới (ads, KOL, đăng bài tìm user trong nhóm) trong 4 tuần, tức đúng 1 chu kỳ dùng của designer. Trong tuần đầu, gọi 10 designer đã rời để ghi lại lý do. Sprint tiếp theo chỉ sửa activation và chất lượng pack.
- **KHÔNG THÌ** không tăng ngân sách ads, không mở thêm phân khúc chủ nhà tự thiết kế để bù số designer rời. Đó là đổ nước vào thùng thủng: CAC tăng mà LTV không đổi.

**Luật 2 ⏹ — Render / phòng (chi phí AI)**
- **NẾU** render trung bình / phòng > 9,6
- **TRONG** 2 tuần liên tiếp
- **VÀ** mỗi tuần ≥ 30 phòng
- **THÌ** dừng onboarding designer mới. Trong cùng sprint, bật trần cứng 12 render/pack và chuyển lượt nháp sang ảnh 1K ($0,067). Chỉ mở onboarding lại khi chỉ số ≤ 9,6 trong 1 tuần.
- **KHÔNG THÌ** không đổi sang model ảnh rẻ hơn (Nano Banana 2.1, Cost/Job ≈ $0,80) khi chưa eval lại 50 phòng. Model chưa eval làm ảnh xấu hơn, designer render lại nhiều hơn và chi phí còn tăng.

**Luật 3 — p95 chi phí AI**
- **NẾU** p95 chi phí AI / designer ÷ doanh thu của designer đó > 0,48
- **TRONG** 1 tháng đóng sổ
- **VÀ** tháng đó ≥ 20 designer trả phí (dưới 20 người thì p95 chỉ là người nặng nhất)
- **THÌ** xuất danh sách 5% designer nặng nhất. Trong tuần đó, kiểm tra từng tài khoản xem có lách quota không (xuất lại cùng `design_id`, render không giới hạn trong 1 pack). Bật thu tự động 129.000 ₫ cho pack thứ 5 trở đi.
- **KHÔNG THÌ** không tăng giá gói 499.000 ₫ cho tất cả. 95% designer dùng nhẹ không gây ra khoản lỗ này, và tăng giá sẽ đẩy họ sang Interior AI hoặc REimagineHome.

**Luật 4 ⏹ — Trial → paid**
- **NẾU** trial→paid < 5,6%
- **TRÊN** 3 phiên bản luồng dùng thử khác nhau
- **VÀ** mỗi phiên bản ≥ 50 người dùng thử
- **THÌ** dừng cho dùng thử 2 pack miễn phí với người tự đăng ký. Chỉ mở trial qua giới thiệu của designer đang trả phí. Founder quay lại onboarding 1–1 tại buổi tư vấn thật (5 designer, 2 tuần) để tìm điểm gãy activation.
- **KHÔNG THÌ** không giảm giá gói và không tăng số pack miễn phí. Sản phẩm chưa tạo được thói quen thì giảm giá chỉ phá neo giá 499k, còn thêm pack miễn phí thì đốt thêm $1,16 cho mỗi pack.

**Luật 5 — Containment**
- **NẾU** containment < 42,4%
- **TRONG** 2 tuần liên tiếp
- **VÀ** mỗi tuần ≥ 30 phòng
- **THÌ** cắt phạm vi xuống 2 loại phòng có containment cao nhất (theo `room_type`), tạm ẩn các loại phòng còn lại trên app trong sprint kế tiếp, và chạy lại eval 50 phòng cho 2 loại đó.
- **KHÔNG THÌ** không chuyển sang Biến thể B (mình dựng tay hộ khi AI trượt) để giữ khách. Mỗi ca tốn $3,75, khiến containment hoà vốn nhảy từ 42,4% lên 80,6% ([MH-2]).
