---
name: project-task-rules
description: >-
  Use this skill whenever the user asks to implement, change, add, fix,
  refactor, configure, document, test, or otherwise modify files in a project
  codebase. Apply it even when the user does not name this skill. Enforce
  minimal scope, pre-edit inspection, appropriate verification, and the
  required Vietnamese implementation report. Do not use for pure questions,
  brainstorming, or read-only investigation.
---

# Project Task Rules

Use this skill for implementation, bug-fix, configuration, documentation, and maintenance tasks that change project files.

## Operating Rules

| Area | Required behavior |
| --- | --- |
| Before editing | In commentary, state the task goal; existing files expected to change and why; new files expected to be created and why; and API, database, public-contract, or dependency impact when applicable. |
| Codebase alignment | Inspect relevant local code first. Follow its naming, error handling, dependency injection, structure, and test patterns. |
| Scope | Keep changes strictly within the requested task. Report unrelated defects; do not fix or refactor them without approval. |
| Minimal change | Prefer the smallest compatible change. Do not rewrite files, add abstractions, rename code, reorganize folders, or alter architecture unless required. |
| Formatting | Do not run formatting tools or leave format-only code changes when there is no related functional change. When formatting is required by validation, limit it to files changed for the task. |
| Dependencies | Do not add dependencies without first naming the package and explaining why an existing dependency cannot solve the task. |
| Compatibility | Preserve backward compatibility and public contracts by default. State the impact before changing an API request/response, DTO, function signature, database schema, environment variable, or observable behavior. |
| Reporting | Report every deletion, rename, API change, interface change, schema change, and behavior change in the final response. |
| Avoid unused code | Do not create unused helpers, interfaces, DTOs, configuration, or duplicate utilities. |
| Verification | Tự động chạy các kiểm tra phù hợp (TypeScript, linting, unit tests, focused integration tests, builds) mà không cần hỏi lại người dùng. Không được thay đổi test chỉ để pass hành vi sai. Không báo pass khi chưa chạy. Báo cáo các check chưa chạy và lý do. |
| No overconfidence | Đừng quá tự tin vào kết quả mình làm. Tuyệt đối KHÔNG báo cáo khẳng định kiểu đã hoàn thành/sửa 100% hoặc tuyên bố không còn lỗi khi chưa được kiểm chứng thực tế qua lệnh test, build hoặc xác minh cụ thể. Luôn trung thực nêu rõ những gì đã kiểm tra và những gì chưa thể kiểm chứng để người dùng tự đánh giá. |

## Báo Cáo Cuối

Kết thúc mỗi task implementation bằng báo cáo ngắn gọn bằng tiếng Việt có dấu. Luôn gồm `Hoàn Thành Task`, `Công Việc Thực Hiện`, `Files & Thay Đổi` và `Xác Minh`. Định dạng các bảng báo cáo bằng **Bảng Markdown Chuẩn (GFM Table)** nguyên bản (không bọc trong khối code block) để tự động co dãn linh hoạt theo kích thước cửa sổ (Responsive), TÊN CỘT BẮT BUỘC IN HOA TOÀN BỘ.

* **Quy tắc hiển thị và phân cách bảng**:
  - **Phân cách dòng bằng hàng đệm trống (Spacer Row)**: BẮT BUỘC chèn một hàng trống đệm (`| &nbsp; | | | |` hoặc `| &nbsp; | |`) ngăn cách giữa các dòng dữ liệu để tạo khoảng cách vật lý rõ ràng, tránh dính chữ giữa các hàng.
  - **Cột TRẠNG THÁI**: Chỉ sử dụng 3 giá trị chuẩn: `CREATE`, `UPDATE`, `DELETE`.
  - **Cột FILE**: Gắn liên kết clickable trực tiếp `[tên_file](file:///...)` ngay bên trong ô của bảng.

| Phần báo cáo | Nội dung bắt buộc |
| --- | --- |
| Công Việc Thực Hiện | Bảng Markdown chuẩn co dãn, tên cột in hoa toàn bộ, có hàng trống đệm `| &nbsp; | |` ngăn cách giữa các dòng, liệt kê các hạng mục công việc và kết quả đạt được. |
| Files & Thay Đổi | Bảng Markdown chuẩn co dãn, tên cột in hoa toàn bộ, có hàng trống đệm `| &nbsp; | | | |` ngăn cách giữa các dòng, cột `TRẠNG THÁI` dùng `CREATE`/`UPDATE`/`DELETE`, cột `FILE` gắn link clickable trực tiếp. |
| Thay Đổi Hành Vi | Chỉ thêm khi có thay đổi có thể quan sát; nêu trước và sau. |
| Ảnh Hưởng API | Chỉ thêm khi endpoint hoặc API contract thay đổi. |
| Ảnh Hưởng Database | Chỉ thêm khi bảng, cột, quan hệ hoặc migration thay đổi. |
| Dependency Đã Thêm | Chỉ thêm khi có package mới. |
| Xác Minh | TypeScript, lint, test, build và trạng thái thực tế. |
| Chưa Xác Minh | Chỉ thêm khi có check hoặc flow liên quan chưa chạy. |
| Vấn Đề Phát Hiện | Chỉ thêm khi phát hiện vấn đề ngoài phạm vi. |
| Hạn Chế Đã Biết | Chỉ thêm khi có workaround hoặc giới hạn kỹ thuật. |
| Gợi Ý Công Việc Tiếp Theo | Chỉ thêm khi có công việc cụ thể, hữu ích và ĐẢM BẢO CÓ THỂ TRIỂN KHAI LIỀN ĐƯỢC NGAY. Nếu không có việc nào khả thi để làm ngay thì BỎ QUA mục này (không cố thêm cho đủ template). Tuyệt đối không tự ý thực hiện khi người dùng chưa yêu cầu rõ ràng. |

```markdown
## Hoàn Thành Task

Mô tả ngắn gọn kết quả tổng thể của task vừa thực hiện.

### Công Việc Thực Hiện

| CÔNG VIỆC | KẾT QUẢ |
| :--- | :--- |
| **Công việc 1** | Kết quả đạt được 1 |
| &nbsp; | |
| **Công việc 2** | Kết quả đạt được 2 |

### Files & Thay Đổi

| FILE | TRẠNG THÁI | THAY ĐỔI CHI TIẾT | ẢNH HƯỞNG |
| :--- | :---: | :--- | :---: |
| [`path/to/file1`](file:///absolute/path/to/file1) | `CREATE` | Mô tả thay đổi 1 | `Không` |
| &nbsp; | | | |
| [`path/to/file2`](file:///absolute/path/to/file2) | `UPDATE` | Mô tả thay đổi 2 | `Không` |
| &nbsp; | | | |
| [`path/to/file3`](file:///absolute/path/to/file3) | `DELETE` | Mô tả thay đổi 3 | `Không` |

### Xác Minh

* TypeScript: PASS / FAIL / CHƯA CHẠY / KHÔNG ÁP DỤNG
* Lint: PASS / FAIL / CHƯA CHẠY / KHÔNG ÁP DỤNG
* Tests: PASS / FAIL / CHƯA CHẠY / KHÔNG ÁP DỤNG
* Build: PASS / FAIL / CHƯA CHẠY / KHÔNG ÁP DỤNG

### Gợi Ý Công Việc Tiếp Theo (Chỉ ghi khi có thể triển khai liền, không có thì bỏ qua)

* [Tên công việc]: Mô tả cụ thể việc có thể bắt tay vào triển khai liền và giá trị mang lại.
```

Tuyệt đối không tự ý thực hiện bước gợi ý tiếp theo cho đến khi người dùng yêu cầu rõ ràng.
