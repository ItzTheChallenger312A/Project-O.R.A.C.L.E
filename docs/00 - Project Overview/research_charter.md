# Project O.R.A.C.L.E. — Research Charter
**Document ID:** P0-01-CHARTER  
**Version:** 0.1.2  
**Date:** 2026-11-10  
**Project Lead:** Dylan  
**Status:** Pre-Registration Lock (Đóng băng tiêu chuẩn nghiên cứu)

---

## 1. Mục tiêu nghiên cứu (Research Objective)
Xác định một cách định lượng và có thể kiểm chứng xem: Việc bổ sung mô hình ngôn ngữ lớn (LLM) để diễn giải dữ liệu vĩ mô và bối cảnh tin tức có tạo ra **lợi thế dự báo (Alpha) thực chất** đối với biến động giá cặp `EURUSD` trong khung thời gian 1 phiên giao dịch hay không, so với các phương pháp đối chứng cơ bản không dùng AI và **sau khi hạch toán chi phí giao dịch thực tế (Dynamic Bid/Ask Spread + Estimated Slippage)**.

---

## 2. Câu hỏi nghiên cứu (Research Questions)

### Câu hỏi chính (Primary Question):
- **RQ-1:** *Sau khi đã trừ chi phí spread thực tế (Bid/Ask) và trượt giá mô phỏng, chiến lược dự báo phương hướng phiên tiếp theo của LLM có mang lại tỷ suất lợi nhuận kỳ vọng ròng (Net Expected Return) vượt trội hơn phương pháp dự báo theo luật vĩ mô (Macro Surprise Rule) và các phương pháp kỹ thuật cơ bản hay không?*

### Các câu hỏi phụ (Secondary Questions):
- **RQ-2 (Hiệu chỉnh độ tự tin):** *Chỉ số `confidence` (0.00 – 1.00) do LLM tự báo cáo có phản ánh xác suất thành công thực tế (Probability Calibration) hay bị overconfidence?*
- **RQ-3 (Quyền từ chối):** *Cơ chế `abstain` (từ chối đưa ra nhận định khi dữ liệu mâu thuẫn) có giúp bảo vệ hệ thống khỏi những phiên nhiễu động cao và giảm thiểu drawdown không?*
- **RQ-4 (Tối ưu kinh tế):** *Mức lợi nhuận thặng dư (nếu có) do LLM tạo ra có bù đắp được chi phí token API và độ phức tạp vận hành của hệ thống hay không?*

---

## 3. Hệ thống Giả thuyết (Hypotheses)

- **Giả thuyết Không ($H_0$):**
  > Việc bổ sung LLM để giải thích các sự kiện kinh tế vĩ mô **không tạo ra sự cải thiện có ý nghĩa** về lợi nhuận ròng sau chi phí so với phương pháp đối chứng tốt nhất không dùng LLM (Macro Surprise Rule) trên cặp EURUSD:
  > $$\mu_{\text{Net PnL (AI)}} \le \mu_{\text{Net PnL (Comparator)}}$$

- **Giả thuyết Đối ($H_1$):**
  > Việc bổ sung LLM tạo ra mức cải thiện dương về lợi nhuận ròng sau chi phí so with phương pháp đối chứng tốt nhất không dùng LLM trên cặp EURUSD:
  > $$\mu_{\text{Net PnL (AI)}} > \mu_{\text{Net PnL (Comparator)}}$$

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

## 5. Thước đo Đánh giá & Hạch toán Chi phí (Metrics & Costs)

### Mô hình Chi phí Giao dịch Thực tế (Dynamic Cost Model):
- **Không áp dụng mức phí cố định giả định:** Thay vì dùng con số 1.5 pips cố định, chi phí giao dịch sẽ được **đo lường trực tiếp từ chênh lệch Bid/Ask thực tế** tại thời điểm vào/ra lệnh từ dataset, cộng thêm ước lượng trượt giá (slippage model) phụ thuộc vào volatility của phiên.
- Mọi phép tính PnL bắt buộc phải hạch toán dựa trên:
  - Lệnh Long: Mua tại `Ask_entry`, đóng tại `Bid_exit`.
  - Lệnh Short: Bán tại `Bid_entry`, đóng tại `Ask_exit`.

### Thước đo chính (Primary Metric):
- **Chênh lệch Lợi nhuận Ròng Giả lập bình quân theo phiên (Mean Net PnL Difference):**
  $$\Delta \text{Net Return} = \overline{R}_{\text{AI}} - \overline{R}_{\text{Comparator}}$$
  *(Đo bằng pips sau khi đã trừ toàn bộ Bid/Ask spread và slippage thực tế).*

### Thước đo phụ (Secondary Metrics):
1. **Directional Accuracy (Hit Rate):** Tỷ lệ phiên dự đoán đúng chiều biến động giá (loại trừ các phiên `abstain`).
2. **Expectancy (Pips/trade):** $(W \times \text{Avg Win}) - (L \times \text{Avg Loss}) - \text{Costs}$.
3. **Brier Score:** Đo lường sai số bình phương giữa `confidence` và kết quả thực tế.
4. **Coverage / Abstain Ratio:** Tỷ lệ % số phiên hệ thống chủ động đứng ngoài thị trường.
5. **Maximum Drawdown (Simulated):** Mức sụt giảm vốn mô phỏng lớn nhất trong chuỗi thử nghiệm.

---

## 6. Yêu cầu Toàn vẹn Dữ liệu (Data Integrity Requirements)

1. **Phân tách Hai trục Nghiên cứu rõ ràng:**
   - **Historical Price Research (Phase 2):** Kiểm nghiệm 3 baseline kỹ thuật trên dữ liệu giá quá khứ đã được chuẩn hóa.
   - **Prospective Macro Research (Phase 4+):** Chỉ thu thập consensus và sự kiện vĩ mô theo thời gian thực (Point-in-Time - PIT). Tuyệt đối không dùng dữ liệu macro được cập nhật sau sự kiện (ex-post revisions) để đánh giá quá khứ.
2. **Nguyên tắc Bất biến (Append-Only):**
   - Mọi dự đoán sinh ra phải được lưu vào SQLite kèm `timestamp`, `model_version`, `prompt_hash`, `input_snapshot_id`.
   - SQLite enforce Trigger cấm thao tác `UPDATE` và `DELETE`.
   - Snapshot đầu vào phải được lưu trữ để đảm bảo khả năng tái lập (reproducibility) 100%.

---

## 7. Khung Nghiên cứu Thăm dò & Xác định MDE (Exploratory Framework & Statistical Planning)

- **Không ấn định trước ngưỡng Hit Rate cứng (No Arbitrary Targets):**
  - Tránh bẫy tự huyễn hoặc bằng các con số giả định (như $62\%$ hay $65\%$).
  - Trong Phase 0–3, dự án vận hành dưới **Khung nghiên cứu thăm dò (Exploratory Research Framework)**.
- **Quy trình xác định Cỡ mẫu và MDE (Minimum Detectable Effect):**
  1. Trong Phase 2, hệ thống sẽ tính toán phương sai lợi nhuận thực tế ($\sigma_{\text{daily}}$), phân phối độ biến động và spread thực tế từ historical data của EURUSD.
  2. Từ các tham số thực nghiệm đó, tài liệu `03-methodology/statistical_feasibility.md` sẽ tính toán chính xác:
     - Minimum Detectable Effect (MDE) khả thi.
     - Statistical Power ứng với các kịch bản kích thước mẫu ($N$).
  3. Chỉ sau khi có các tham số này, ngưỡng đánh giá chính thức mới được ấn định trước khi bước vào Phase 5 (Forward-test).

---

## 8. Ngân sách & Điểm kiểm soát (Budget & Checkpoints)

- **Initial Time Checkpoint:** **`12 Giờ làm việc thực tế`** (dành cho Phase 0 đến Phase 3). Sau 12 giờ này, toàn bộ dữ liệu baseline giá sẽ được tổng hợp để xem xét tiếp tục hay dừng lại.
- **Ngân sách LLM hiện tại:** **`0.00 USD`**. Không gọi API LLM trả phí nào trong Phase 0–3.
- **Chi phí tối đa dự kiến cho Phase 5 (Forward-test):** Dưới 5.00 USD/tháng bằng cách tận dụng Prompt Caching và Model phân tích nhẹ (như Gemini Flash / GPT-4o-mini).

---

## 9. Tiêu chuẩn Quyết định GO / NO-GO (Decision Gates)

### Gate 1: Checkpoint sau Phase 3 (Historical Baseline Review)
- **Điều kiện GO:** Hoàn thành việc tính toán phân phối lợi nhuận, volatility và kết quả của 3 baseline trên dữ liệu giá EURUSD; xác nhận chi phí spread thực tế không triệt tiêu hoàn toàn khả năng có lợi nhuận.
- **Điều kiện NO-GO:** Dữ liệu giá không đủ độ tin cậy, chi phí cản trở quá lớn khiến mọi chiến lược đơn giản đều có kỳ vọng âm nặng.

### Gate 2: Checkpoint sau Phase 6 (AI vs. Baseline Evaluation)
Hệ thống sẽ tuyên bố **NO-GO (Dừng dự án và kết luận AI không có Alpha)** nếu:
1. `Net Return (AI) <= Net Return (Macro Surprise Rule)`.
2. Lợi nhuận ròng sau chi phí thực tế của AI $\le 0$ pips sau khi đạt đủ cỡ mẫu MDE quy định.
3. Brier Score cho thấy model hoàn toàn không có khả năng hiệu chỉnh độ tự tin (overconfident ở các lệnh lỗ nặng).
4. Tỷ lệ `abstain > 80%` nhưng 20% còn lại không có hiệu suất vượt trội.
