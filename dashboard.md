# OPERATING DASHBOARD — AI dựng concept nội thất cho Freelance Designer

**Loại mô hình:** B2C. Freelance designer vừa tự trả 499.000 ₫/tháng vừa tự dùng; chủ nhà chỉ xem pack qua link Zalo · **Cập nhật:** 09/10/2026 (trước pilot 13/10) · Phan Duy Thành – 2A202602930

**NORTH STAR:** Retention tháng-qua-tháng của designer trả phí (đường cong cohort M1→M3) · hiện tại **chưa có** (điểm đầu ~12/12/2026) · mục tiêu **≥ 91,7%/tháng**, đường cong phẳng dần

### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| Activation 48h: % designer mới xuất pack đạt chuẩn đầu tiên ≤48h | 🔧 từ 13/10 | ≥60% / 30–59% / <30% | [TB] baseline 14/12/2026; tạm dùng KPI 6/10 | Trial→paid → Retention |
| ⭐ Retention tháng-qua-tháng (trả phí **và** ≥1 pack/tháng) | 🔧 ~12/12 | ≥91,7% / 87,8–91,7% / <87,8% | [MH] ở ≥12 tháng · [BM] M12 AI 21,1% (RevenueCat, ktra 09/10/2026) | CAC payback, LTV |
| Render trung bình / phòng (chi phí AI) | 🔧 (mô hình: 5) | ≤6,7 / 6,8–9,6 / >9,6 | [MH] >9,6 → GM <60% | p95 cost → GM |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| p95 chi phí AI/designer ÷ doanh thu của designer đó (chi phí AI) | 🔧 log token 27/10 | ≤0,24 / 0,24–0,48 / >0,48 | [MH] 0,48 = trần 12 render × 4 pack | GM, CAC payback |
| Containment: phòng ra pack đạt chuẩn ≤7 ngày | 🔧 eval 31/10 (giả định 70%) | ≥70% / 42,4–69% / <42,4% | [MH] breakeven GM 60% | Activation, Retention, GM |
| Trial → paid (trả gói ≤14 ngày) | 🔧 tháng 2 | ≥8,5% / 5,6–8,4% / <5,6% | [BM] AI 8,5% · non-AI 5,6% (RevenueCat, ktra 09/10/2026) + [MH] | Retention → CAC payback |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| CAC payback **có tính churn** (CAC gồm chi phí API của trial và giờ onboarding) | ❌ chưa có CAC | ≤7,5 / 7,5–12 / >12 tháng | [MH] (runway 18 − 3) ÷ 2 · [BM] Bessemer SMB 12 tháng |

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** retention <87,8% **TRONG** 2 cohort liên tiếp **VÀ** gộp ≥40 designer **THÌ** đóng băng mọi chi tiêu thu hút 4 tuần, gọi 10 người đã rời, sprint sau chỉ sửa activation và chất lượng pack. **KHÔNG THÌ** không tăng ads, không mở phân khúc chủ nhà để bù churn.
2. ⏹ **NẾU** render/phòng >9,6 **TRONG** 2 tuần **VÀ** ≥30 phòng/tuần **THÌ** dừng onboarding designer mới, bật trần 12 render/pack và nháp 1K ngay trong sprint. **KHÔNG THÌ** không đổi sang model ảnh rẻ hơn khi chưa eval lại 50 phòng.
3. **NẾU** p95 cost ÷ doanh thu >0,48 **TRONG** 1 tháng **VÀ** ≥20 designer trả phí **THÌ** rà từng tài khoản trong top 5% để tìm cách lách quota, bật thu tự động 129k từ pack thứ 5. **KHÔNG THÌ** không tăng giá gói 499k cho tất cả.
4. ⏹ **NẾU** trial→paid <5,6% **TRÊN** 3 phiên bản trial **VÀ** mỗi bản ≥50 người **THÌ** dừng trial miễn phí cho người tự đăng ký, chỉ mở trial qua giới thiệu, founder onboarding 1–1 cho 5 designer. **KHÔNG THÌ** không giảm giá, không tặng thêm pack.
5. **NẾU** containment <42,4% **TRONG** 2 tuần **VÀ** ≥30 phòng/tuần **THÌ** cắt xuống 2 `room_type` tốt nhất, ẩn các loại còn lại, eval lại 50 phòng. **KHÔNG THÌ** không làm hộ bằng tay (Biến thể B), vì khi đó containment hoà vốn nhảy lên 80,6%.

### Cổng gác 90 ngày (ngày 0 = 13/10/2026)

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 · 12/11 (học) | Số phòng thật có log đủ `room_photo_uploaded → ai_render_completed → design_concept_exported` và token theo `render_id` | ≥ 50 phòng | `eval_50_phong.csv` + Pilot Report v0 có baseline activation, containment, render/phòng | FIX tracking 1 lần → trượt lần 2: KILL pilot |
| 60 · 12/12 | Retention M1 của cohort trả phí đầu tiên | ≥ 87,8% (cohort ≤8 người = không ai rời) | Bảng cohort xuất từ event + danh sách người rời kèm lý do | Lý do là chất lượng/activation → FIX (Luật 1); lý do "không có khách mới" → PIVOT sang gói Team studio |
| 90 · 11/01/2027 | Trial → paid | ≥ 8,5% trên ≥ 100 lượt thử | Báo cáo funnel `trial_started → subscription_paid` | 5,6–8,4% → FIX activation 1 lần; <5,6% → PIVOT đổi người trả tiền (studio 2–5 người) |

**KILL CRITERIA:** Nếu đến **28/02/2027** retention tháng-qua-tháng của designer trả phí vẫn **< 87,8% trên 2 cohort liên tiếp (gộp ≥ 40 designer) sau khi đã FIX một lần**, mình dừng hẳn hướng B2C cho freelance designer và dùng khoảng 13 tháng runway còn lại cho gói Team studio.

**CHƯA ĐO ĐƯỢC** (hôm nay chưa có số thật nào; mọi số ở trên là ngưỡng, không phải hiện trạng):

- **Retention, Activation:** cần event `design_concept_exported` chạy production và gắn `plan_status`. Có số từ 13/10; baseline activation 14/12/2026; điểm retention M1 đầu tiên khoảng 12/12/2026.
- **Chi phí thật (render/phòng, p95):** cần log `usage_metadata` của Gemini theo `render_id → user_id`. Hạn 27/10/2026. Cost/Job $1,16 vẫn là **ước tính**.
- **Containment:** đang là giả định 70%. Cần eval 50 phòng thật, hạn 31/10/2026.
- **Trial→paid:** cần luồng dùng thử 2 pack trên Zalo Mini App, có từ tháng 2 (~12/11/2026); ≥100 lượt thử khoảng 11/01/2027.
- **CAC:** cần sổ chi theo kênh và log giờ onboarding tay, bắt đầu 13/10. Payback đầu tiên tính được cuối quý 1/2027.
- ❌ **Retention M12:** chỉ có số khoảng 11/2027. ❌ **Refund:** chưa có cổng thanh toán và chính sách hoàn tiền; phải chốt chính sách trước 30/11/2026.
