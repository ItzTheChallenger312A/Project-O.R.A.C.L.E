# Project O.R.A.C.L.E. — Experiment Charter & Hypothesis
**Version:** 0.2.0 (Council & GPT Synthesis — Reality-Checked)  
**Date:** 2026-11-10  
**Author:** Dylan (Project Lead)  
**Status:** Pre-experiment Lock (Đóng băng tiêu chuẩn trước khi thu thập dữ liệu)

---

## 1. Câu hỏi nghiên cứu (Research Question)
> **"Liệu việc sử dụng LLM để giải thích các sự kiện kinh tế vĩ mô có tạo ra một hiệu ứng vượt trội đủ lớn (Large Effect: Hit Rate $\ge 62\%$ sau khi trừ 1.5 pips chi phí) so với các quy tắc vĩ mô cứng (Macro Surprise Rule) trên cặp EURUSD trong khung thời gian 1 ngày giao dịch hay không?"**

### Các nguyên tắc bất biến (Non-negotiable Constraints):
1. **Zero Real-Money Trading:** Không dùng tiền thật dưới mọi hình thức.
2. **Immutable Audit Trail:** SQLite kích hoạt Trigger cấm tuyệt đối lệnh `UPDATE`/`DELETE`. Mỗi prediction chứa hash SHA-256 liên kết chuỗi (Merkle chain) và được tự động commit lên GitHub để bảo chứng timestamp.
3. **Phân tách Backtest vs. Forward-test:**
   - **Baseline 4 (Luật cứng không LLM):** Được phép backtest trên 3–5 năm dữ liệu lịch sử để thiết lập chuẩn mực (benchmark).
   - **LLM Intelligence:** 100% Forward-testing theo thời gian thực (để triệt tiêu hoàn toàn rủi ro Data Leakage).
4. **Không đo edge siêu mỏng (No Micro-Edge Chasing):** Từ bỏ việc chứng minh edge mỏng $52\%-54\%$ vì đòi hỏi $>700$ mẫu (tương đương 3 năm chạy live) và sẽ bị spread nuốt chửng. Hệ thống chỉ tìm kiếm **Hiệu ứng Đủ Lớn** ($N \approx 85-100$ mẫu độc lập).

---

## 2. Thiết lập kỹ thuật (Technical Specs)

| Thuộc tính | Lựa chọn chốt | Lý do khoa học |
| :--- | :--- | :--- |
| **Cặp tài sản (Instrument)** | `EURUSD` | Cặp tiền thanh khoản cao nhất, spread mỏng nhất, phản ánh tương quan chính sách Fed vs. ECB. |
| **Khung thời gian (Horizon)** | `1 Trading Day (24H)` | Tránh nhiễu HFT vi mô; kiểm tra khả năng hấp thụ tin tức vĩ mô theo ngày. |
| **Chi phí giao dịch giả định** | `1.5 pips` cố định | 1.0 pip spread + 0.5 pip trượt giá (slippage) trong các phiên tin mạnh. |
| **Cơ chế lưu trữ** | `SQLite Append-Only` | Có SQLite Trigger chặn UPDATE/DELETE + SHA-256 hash validation + Git remote push. |

---

## 3. Hệ thống 4 Baselines (Hệ quy chiếu đối thủ)

| Baseline | Tên đối thủ | Cơ chế hoạt động | Phương thức đánh giá |
| :--- | :--- | :--- | :--- |
| **B1** | **Random Walk** | Tung đồng xu 50/50 ngẫu nhiên. | Lý thuyết xác suất. |
| **B2** | **Rolling Base Rate** | Tỷ lệ nến xanh/đỏ EURUSD tính trong cửa sổ trượt (Rolling Window) 60 ngày gần nhất (tránh data leakage). | Lịch sử & Realtime. |
| **B3** | **Yesterday Trend** | Nến Daily hôm qua tăng $\to$ Đoán tăng; giảm $\to$ Đoán giảm. | Lịch sử & Realtime. |
| **B4** | **Macro Surprise Engine** *(Đối thủ chính)* | Bộ luật cứng định hướng tác động: $Actual$ vs $Forecast$, phân định theo cặp EURUSD (ví dụ: Tin tốt cho USD $\to$ Sell EURUSD). | **Backtest lịch sử 3-5 năm TRƯỚC TIÊN** + Forward-test song song. |

---

## 4. Minimum Detectable Effect (M.D.E.) & Cỡ mẫu thống kê

- **Tại sao không nhắm tới Hit Rate 55%?**
  Để chứng minh $55\%$ khác $50\%$ có ý nghĩa thống kê (Power 80%, $\alpha = 0.05$), cần tới $\approx 780$ mẫu độc lập ($\approx 3$ năm). Thêm vào đó, $55\%$ trừ 1.5 pips spread sẽ đưa lợi nhuận thực tế về ngưỡng hòa vốn hoặc lỗ.
- **Mục tiêu thực tế của O.R.A.C.L.E.:**
  - Giả định mục tiêu Hit Rate: $\ge 62\%$.
  - Cỡ mẫu tối thiểu cần thiết để đạt ý nghĩa thống kê ở mức này: **$N \approx 85 - 100$ sự kiện vĩ mô độc lập**.
  - Thời gian quan sát dự kiến: **4 đến 6 tháng**.

---

## 5. Kế hoạch triển khai hành động (Next Immediate Actions)

### 🎯 Tác vụ đầu tiên (The One Thing to Do First):
**Backtest duy nhất Baseline 4 (Macro Surprise Rules - Không AI) trên dữ liệu EURUSD 3 năm gần nhất.**
- Code 1 file Python đơn giản.
- Tải lịch kinh tế (CPI, NFP, Lãi suất Fed/ECB) 2021–2024.
- Áp dụng luật phản ứng giá sau 24h trừ 1.5 pips spread.
- **Nếu Baseline 4 này âm tiền nặng:** Chúng ta biết thị trường đã "discount" tin tức cực nhanh và việc giao dịch theo tin tức vĩ mô 1 ngày là bài toán cực kỳ gian nan.
- **Nếu Baseline 4 này có lãi hoặc hòa vốn:** Ta có ngay một mốc benchmark hoàn hảo để AI phải vượt qua.
