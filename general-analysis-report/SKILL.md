---
name: general-analysis-report
description: >-
  Use this skill whenever the user asks to analyze, evaluate, review, compare,
  investigate, assess risks, diagnose, summarize findings, or create a report
  about code, documents, architecture, configuration, logs, data, processes,
  products, or business decisions. Apply it even when the user does not name
  this skill. Produce an evidence-based Vietnamese report with clear
  verification tiers and multi-AI synthesis when multiple sources are supplied.
---

# General Analysis Report Skill

Use this skill whenever you need to create an analysis report for code, documentation, systems, configuration, processes, data, products, or business issues. It is AI-agnostic and specializes in synthesizing insights from multiple AI models/agents into a unified, objective, and verified final report.

## 1. Nguyên Tắc Cốt Lõi (Core Principles)

| Nguyên tắc | Yêu cầu thực thi |
| --- | --- |
| **Xác định mục tiêu & phạm vi** | Luôn làm rõ: mục tiêu phân tích, phạm vi kiểm tra, đối tượng đánh giá và các tiêu chí đo lường cụ thể. |
| **Bằng chứng là trên hết (Evidence-First)** | Không coi kết luận của bất kỳ AI hay giả định nào là sự thật nếu chưa có bằng chứng xác thực trực tiếp. |
| **3 Cấp độ xác minh** | Phải phân định rõ: <br>• **Đã xác minh**: Có bằng chứng trực tiếp từ code, logs, cấu hình hoặc tài liệu gốc. <br>• **Suy luận**: Kết luận logic dựa trên các dữ kiện hiện có nhưng chưa kiểm chứng thực nghiệm. <br>• **Chưa xác minh**: Thiếu dữ liệu hoặc chưa thể kiểm tra. |
| **Tổng hợp đa AI / Multi-Agent** | Khi có dữ liệu từ nhiều AI/agent: ghi nhận nguồn từng bên, đối chiếu điểm đồng thuận và mâu thuẫn, giải quyết mâu thuẫn bằng bằng chứng khách quan, không biểu quyết theo số đông. |
| **Trung thực tuyệt đối** | Tuyệt đối không tự bịa nguồn, số liệu, kết quả kiểm tra hay trạng thái PASS. Ưu tiên dẫn tài liệu chính thống kèm link đối với thông tin biến động hoặc đòi hỏi độ chính xác cao. |
| **Phạm vi an toàn (Read-Only)** | Không tự ý sửa code, sửa tài liệu, thay đổi cấu hình, database hay can thiệp hệ thống nếu chưa được yêu cầu rõ ràng. |
| **Chuẩn mực ngôn ngữ** | Báo cáo hoàn toàn bằng tiếng Việt có dấu, văn phong mạch lạc, khách quan, trung thực và phù hợp với đối tượng tiếp nhận. |
| **Quy chuẩn kẻ bảng** | Tuân thủ `project-task-rules`: Toàn bộ bảng biểu PHẢI dùng định dạng kẻ viền đơn đầy đủ (Box-drawing `┌ ─ ┐ │ ├ ┼ ┤ └ ┴ ┘`) và BẮT BUỘC BỌC TRONG KHỐI CODE BLOCK ` ```ansi ` với TÊN CỘT IN HOA TOÀN BỘ VÀ TÔ MÀU CHỮ bằng mã màu ANSI (ví dụ: `\x1b[1;36m` Cyan, kết thúc bằng `\x1b[0m`), đường viền giữ màu mặc định để font Monospace thẳng hàng tuyệt đối. |

---

## 2. Cấu Trúc Báo Cáo Chuẩn (Standard Report Template)

Mọi báo cáo phân tích theo skill này phải tuân thủ cấu trúc sau:

```markdown
# Báo Cáo Phân Tích

## Tóm Tắt Điều Hành

Nêu kết luận quan trọng nhất, rủi ro chính và khuyến nghị ngắn gọn.

## Mục Tiêu Và Phạm Vi

- **Mục tiêu phân tích**: ...
- **Nội dung đã kiểm tra**: ...
- **Nội dung nằm ngoài phạm vi**: ...

## Nguồn Dữ Liệu Và Agent

```ansi
┌────────────────────────┬────────────────────────────────────────┬──────────────────────┐
│  [1;36mNGUỒN / AGENT [0m          │  [1;36mNỘI DUNG CUNG CẤP [0m                    │  [1;36mĐỘ TIN CẬY / GIỚI HẠN [0m │
├────────────────────────┼────────────────────────────────────────┼──────────────────────┤
│ ...                    │ ...                                    │ ...                  │
└────────────────────────┴────────────────────────────────────────┴──────────────────────┘
```

## Phát Hiện Chính

```ansi
┌────────────────────────┬────────────────────────────────────────┬──────────────────────┐
│  [1;36mPHÁT HIỆN [0m              │  [1;36mBẰNG CHỨNG [0m                           │  [1;36mTRẠNG THÁI [0m           │
├────────────────────────┼────────────────────────────────────────┼──────────────────────┤
│ ...                    │ ...                                    │ Đã xác minh /        │
│                        │                                        │ Suy luận /           │
│                        │                                        │ Chưa xác minh        │
└────────────────────────┴────────────────────────────────────────┴──────────────────────┘
```

## So Sánh Kết Quả Từ Nhiều AI

*(Chỉ thêm khi phân tích dữ liệu từ từ 2 AI/agent trở lên; nếu chỉ có 1 nguồn thì có thể lược bớt)*
- **Điểm các AI đồng thuận**: ...
- **Điểm mâu thuẫn hoặc chưa thống nhất**: ...
- **Cách giải quyết mâu thuẫn dựa trên bằng chứng**: ...

## Phân Tích Nguyên Nhân

*(Chỉ thêm khi có đủ bằng chứng để xác định nguyên nhân hoặc đưa ra các giả thuyết hợp lý có căn cứ)*
...

## Tác Động Và Rủi Ro

Nêu tác động cụ thể đến người dùng, hệ thống, bảo mật, chi phí, vận hành hoặc tiến độ nếu có.

## Khuyến Nghị

Sắp xếp theo mức độ ưu tiên:
1. **Việc cần làm ngay**: ...
2. **Việc nên làm tiếp theo**: ...
3. **Việc cần theo dõi**: ...

## Xác Minh Và Hạn Chế

- **Những gì đã được kiểm tra**: ...
- **Những gì chưa thể kiểm tra**: ...
- **Điều kiện cần có để xác minh tiếp**: ...
```

---

## 3. Quy Tắc Chặn

Tuyệt đối KHÔNG tự ý triển khai các khuyến nghị trong báo cáo cho đến khi nhận được yêu cầu rõ ràng từ người dùng.
