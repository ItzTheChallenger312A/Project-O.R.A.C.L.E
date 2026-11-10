# Project O.R.A.C.L.E. — Experiment Charter & Hypothesis
**Version:** 0.3.0 (Synchronized with Research Charter v0.1.2)  
**Date:** 2026-11-10  
**Author:** Dylan (Project Lead)  
**Status:** Pre-experiment Lock (Đóng băng tiêu chuẩn trước khi thu thập dữ liệu)

---

## 1. Câu hỏi nghiên cứu (Research Question)
> **"Liệu việc sử dụng LLM để giải thích các sự kiện kinh tế vĩ mô có tạo ra lợi thế dự báo (Alpha) thực chất sau khi trừ chi phí giao dịch thực tế (Bid/Ask spread + Slippage) so với các quy tắc vĩ mô (Macro Surprise Rule) và các phương pháp kỹ thuật cơ bản trên cặp EURUSD trong khung thời gian 1 phiên giao dịch hay không?"**

### Các nguyên tắc bất biến (Non-negotiable Constraints):
1. **Zero Real-Money Trading:** Không dùng tiền thật dưới mọi hình thức.
2. **Immutable Audit Trail:** SQLite kích hoạt Trigger cấm tuyệt đối lệnh `UPDATE`/`DELETE`. Mỗi prediction chứa hash SHA-256 liên kết chuỗi và được tự động commit lên GitHub để bảo chứng timestamp.
3. **Phân tách Hai trục Nghiên cứu rõ ràng:**
   - **Historical Price Research (Phase 2):** Kiểm nghiệm 3 baseline kỹ thuật không dùng LLM trên dữ liệu giá quá khứ.
   - **Prospective Macro Research (Phase 4+):** Thu thập dữ liệu vĩ mô và consensus theo thời gian thực (Point-in-Time) để kiểm nghiệm LLM, triệt tiêu rủi ro Data Leakage.
4. **Data-First & Dynamic Cost:** Đo lường chi phí trực tiếp từ Bid/Ask thực tế, không dùng số liệu ước tính cố định thiếu cơ sở.

---

## 2. Thiết lập kỹ thuật (Technical Specs)

| Thuộc tính | Lựa chọn chốt | Lý do khoa học |
| :--- | :--- | :--- |
| **Cặp tài sản (Instrument)** | `EURUSD` | Cặp tiền thanh khoản cao nhất, spread mỏng nhất, phản ánh tương quan chính sách Fed vs. ECB. |
| **Khung thời gian (Horizon)** | `1 Trading Session — Snapshot-to-Snapshot` | Snapshot: 15:30 `America/New_York`, Prediction window: 15:30–15:35, Friday/Weekend tách cohort riêng. |
| **Mô hình Chi phí** | `Dynamic Bid/Ask Spread + Volatility Slippage` | Đo lường trực tiếp từ báo giá thực tế của dataset (Long: Buy Ask/Sell Bid; Short: Sell Bid/Buy Ask). |
| **Cơ chế lưu trữ** | `SQLite Append-Only` | Có SQLite Trigger chặn UPDATE/DELETE + SHA-256 hash validation + Git remote push. |

---

## 3. Hệ thống 4 Baselines (Hệ quy chiếu đối thủ)

| Baseline | Nguồn dữ liệu | Cơ chế hoạt động | Giai đoạn triển khai |
| :--- | :--- | :--- | :--- |
| **B1: Random Direction** | Random seed | Đoán ngẫu nhiên có kiểm soát | Phase 2 (Historical) |
| **B2: Historical Base Rate** | Historical price | Tỷ lệ nến tăng/giảm trong 252 phiên rolling | Phase 2 (Historical) |
| **B3: Previous Trend** | Historical price | Xu hướng phiên liền trước | Phase 2 (Historical) |
| **B4: Macro Surprise Engine** | Actual vs. Consensus PIT | Bộ luật phản ứng kinh tế vĩ mô chuẩn hóa | Phase 4 trở đi (Prospective) |

---

## 4. Khung Nghiên cứu Thăm dò & Xác định MDE (Exploratory Framework)

- **Không ấn định trước ngưỡng Hit Rate cứng:** 
  Dự án khởi động với tư cách là nghiên cứu thăm dò (Exploratory Research).
- **Quy trình xác định MDE (Minimum Detectable Effect):**
  1. Trong Phase 2, đo lường độ biến động thực tế ($\sigma_{\text{daily}}$), phân phối lợi nhuận và spread thực tế từ historical data của EURUSD.
  2. Dựa vào các tham số thực nghiệm đó, tài liệu phương pháp luận sẽ tính toán chính xác MDE và kích thước mẫu ($N$) cần thiết để đạt đủ năng lực thống kê (Statistical Power $\ge 80\%$).
  3. Khóa các ngưỡng mục tiêu định lượng chính thức trước khi bước vào Phase 5 (Forward-test).
