---
name: agent-workflow
description: >-
  Use this skill whenever the user asks to execute or follow the full end-to-end
  development lifecycle for any task across any project: task ingestion,
  requirements grilling, Git branching, TDD implementation, quality verification
  gates, progress synchronization, and PR creation with automated review loops.
  Apply it even when the user simply asks to "triển khai task tiếp theo", "làm theo
  workflow", or "run agent workflow".
---

# Agent Development Workflow (Universal SOP)

Quy trình làm việc chuẩn mực toàn diện (Standard Operating Procedure) dành cho AI Coding Agent trên mọi dự án phần mềm: từ **Tiếp nhận task** $\rightarrow$ **Thẩm vấn (Grill)** $\rightarrow$ **Tạo nhánh** $\rightarrow$ **TDD Code** $\rightarrow$ **Kiểm chứng chất lượng** $\rightarrow$ **Đồng bộ trạng thái** $\rightarrow$ **Tạo PR & Vòng lặp phản biện**.

---

## 1. Sơ Đồ Tổng Quan Các Giai Đoạn

```text
  [0. Reset & Pull Code] ──► [1. Nhận Task] ──────────► [2. Grill Làm Rõ]
         │                            │                           │
         ▼                            ▼                           ▼
  Clear session (/compact)   Kiểm tra gate/deps         Chốt Design Tree
  git checkout base && pull  Đọc spec & ACs             Facts tự tra trước
                                                                  │
  ┌───────────────────────────────────────────────────────────────┘
  │
  ▼
  [3. Tạo Nhánh Git] ──────► [4. Implement TDD] ──────► [5. Quality Gates]
         │                            │                           │
         ▼                            ▼                           ▼
  git checkout -b <branch>   File budget <= 20-30       Format + Lint + Typecheck
  Tránh commit main          Red -> Green -> Refactor   Tests + Build (100% PASS)
                                                                  │
  ┌───────────────────────────────────────────────────────────────┘
  │
  ▼
  [6. Đồng Bộ & Báo Cáo] ──► [7. Tạo PR & Review Loop]
         │                            │
         ▼                            ▼
  Cập nhật tracking/docs     PR Body chuẩn (Diagram, Evidence, Merge Danger)
  Lưu memory ngữ cảnh        Vòng lặp bot review -> Đề xuất Squash & Merge
```

---

## 2. Chi Tiết Từng Giai Đoạn

### Giai Đoạn 0: Làm Sạch Session & Kéo Code Mới Nhất (Pre-Task Reset)

Trước khi bắt tay vào bất kỳ task mới nào, agent và người dùng **bắt buộc** phải thực hiện 2 thao tác:

1. **Làm sạch phiên làm việc (Clear Session / Fresh Context):**
   - **Mục đích:** Tránh hiện tượng context bloat, cạn token hoặc ảo tưởng (hallucination) từ mã nguồn và thảo luận của task trước.
   - **Hành động:** Chạy lệnh `/compact` hoặc khởi động phiên mới (`agy` fresh session).
   - **Tái nạp ngữ cảnh:** Đọc lại file theo dõi task (`TASK_REPORTS.md` hoặc tài liệu tương đương) và bộ nhớ bền vững `claude-mem`.
2. **Kéo mã nguồn mới nhất (Pull Latest Base Branch):**
   - **Hành động:** Kiểm tra `git status` đảm bảo working tree sạch sẽ, chuyển về nhánh chính (`main`/`master`) và pull code mới:
     ```bash
     git checkout main && git pull origin main
     ```
   - Tuyệt đối không tạo nhánh task mới từ một nhánh cũ đang dở dang hoặc local chưa đồng bộ.

---

### Giai Đoạn 1: Tiếp Nhận & Phân Tích Task (Task Ingestion)

1. **Xác định task cần thực hiện:**
   - Tra cứu tài liệu theo dõi tiến độ của dự án (ví dụ: `implementation-plan.md`, `TASK_REPORTS.md`, backlog issues, hoặc yêu cầu trực tiếp từ prompt).
   - Chọn đúng task chưa hoàn thành theo thứ tự ưu tiên hoặc phụ thuộc.
2. **Kiểm tra Gate & Điều kiện tiên quyết (Prerequisites):**
   - Mọi task hoặc điều kiện mà task hiện tại phụ thuộc (`Depends on`) bắt buộc phải đã hoàn tất.
   - Nếu điều kiện tiên quyết chưa đạt: **DỪNG LẠI** và báo cáo ngay blocker, không nhảy cóc qua task khác khi chưa được cho phép.
3. **Phân tích yêu cầu kỹ thuật (Spec):**
   - Xác định rõ: Mục tiêu (Goal), Phạm vi (Scope), Phần ngoài phạm vi (Out of scope), Tiêu chí nghiệm thu (`AC-1..N`) và Điều kiện hoàn thành (Done when).
   - Tuyệt đối không tự ý làm trước các hạng mục ngoài phạm vi (`Out of scope`).

---

### Giai Đoạn 2: Thẩm Vấn & Làm Rõ Yêu Cầu (Grilling & Clarification)

Áp dụng kỹ thuật Grilling (phối hợp `mattpocock-skills/grilling` hoặc `grill-me`) khi gặp yêu cầu mới, kiến trúc phức tạp hoặc còn điểm mơ hồ:

1. **Nguyên tắc cốt lõi: Facts là việc của Agent, Decisions là quyền của User:**
   - Agent tự tìm kiếm thông tin kỹ thuật trong workspace trước (đọc code, kiểm tra kiểu dữ liệu, schema, cấu hình, dependencies).
   - **Tuyệt đối không** hỏi người dùng những thông tin mà agent có thể tự tra cứu trong codebase.
2. **Xây dựng Cây Quyết Định (Design Tree) & Ranh Giới (Decision Frontier):**
   - Liệt kê toàn bộ các quyết định kiến trúc, contract, edge cases cần chốt trước khi viết code.
3. **Định dạng câu hỏi theo Rounds:**
   - Mỗi câu hỏi phải đi kèm giải pháp khuyến nghị kỹ thuật tối ưu để người dùng có thể chấp thuận nhanh ("Yes"):

   ```text
   ❓ **Q1** - **<Tên Vấn Đề>**: <Bối cảnh và các lựa chọn kỹ thuật>

   ➡️ **Khuyến nghị:** <Phương án kỹ thuật tối ưu nhất kèm lý do>

   ---

   ❓ **Q2** - **<Tên Vấn Đề>**: <Bối cảnh câu hỏi tiếp theo>

   ➡️ **Khuyến nghị:** <Phương án khuyến nghị>
   ```

4. **Điều kiện hoàn thành Grilling:** Không còn giả định ngầm, người dùng đã xác nhận đồng thuận với thiết kế trước khi bắt đầu code.

---

### Giai Đoạn 3: Chuẩn Bị Git & Quản Lý Nhánh (Branch Setup)

1. **Tạo nhánh từ bản mới nhất của nhánh chính:**
   - Luôn fetch code mới nhất trước khi tạo nhánh:
     ```bash
     git fetch origin <base-branch> && git checkout -b <type>/<task-id>-<short-description> origin/<base-branch>
     ```
   - Tiền tố nhánh hợp lệ: `feat/`, `fix/`, `refactor/`, `test/`, `docs/`, `chore/`, `ci/`.
   - **Tuyệt đối không:** Commit trực tiếp vào nhánh `main`/`master` hoặc tạo nhánh từ một nhánh feature cũ chưa được merge.
2. **Bảo vệ dữ liệu nhạy cảm:**
   - Không được chạm vào hoặc đọc file bí mật (`.env`, `.env.*`, API keys, private tokens, certificates) khi chưa được người dùng cho phép rõ ràng.

---

### Giai Đoạn 4: Hiện Thực Hoá Theo TDD (Implement - Red/Green/Refactor)

1. **Ngân sách file (File Budget):**
   - Giữ phạm vi thay đổi gọn gàng, thông thường **8–18 files** và tối đa không vượt quá **20–30 files / task**.
2. **Tôn trọng kiến trúc của dự án:**
   - Tuân thủ cấu trúc thư mục, chuẩn đặt tên, Dependency Injection, quản lý lỗi và cách xử lý dữ liệu hiện có trong dự án.
3. **Quy trình TDD 3 bước:**
   - **Bước 1 (RED):** Viết unit/integration test phản ánh chính xác từng Acceptance Criterion (`AC-1`, `AC-2`, ...). Chạy test và xác nhận test thất bại vì chưa có code xử lý.
   - **Bước 2 (GREEN):** Viết mã nguồn tối thiểu để toàn bộ test cases chuyển sang màu xanh (PASS).
   - **Bước 3 (REFACTOR):** Tối ưu hóa mã nguồn, dọn dẹp biến thừa, tuân thủ strict mode, và che giấu bí mật trong log.

---

### Giai Đoạn 5: Bộ Kiểm Chứng Chất Lượng Bắt Buộc (Quality Gates Verification)

Trước khi tuyên bố hoàn thành task, agent **bắt buộc** phải chạy chuỗi lệnh kiểm thử phù hợp với hệ sinh thái của dự án. Ví dụ đối với dự án Node.js/TypeScript:

```bash
npm run format:check && npm run lint && npm run typecheck && npm test && npm run test:e2e && npm run build
```

| HẠNG MỤC KIỂM TRA | TIÊU CHÍ ĐẠT (PASS CRITERIA) |
| :--- | :--- |
| **Formatting** | Toàn bộ mã nguồn tuân thủ Prettier / linter format |
| **Linting** | 0 lỗi và 0 cảnh báo nghiêm trọng từ linter |
| **Typecheck** | Trình biên dịch kiểu (`tsc --noEmit`, `mypy`, `go vet`, v.v.) không báo lỗi |
| **Unit & Integration Tests** | 100% test suites đều PASS |
| **Build** | Biên dịch/đóng gói dự án thành công không lỗi |

> [!CAUTION]
> Nghiêm cấm đánh dấu hoàn tất task nếu có bất kỳ lệnh kiểm thử nào thất bại hoặc bị bỏ qua mà không có lý do chính đáng.

---

### Giai Đoạn 6: Đồng Bộ Trạng Thái & Báo Cáo Handoff (State Synchronization)

1. **Cập nhật tài liệu tiến độ:**
   - Đánh dấu hoàn thành (`[x]`) trên master task list / implementation plan.
   - Bổ sung báo cáo chi tiết vào tài liệu nhật ký dự án (`TASK_REPORTS.md` hoặc tương đương).
2. **Lưu ngữ cảnh làm việc bền vững:**
   - Lưu tóm tắt kết quả vào bộ nhớ (`claude-mem` hoặc memory file của dự án) để phiên làm việc sau kế thừa được ngay.
3. **Mẫu báo cáo kết thúc lượt:**
   - Trình bày bằng Tiếng Việt có dấu, sử dụng Bảng Markdown chuẩn co dãn, tên cột IN HOA TOÀN BỘ, có hàng đệm trống `| &nbsp; | |` ngăn cách giữa các dòng và gắn link file clickable.

---

### Giai Đoạn 7: Tạo PR & Vòng Lặp Phản Biện Tự Động (PR & Review Loop)

1. **Commit chuẩn mực:**
   - Chỉ commit khi người dùng cho phép rõ ràng:
     `git commit -m "<type>(<scope>): <mô tả súc tích>"`
2. **Mẫu PR Body chuẩn kỹ thuật cao (từ `mattpocock-skills/pr`):**

```markdown
## Summary

<Sơ đồ luồng gọi (Call Tree), Sơ đồ tuần tự (Mermaid Sequence), hoặc Diff Sketch>

## Evidence

- **Before:** <Output hoặc lỗi khi test thất bại trước khi có code>
- **After:** <Output toàn bộ test suites PASS kèm thời gian thực thi>

## Merge Danger

- **Door:** `Two-way` (Dễ dàng rollback, không phá hủy schema/hợp đồng API)
- **Blast Radius:** `Isolated` (Chỉ ảnh hưởng nội bộ module hiện tại)
```

3. **Vòng lặp Review – Phản Biện – Sửa Code (AI Review Loop):**
   - **Khi Reviewer/Bot ĐÚNG:** Sửa code tối thiểu, cập nhật test, chạy lại bộ kiểm chứng, commit fix và phản hồi mã commit SHA vào thread review.
   - **Khi Reviewer/Bot SAI hoặc VI PHẠM RÀNG BUỘC DỰ ÁN (RAM, kiến trúc monolith, YAGNI):** Phản biện kỹ thuật dứt khoát, dẫn chứng mã nguồn và tài liệu kiến trúc. Tuyệt đối không nhắm mắt làm theo gợi ý sai.
4. **Bàn giao:**
   - Sau khi các review threads được resolve và CI xanh toàn bộ, báo cáo cho người dùng và đề xuất **Squash and Merge** (người dùng tự thực hiện merge, AI không tự merge vào nhánh chính).

---

## 3. Checklist Nhanh Dành Cho Agent (Pre-flight & Post-flight)

- [ ] Đã làm sạch session (`/compact` hoặc phiên mới) và pull code mới nhất từ `main`/`master` trước khi làm task?
- [ ] Đã kiểm tra và thỏa mãn mọi task tiên quyết (Prerequisites)?
- [ ] Đã giải quyết triệt để các khúc mắc kỹ thuật qua Grilling trước khi code?
- [ ] Không tự ý làm các mục thuộc `Out of scope`?
- [ ] Giữ số lượng file thay đổi trong giới hạn cho phép ($\le 20\text{--}30$ files)?
- [ ] Đã chạy và vượt qua 100% chuỗi lệnh kiểm thử chất lượng?
- [ ] Đã cập nhật tài liệu theo dõi tiến độ và lưu memory?
- [ ] Không tự ý thực hiện lệnh `git merge` vào nhánh chính?
