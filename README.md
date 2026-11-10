# Project O.R.A.C.L.E.
**Observation, Reasoning, Analysis, Calibration & Logging Engine**

> **Research before execution.**  
> *Một chương trình nghiên cứu định lượng nhằm kiểm chứng xem LLM có thực sự mang lại lợi thế dự báo (Alpha) vĩ mô trên thị trường tài chính hay không.*

---

## 📌 Tổng quan dự án (Overview)
Project O.R.A.C.L.E. không phải là một bot giao dịch tự động đặt lệnh để kiếm tiền nhanh. Đây là một **Hệ thống Nghiên cứu & Đánh giá Trí tuệ Thị trường (Market Intelligence & Quantitative Research System)** được thiết kế để trả lời một câu hỏi cốt lõi:

> *"Liệu việc sử dụng LLM để phân tích và diễn giải các sự kiện kinh tế vĩ mô có tạo ra lợi thế dự báo xu hướng giá (sau chi phí giao dịch) tốt hơn các phương pháp đơn giản không dùng AI hay không?"*

Mọi nhận định và dự đoán trong dự án đều được ghi nhận theo thời gian thực (Forward-testing), gắn dấu thời gian bất biến và đối chứng với 4 baseline độc lập trước khi cân nhắc bất kỳ cơ chế đặt lệnh nào.

---

## 🛑 Nguyên tắc bất biến (Core Constraints)
1. **Zero Real-Money Trading:** Tuyệt đối không giao dịch tiền thật trong toàn bộ giai đoạn nghiên cứu.
2. **Immutable Predictions:** Mọi dự đoán một khi đã ghi nhận kèm snapshot đầu vào thì không được phép chỉnh sửa hoặc xóa bỏ.
3. **No Historical Backtesting on LLM:** Tránh hoàn toàn rủi ro rò rỉ dữ liệu (Data Leakage) bằng cách chỉ forward-test LLM trên các sự kiện tương lai.
4. **Data-First & Evidence-Based:** Không tối ưu token, không xây dựng giao diện phức tạp trước khi chứng minh được giá trị thống kê.

---

## 🗺️ Lộ trình nghiên cứu tổng thể (8 Phases)

```
01. Feasibility (Tính khả thi)
     ↓
02. Data Research (Nghiên cứu dữ liệu nguồn)
     ↓
03. Baseline Research (Kiểm nghiệm 3 Baseline giá lịch sử)
     ↓
[ CHECKPOINT 12 HOURS ] ──→ Quyết định GO / NO-GO
     ↓
04. Prospective Macro Data (Thu thập dữ liệu vĩ mô thực tế)
     ↓
05. AI Intelligence Experiment (Thử nghiệm suy luận LLM & Output có cấu trúc)
     ↓
06. Comparative Evaluation (So sánh đối chứng AI vs 4 Baselines)
     ↓
07. Final Research Review (Báo cáo tổng kết & Kết luận khoa học)
```

---

## 📂 Trạng thái hiện tại (Current Status)
- **Phase:** Phase 0 — Research Foundation & Feasibility
- **Instrument:** `EURUSD`
- **Horizon:** 1 Trading Day (24h)
- **Initial Time Checkpoint:** 12 Giờ
- **LLM Budget hiện tại:** $0.00 USD (Chưa tích hợp LLM cho đến khi hoàn thành Baseline)

---

## 📚 Cấu trúc tài liệu (Documentation Tree)
- `README.md` *(Tài liệu hiện tại)*
- `00-project-foundation/` *(Đang triển khai)*
  - `research_charter.md` — Hiến chương nghiên cứu & tiêu chí đánh giá
  - `research_plan.md` — Kế hoạch chi tiết v0.1.2
  - `research_scope.md` — Phạm vi và giới hạn dự án
  - `research_budget.md` — Ngân sách thời gian và chi phí
  - `decision_log.md` — Nhật ký các quyết định kiến trúc
  - `assumptions_register.md` — Danh sách các giả định cần kiểm chứng
