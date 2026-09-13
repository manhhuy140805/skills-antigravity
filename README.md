# Antigravity Skills Collection

Bộ sưu tập các kỹ năng tùy chỉnh (Custom Skills) dành cho **Google Antigravity** và **Gemini CLI**, giúp trợ lý AI tự động hóa quy trình làm việc, chuẩn hóa coding style, quản lý Git và tích hợp hệ thống.

---

## 📂 Vị Trí Cài Đặt (Installation Paths)

Antigravity hỗ trợ tải skills từ 2 phạm vi: **Global (Toàn cục)** và **Workspace (Theo từng dự án)**.

### 1. Cài đặt Toàn Cục (Global Scope - Khuyến nghị)
Áp dụng cho mọi dự án và phiên làm việc trên máy của bạn.

* **Linux / macOS**: `~/.gemini/config/skills/`
* **Windows**: `%USERPROFILE%\.gemini\config\skills\` (hoặc `$HOME\.gemini\config\skills\`)

### 2. Cài đặt Theo Dự Án (Workspace Scope)
Áp dụng riêng cho từng repository dự án cụ thể.

* Đặt tại: `<project-root>/.agents/skills/` hoặc `<project-root>/.gemini/skills/`

---

## 🚀 Hướng Dẫn Cài Đặt & Sử Dụng

### Cách 1: Clone trực tiếp làm Global Skills (Khuyến nghị)

Chạy lệnh sau trên Terminal để clone toàn bộ repository về đúng thư mục cấu hình toàn cục:

#### Trên Linux / macOS:
```bash
# Tạo thư mục config nếu chưa có
mkdir -p ~/.gemini/config

# Clone repository về thư mục skills
git clone https://github.com/manhhuy140805/skills-antigravity.git ~/.gemini/config/skills
```

#### Trên Windows (PowerShell):
```powershell
# Tạo thư mục config nếu chưa có
New-Item -ItemType Directory -Force -Path "$HOME\.gemini\config"

# Clone repository về thư mục skills
git clone https://github.com/manhhuy140805/skills-antigravity.git "$HOME\.gemini\config\skills"
```

---

### Cách 2: Cài đặt cho một dự án cụ thể (Project Workspace)

Nếu bạn muốn chia sẻ skill chung với team qua Git của dự án:

```bash
# Di chuyển vào thư mục dự án của bạn
cd /path/to/your/project

# Tạo thư mục .agents/skills
mkdir -p .agents/skills

# Copy skill mong muốn vào dự án (Ví dụ: git-convention)
cp -r ~/.gemini/config/skills/git-convention .agents/skills/
```

---

### 🔄 Cập Nhật Skills (Update)

Để cập nhật các tính năng và skill mới nhất từ repository:

```bash
# Di chuyển vào thư mục skills
cd ~/.gemini/config/skills

# Kéo bản cập nhật mới nhất
git pull origin main
```

---

## 📋 Danh Sách Các Skill Hiện Có

| Skill | Thư Mục | Mô Tả |
| :--- | :--- | :--- |
| **`frontend-backend-integration`** | [`frontend-backend-integration/`](./frontend-backend-integration/SKILL.md) | Tích hợp Frontend với Backend API (Auth, Profiles, CRUD, File Upload, Search, Pagination). Coi API contract là nguồn sự thật (Read-only). |
| **`general-analysis-report`** | [`general-analysis-report/`](./general-analysis-report/SKILL.md) | Phân tích sâu, đánh giá rủi ro kiến trúc/mã nguồn và xuất báo cáo tiếng Việt đa chiều kèm mức độ kiểm chứng. |
| **`git-convention`** | [`git-convention/`](./git-convention/SKILL.md) | Chuẩn hóa quy trình Git (Conventional Commits, Branch Naming, Pull Requests, Squash & Merge). |
| **`project-task-rules`** | [`project-task-rules/`](./project-task-rules/SKILL.md) | Quy chuẩn triển khai tác vụ code: phạm vi tối thiểu, kiểm thử trước/sau chỉnh sửa và xuất báo cáo hoàn thành chuẩn format ANSI. |
| **`workspace-and-git-rules`** | [`workspace-and-git-rules/`](./workspace-and-git-rules/SKILL.md) | Bảo vệ an toàn dữ liệu nhạy cảm (secrets, env files), tuân thủ ranh giới thao tác trên workspace và Git. |
| **`worktree-setup`** | [`worktree-setup/`](./worktree-setup/SKILL.md) | Thiết lập và quản lý Git Worktree cô lập trong `.worktrees/` để xử lý song song các tác vụ mà không ảnh hưởng nhánh hiện tại. |

---

## 🛠️ Cấu Trúc Thư Mục Chuẩn Của Một Skill

Mỗi skill là một thư mục độc lập chứa file định nghĩa bắt buộc **`SKILL.md`**:

```text
skills/
├── <tên-skill>/
│   ├── SKILL.md          # (Bắt buộc) Chứa YAML frontmatter (name, description) và hướng dẫn
│   ├── scripts/          # (Tùy chọn) Script tiện ích thực thi
│   ├── examples/         # (Tùy chọn) Ví dụ tham khảo
│   ├── references/       # (Tùy chọn) Tài liệu bổ trợ chi tiết
│   └── resources/        # (Tùy chọn) Mẫu template, file cấu hình mẫu
└── README.md             # Tài liệu hướng dẫn tổng quan
```

---

## 💡 Cơ Chế Hoạt Động

* **Progressive Disclosure**: Antigravity chỉ nạp `name` và `description` của skill vào ngữ cảnh ban đầu để tối ưu token. Nội dung đầy đủ của `SKILL.md` chỉ được đọc khi trợ lý AI phát hiện yêu cầu phù hợp với mô tả của skill hoặc khi bạn yêu cầu kích hoạt trực tiếp.
* **Tự động nhận diện**: Sau khi clone về đúng thư mục `~/.gemini/config/skills/`, bạn không cần cấu hình thêm gì cả, Antigravity sẽ tự động phát hiện và sử dụng khi khởi động phiên làm việc mới.
