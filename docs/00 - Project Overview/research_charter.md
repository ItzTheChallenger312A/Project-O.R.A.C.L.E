# Project O.R.A.C.L.E. — Research Charter
**Document ID:** P0-01-CHARTER  
**Version:** 0.1.2  
**Date:** 2026-11-10  
**Project Lead:** Dylan  
**Status:** Pre-Registration Lock (Đóng băng tiêu chuẩn nghiên cứu)

---

## 1. Mục tiêu nghiên cứu (Research Objective)
Xác định một cách khoa học, định lượng và có thể kiểm chứng xem: Việc bổ sung mô hình ngôn ngữ lớn (LLM) để giải thích dữ liệu vĩ mô và bối cảnh tin tức có tạo ra **lợi thế dự báo vượt trội có ý nghĩa thống kê (Statistical Edge / Alpha)** đối với biến động giá cặp `EURUSD` trong khung thời gian 1 phiên giao dịch hay không, so với các phương pháp đối chứng cơ bản không dùng AI và sau khi trừ toàn bộ chi phí giao dịch mô phỏng.

---

## 2. Câu hỏi nghiên cứu (Research Questions)

### Câu hỏi chính (Primary Question):
- **RQ-1:** *Sau khi đã trừ chi phí spread và trượt giá (1.5 pips), chiến lược dự báo phương hướng phiên tiếp theo của LLM có mang lại tỷ suất lợi nhuận kỳ vọng ròng (Net Expected Return) vượt trội hơn phương pháp dự báo theo luật vĩ mô cứng (Macro Surprise Rule) và các phương pháp kỹ thuật cơ bản hay không?*

### Các câu hỏi phụ (Secondary Questions):
- **RQ-2 (Hiệu chỉnh độ tự tin):** *Chỉ số `confidence` (0.00 – 1.00) do LLM tự báo cáo có phản ánh xác suất thành công thực tế (Probability Calibration) hay bị overconfidence?*
- **RQ-3 (Quyền từ chối):** *Cơ chế `abstain` (từ chối đưa ra nhận định khi dữ liệu mâu thuẫn) có giúp bảo vệ hệ thống khỏi những phiên nhiễu động cao và giảm thiểu drawdown không?*
- **RQ-4 (Tối ưu kinh tế):** *Mức lợi nhuận thặng dư (nếu có) do LLM tạo ra có bù đắp được chi phí token API và độ phức tạp vận hành của hệ thống hay không?*

---

## 3. Hệ thống Giả thuyết (Hypotheses)

- **Giả thuyết Không ($H_0$):**
  > Việc bổ sung LLM để giải thích các sự kiện kinh tế vĩ mô **không tạo ra sự cải thiện có ý nghĩa thống kê** về lợi nhuận ròng sau chi phí so với phương pháp đối chứng tốt nhất không dùng LLM (Macro Surprise Rule) trên cặp EURUSD:
  > $$\mu_{\text{Net PnL (AI)}} \le \mu_{\text{Net PnL (Baseline)}}$$

- **Giả thuyết Đối ($H_1$):**
  > Việc bổ sung LLM tạo ra mức cải thiện dương có ý nghĩa thống kê về lợi nhuận ròng sau chi phí so với phương pháp đối chứng tốt nhất không dùng LLM trên cặp EURUSD:
  > $$\mu_{\text{Net PnL (AI)}} > \mu_{\text{Net PnL (Baseline)}}$$

---

## 4. Công cụ & Khung thời gian giao dịch (Instrument & Horizon)

- **Tài sản nghiên cứu (Instrument):** `EURUSD` duy nhất trong Phase 0–6.
  - *Lý do:* Thanh khoản lớn nhất thế giới, chênh lệch giá mua/bán (spread) thấp nhất, phản ánh tương quan chính sách tiền tệ song phương rõ nét (Fed vs. ECB).
- **Khung thời gian giao dịch (Horizon Definition):** `1 Trading Session — Snapshot-to-Snapshot`
  - **Snapshot Time:** 15:30 `America/New_York` (thời điểm thị trường bước vào giai đoạn đóng cửa phiên Mỹ).
  - **Prediction Window:** 15:30 – 15:35 `America/New_York`.
  - **Entry Simulation:** Báo giá đầu tiên ghi nhận được ngay sau khi nhận định hoàn tất trong cửa sổ trên.
  - **Exit Simulation:** Báo giá đầu tiên ghi nhận tại hoặc ngay sau mốc Snapshot 15:30 của phiên kế tiếp.
  - **Cohort phân tách:** Thứ Hai đến Thứ Năm là Cohort chính. Thứ Sáu được theo dõi riêng biệt (Weekend Holding Cohort) để tránh làm méo mó rủi ro khoảng trống giá (gap) đầu tuần.

---

## 5. Thước đo Đánh giá (Metrics & Costs)

### Chi phí giao dịch giả định cố định (Transaction Cost Floor):
- **Spread:** 1.0 pip.
- **Slippage ước tính:** 0.5 pip.
- **Tổng chi phí cản trở (Hurdle Rate):** **`1.5 pips/phiên`** (tương đương 0.00015 đối với EURUSD).

### Thước đo chính (Primary Metric):
- **Chênh lệch Lợi nhuận Ròng Giả lập bình quân theo phiên (Mean Net PnL Difference):**
  $$\Delta \text{Net Return} = \overline{R}_{\text{AI}} - \overline{R}_{\text{Comparator}}$$
  *(Đo bằng pips sau khi đã trừ 1.5 pips chi phí cho mỗi vị thế mở).*

### Thước đo phụ (Secondary Metrics):
1. **Directional Accuracy (Hit Rate):** Tỷ lệ phiên dự đoán đúng chiều biến động giá (loại trừ các phiên `abstain`).
2. **Expectancy (Pips/trade):** $(W \times \text{Avg Win}) - (L \times \text{Avg Loss}) - \text{Costs}$.
3. **Brier Score:** Đo lường sai số bình phương giữa `confidence` và kết quả nhị phân $\{0, 1\}$.
4. **Coverage / Abstain Ratio:** Tỷ lệ % số phiên hệ thống chủ động đứng ngoài thị trường.
5. **Maximum Drawdown (Simulated):** Mức sụt giảm vốn mô phỏng lớn nhất trong chuỗi thử nghiệm.

---

## 6. Yêu cầu Toàn vẹn Dữ liệu (Data Integrity Requirements)

1. **Phân tách Hai trục Nghiên cứu:**
   - **Historical Price Research:** Kiểm nghiệm 3 baseline kỹ thuật trên dữ liệu giá quá khứ đã được chuẩn hóa.
   - **Prospective Macro Research:** Chỉ thu thập consensus và sự kiện vĩ mô theo thời gian thực (Point-in-Time - PIT). Tuyệt đối không dùng dữ liệu macro được cập nhật sau sự kiện (ex-post revisions) để đánh giá quá khứ.
2. **Nguyên tắc Bất biến (Append-Only):**
   - Mọi dự đoán sinh ra phải được lưu vào SQLite kèm `timestamp`, `model_version`, `prompt_hash`, `input_snapshot_id`.
   - SQLite enforce Trigger cấm thao tác `UPDATE` và `DELETE`.
   - Snapshot đầu vào phải được lưu trữ để đảm bảo khả năng tái lập (reproducibility) 100%.

---

## 7. Giới hạn Thống kê & Kích thước mẫu (Statistical Limitations)

- **Power Analysis & Sample Size:**
  - Để phát hiện một lợi thế siêu mỏng (ví dụ: Hit rate 55% so với 50%) ở mức tin cậy 95% và power 80%, cần khoảng 780 mẫu độc lập ($\approx 3$ năm giao dịch). Điều này nằm ngoài tính khả thi của một dự án sinh viên.
  - **Quy ước Giới hạn của O.R.A.C.L.E.:** Dự án chỉ tập trung kiểm chứng xem AI có tạo ra **Hiệu ứng Đủ Lớn (Large Effect Size: Hit Rate $\ge 62\%$, Net Expectancy $\ge +3.0$ pips/phiên)** hay không.
  - Cỡ mẫu tối thiểu để kết luận: **$N \ge 85 - 100$ phiên/sự kiện độc lập** (tương đương 4–6 tháng forward-test).
  - Giai đoạn 4–6 tuần đầu tiên chỉ là **Pipeline Sanity Check**, không dùng để kết luận bất kỳ điều gì về Alpha.

---

## 8. Ngân sách & Điểm kiểm soát (Budget & Checkpoints)

- **Initial Time Checkpoint:** **`12 Giờ làm việc thực tế`** (dành cho Phase 0 đến Phase 3). Sau 12 giờ này, toàn bộ dữ liệu baseline giá sẽ được tổng hợp để xem xét tiếp tục hay dừng lại.
- **Ngân sách LLM hiện tại:** **`0.00 USD`**. Không gọi API LLM trả phí nào trong Phase 0–3.
- **Chi phí tối đa dự kiến cho Phase 5 (Forward-test):** Dưới 5.00 USD/tháng bằng cách tận dụng Prompt Caching và Model phân tích nhẹ (như Gemini Flash / GPT-4o-mini).

---

## 9. Tiêu chuẩn Quyết định GO / NO-GO (Decision Gates)

### Gate 1: Checkpoint sau Phase 3 (Historical Baseline Review)
- **Điều kiện GO:** Hoàn thành việc tính toán phân phối lợi nhuận, volatility và kết quả của 3 baseline trên dữ liệu giá EURUSD; xác nhận chi phí 1.5 pips không triệt tiêu hoàn toàn khả năng có lợi nhuận.
- **Điều kiện NO-GO:** Dữ liệu giá không đủ độ tin cậy, chi phí cản trở quá lớn khiến mọi chiến lược đơn giản đều có kỳ vọng âm nặng.

### Gate 2: Checkpoint sau Phase 6 (AI vs. Baseline Evaluation)
Hệ thống sẽ tuyên bố **NO-GO (Dừng dự án và kết luận AI không có Alpha)** nếu:
1. `Net Return (AI) <= Net Return (Macro Surprise Rule)`.
2. Lợi nhuận ròng sau chi phí của AI $\le 0$ pips sau 85 mẫu độc lập.
3. Brier Score cho thấy model hoàn toàn không có khả năng hiệu chỉnh độ tự tin (overconfident ở các lệnh lỗ nặng).
4. Tỷ lệ `abstain > 80%` nhưng 20% còn lại không có hiệu suất vượt trội.
