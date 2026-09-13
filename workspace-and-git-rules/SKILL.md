---
name: workspace-and-git-rules
description: >-
  Use this skill whenever a task requires reading workspace files, accessing
  environment or credential files, running Git commands, investigating Git
  history, creating branches, committing, pushing, rebasing, or merging.
  Apply it even when the user does not name this skill. Enforce sensitive-file
  protection and the workspace/Git authorization boundaries.
---

# Workspace and Git Operating Rules

Use this skill to govern tool execution regarding file reading permissions, sensitive credential protection, and Git commands.

## 1. Quyền Đọc File Trong Workspace & Bảo Mật

* **Quyền đọc file thông thường**: Bạn được toàn quyền chủ động đọc tất cả các file mã nguồn, tài liệu và cấu hình trong thư mục/workspace đang làm việc để giải quyết task mà không cần hỏi lại người dùng.
* **Bảo vệ file nhạy cảm / bảo mật (Bắt buộc xin phép)**:
  - Áp dụng cho: file môi trường (`.env`, `.env.*`, `.env.local`, v.v.), secret keys, API credentials, private tokens, private keys (`.pem`, `.key`, v.v.), passwords, database credentials.
  - BẮT BUỘC phải hỏi ý kiến và được người dùng cấp quyền rõ ràng trước khi đọc hoặc truy cập nội dung các file này.

## 2. Quy Định Sử Dụng Git & Điều Tra Code

* **Ưu tiên phân tích code hiện tại**:
  - Khi tìm hiểu code, gỡ lỗi hoặc tìm nguyên nhân bug: Luôn phân tích trực tiếp cấu trúc, logic, DOM, CSS và kiểu dữ liệu hiện có trong mã nguồn của dự án.
  - **Không cần chạy git log / git blame**: Tuyệt đối KHÔNG tự ý tra cứu lịch sử commit (`git log`, `git blame`, `git show`), TRỪ KHI:
    1. Đã đọc và phân tích kỹ lưỡng các file code liên quan hiện tại mà vẫn KHÔNG tìm ra được nguyên nhân;
    2. HOẶC người dùng có yêu cầu rõ ràng về việc tra cứu lịch sử commit.
* **Quy định về thao tác Git (Commit / Push / Branch)**:
  - Chỉ khi được người dùng yêu cầu rõ ràng mới thực hiện các thao tác thay đổi trạng thái Git.
  - Tuyệt đối không tự ý: `git commit`, `git push`, `merge`, `rebase`, tạo/xóa nhánh.
  - Chỉ đề xuất commit message ở cuối task để người dùng tự quyết định.
