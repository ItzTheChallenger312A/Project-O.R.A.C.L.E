# Phase 0 Research Workstreams: P0-09, P0-10, P0-11

**Document:** ORACLE Phase 0 Research Expansion  
**Date:** 09 November 2026  
**Status:** Approved for Research Exploration  

---

## 1. P0-09: Macro-to-Market Mapping (`MacroMarketMapper`)

### 1.1 Nguyên lý Cốt lõi
- **Không suy diễn cứng nhắc:** Thị trường không phản ứng theo quan hệ cứng $A \rightarrow B$, mà phản ứng dựa trên:
  1. **Mức độ bất ngờ (Surprise Delta):** $\text{Actual} - \text{Forecast}$ so với độ lệch chuẩn lịch sử.
  2. **Bối cảnh định giá trước sự kiện (Priced-in expectations):** Tâm lý và vị thế thị trường trước thời điểm ra tin.
  3. **Thành phần chi tiết:** Ví dụ CPI tổng thể vs CPI lõi, cấu phần dịch vụ, điều chỉnh kỳ trước (*previous revised*).
  4. **Quan hệ liên thị trường (Cross-Asset Hypotheses):** Mối quan hệ giữa DXY, Lợi suất trái phiếu (US Yields), XAUUSD, EURUSD, Dầu chỉ là **giả thuyết có điều kiện**, không phải quy luật cố định.

### 1.2 Thiết kế Module `MacroMarketMapper`
- **Đầu vào (Inputs):**
  - Sự kiện kinh tế / địa chính trị.
  - Số liệu: `Actual`, `Forecast`, `Previous`, `Revised`.
  - Độ lệch bất ngờ (*Surprise factor*).
  - Timestamp công bố & bối cảnh thị trường trước giờ G.
- **Đầu ra (Outputs):**
  - Danh sách tài sản bị ảnh hưởng tiềm năng.
  - Hướng tác động kỳ vọng & biên độ biến động dự kiến.
  - Thời hạn tác động: Ngắn hạn (M15–H1), Trung hạn (H4–D1), Dài hạn.
  - Điều kiện đảo ngược luận điểm (*Invalidation factors*).
  - Độ tin cậy & nguồn dữ liệu.
- **Vòng lặp kiểm chứng thực nghiệm (Empirical Feedback Loop):**
  - Lưu trữ biến động thực tế của thị trường tại các mốc (1m, 15m, 1h, 4h) sau tin.
  - Đối chiếu thực tế với giả thuyết ban đầu để hiệu chuẩn (calibration) prompt và mô hình LLM, tránh tình trạng LLM hallucinate hoặc thiên kiến.

---

## 2. P0-10: Token Optimization Experiment

### 2.1 Nguyên tắc
> **Tối ưu token không phải là mục tiêu cuối cùng. Mục tiêu là tối đa hóa chất lượng phân tích trên mỗi token được sử dụng.**

### 2.2 Thiết kế Thí nghiệm 3 Cấp độ
- **Experiment A (Full Context Baseline):**
  - Đưa toàn bộ nội dung bài báo, biên bản họp, dữ liệu thô vào LLM.
- **Experiment B (Structured Context Optimized):**
  - Trích xuất và cấu trúc hóa dữ liệu thành JSON chuẩn: giữ lại headline, MoM, YoY, sub-components quan trọng, điều chỉnh kỳ trước.
- **Experiment C (Minimal Context Token-Efficient):**
  - Chỉ gửi các giá trị delta toán học then chốt kèm trạng thái regime hiện tại.

### 2.3 Khung Đo lường & Đánh giá
| Chỉ số | Mục tiêu đo lường |
|---|---|
| **Input Tokens / Output Tokens** | Chi phí tài nguyên mô hình |
| **Latency (ms)** | Tốc độ phản hồi tín hiệu |
| **JSON Validity Rate** | Độ tuân thủ schema đầu ra của LLM |
| **Directional Accuracy** | Độ chính xác dự phóng xu hướng |
| **Calibration** | Mức độ tương quan giữa Confidence Score và kết quả thực tế |
| **Decision Consistency** | Tính ổn định khi chạy nhiều lần trên cùng dữ liệu |

---

## 3. P0-11: Human-in-the-Loop & Telegram Interface Architecture

### 3.1 Pipeline Luồng Tín hiệu & Quyết định
```
ORACLE Intelligence 
       ↓
Signal Generator 
       ↓
Independent Risk Engine (Kiểm tra kỹ thuật: Lot size, Margin, Max Drawdown)
       ↓
Telegram Notification (Gửi Interactive Signal Card)
       ↓
Human Approval (Approve / Reject / Modify)
       ↓
Revalidate Proposal (Kiểm tra lại trượt giá & biến động trước khi gửi lệnh)
       ↓
MT5 Execution Adapter (Demo / Paper execution)
       ↓
Execution Report (Gửi xác nhận ngược lại Telegram/Dashboard)
```

### 3.2 Quy tắc Hệ thống Bắt buộc
1. **Unique Proposal ID (`proposal_id`):** Mỗi đề xuất có ID duy nhất, ngăn chặn trùng lặp.
2. **Idempotency & Single Approval:** Ngăn chặn click đúp hoặc duyệt nhiều lần một lệnh.
3. **Auto-Expiration (TTL):** Đề xuất có hạn hiệu lực, tự động hủy nếu quá thời gian cho phép mà chưa duyệt.
4. **Price Drift Guard:** Nếu giá thị trường đã chạy quá biên độ quy định trước khi trader bấm Approve, hệ thống bắt buộc tính toán và xác nhận lại.
5. **Recheck on Modification:** Bất kỳ điều chỉnh nào của trader về Entry, SL, TP đều phải được Risk Engine phê duyệt lại trước khi kích hoạt.
6. **Bảo mật tuyệt đối:** Telegram Bot là giao diện trình diễn và tương tác, **tuyệt đối không nắm giữ Broker API Credentials**.
