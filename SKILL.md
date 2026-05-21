---
name: universal-file-converter
description: >
  Chuyển đổi file giữa tất cả các định dạng phổ biến TRỰC TIẾP trong chat, KHÔNG cần sandbox hay tool bên ngoài.
  Sử dụng skill này khi người dùng yêu cầu convert, chuyển đổi, export, xuất file từ bất kỳ định dạng nào
  sang định dạng khác. AI sẽ đọc nội dung file đã được trích xuất và tự format lại sang định dạng đích.
  Hỗ trợ: Markdown (md), DOCX, PDF, HTML, TXT, LaTeX, CSV, JSON, YAML, XML, RST, EPUB.
  Use when user says: "chuyển đổi file", "convert file", "xuất ra", "đổi sang", "chuyển sang md",
  "markdown to html", "pdf to markdown", "csv to json", "chuyển định dạng", "export as", "sang docx".
version: 2.0.0
author: ThoThan-AI
tags:
  - file-conversion
  - document-converter
  - format-converter
  - no-sandbox
---

# 🔄 Universal File Converter (No-Sandbox)

Skill chuyển đổi file đa năng — AI tự xử lý trực tiếp, KHÔNG cần sandbox, KHÔNG cần tool bên ngoài.

## Nguyên Tắc Cốt Lõi

**QUAN TRỌNG: KHÔNG sử dụng sandbox, KHÔNG chạy lệnh shell, KHÔNG gọi Python script.**

AI sẽ:
1. Đọc nội dung file đã được trích xuất từ file đính kèm (system context / file tags)
2. Tự xử lý và format lại nội dung sang định dạng đích
3. Xuất kết quả trực tiếp dưới dạng file hoặc code block để người dùng copy/download

## Khi Nào Sử Dụng

Sử dụng skill này khi người dùng:
- Upload file và yêu cầu chuyển sang định dạng khác
- Yêu cầu export/xuất nội dung sang format khác
- Paste nội dung text và muốn chuyển đổi format
- Yêu cầu batch convert nội dung

## Quy Trình Chuyển Đổi

### Bước 1: Xác định Input
- Nội dung file đã được hệ thống trích xuất tự động khi user upload (trong `<file>` tags hoặc system context)
- Hoặc nội dung text user paste trực tiếp trong chat
- Xác định định dạng nguồn từ phần mở rộng file hoặc cấu trúc nội dung

### Bước 2: Xác định Output
- Hỏi rõ nếu user chưa chỉ định định dạng đích
- Xác nhận yêu cầu đặc biệt (có mục lục không, styling, v.v.)

### Bước 3: Chuyển đổi trực tiếp
- AI tự xử lý text và format lại
- Xuất kết quả trong code block hoặc tạo file artifact
- KHÔNG bao giờ cần sandbox

## Hướng Dẫn Chuyển Đổi Theo Từng Loại

---

### 📄 PDF → Markdown

Khi user upload PDF, hệ thống đã tự trích xuất text trong `<file>` tags. AI cần:

1. Đọc toàn bộ text đã trích xuất từ các trang
2. Phân tích cấu trúc: tiêu đề, đoạn văn, danh sách, bảng
3. Format lại thành Markdown chuẩn:
   - Tiêu đề lớn → `# Heading 1`
   - Tiêu đề phụ → `## Heading 2`, `### Heading 3`
   - Danh sách → `- item` hoặc `1. item`
   - Bảng → Markdown table `| col1 | col2 |`
   - In đậm → `**bold**`
   - In nghiêng → `*italic*`
   - Link → `[text](url)`
   - Code → `` `code` `` hoặc code block
4. Xuất kết quả trong code block ```markdown hoặc tạo file .md

**Ví dụ output:**
```markdown
# Tiêu Đề Tài Liệu

## Phần 1: Giới Thiệu

Nội dung đoạn văn đầu tiên...

### 1.1 Chi tiết

- Mục 1
- Mục 2

| Cột A | Cột B | Cột C |
|-------|-------|-------|
| Data  | Data  | Data  |
```

---

### 📄 PDF → HTML

1. Đọc text trích xuất từ PDF
2. Format thành HTML5 hoàn chỉnh:

```html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>Tiêu đề tài liệu</title>
</head>
<body>
    <h1>Tiêu đề</h1>
    <p>Nội dung...</p>
    <table>
        <tr><th>Cột A</th><th>Cột B</th></tr>
        <tr><td>Data</td><td>Data</td></tr>
    </table>
</body>
</html>
```

---

### 📄 PDF → TXT

1. Đọc text trích xuất từ PDF
2. Loại bỏ mọi formatting, giữ lại plain text
3. Giữ nguyên line breaks và spacing hợp lý
4. Xuất trong code block

---

### 📝 Markdown → HTML

Chuyển đổi trực tiếp theo mapping:

| Markdown | HTML |
|----------|------|
| `# Heading` | `<h1>Heading</h1>` |
| `## Heading` | `<h2>Heading</h2>` |
| `**bold**` | `<strong>bold</strong>` |
| `*italic*` | `<em>italic</em>` |
| `- item` | `<ul><li>item</li></ul>` |
| `1. item` | `<ol><li>item</li></ol>` |
| `[text](url)` | `<a href="url">text</a>` |
| `` `code` `` | `<code>code</code>` |
| `> quote` | `<blockquote>quote</blockquote>` |
| `![alt](src)` | `<img src="src" alt="alt">` |
| `| table |` | `<table>...</table>` |

Wrap trong HTML5 template đầy đủ với `<head>`, charset UTF-8, và CSS cơ bản.

---

### 📝 Markdown → LaTeX

Chuyển đổi trực tiếp:

| Markdown | LaTeX |
|----------|-------|
| `# Heading` | `\section{Heading}` |
| `## Heading` | `\subsection{Heading}` |
| `### Heading` | `\subsubsection{Heading}` |
| `**bold**` | `\textbf{bold}` |
| `*italic*` | `\textit{italic}` |
| `- item` | `\begin{itemize}\item ...\end{itemize}` |
| `1. item` | `\begin{enumerate}\item ...\end{enumerate}` |
| `[text](url)` | `\href{url}{text}` |
| `` `code` `` | `\texttt{code}` |
| code block | `\begin{verbatim}...\end{verbatim}` |
| `> quote` | `\begin{quote}...\end{quote}` |

Wrap trong LaTeX document class đầy đủ:
```latex
\documentclass[a4paper,12pt]{article}
\usepackage[utf8]{inputenc}
\usepackage[vietnamese]{babel}
\usepackage{hyperref}
\begin{document}
% nội dung ở đây
\end{document}
```

---

### 📝 Markdown → RST (reStructuredText)

| Markdown | RST |
|----------|-----|
| `# Heading` | Heading + `=====` underline |
| `## Heading` | Heading + `-----` underline |
| `**bold**` | `**bold**` |
| `*italic*` | `*italic*` |
| `- item` | `* item` |
| `[text](url)` | `` `text <url>`_ `` |
| `` `code` `` | ` `` code `` ` |

---

### 📝 Markdown → TXT

1. Loại bỏ tất cả Markdown syntax (`#`, `**`, `*`, `[]()`, etc.)
2. Giữ lại plain text content
3. Giữ line breaks và indent hợp lý

---

### 🌐 HTML → Markdown

1. Parse cấu trúc HTML
2. Chuyển ngược mapping HTML → Markdown (xem bảng ở mục Markdown → HTML)
3. Loại bỏ các tags không có tương đương Markdown
4. Giữ nội dung text

---

### 🌐 HTML → TXT

1. Strip tất cả HTML tags
2. Giữ lại text content
3. Decode HTML entities (`&amp;` → `&`, `&lt;` → `<`, etc.)

---

### 📊 CSV → JSON

Chuyển đổi trực tiếp:

**Input CSV:**
```csv
name,age,city
Alice,30,Hanoi
Bob,25,HCMC
```

**Output JSON:**
```json
[
  {"name": "Alice", "age": "30", "city": "Hanoi"},
  {"name": "Bob", "age": "25", "city": "HCMC"}
]
```

Quy tắc:
- Dòng đầu tiên = keys
- Mỗi dòng sau = một object
- Wrap trong array `[]`

---

### 📊 CSV → YAML

**Input CSV:**
```csv
name,age,city
Alice,30,Hanoi
```

**Output YAML:**
```yaml
- name: Alice
  age: "30"
  city: Hanoi
```

---

### 📊 CSV → XML

**Output XML:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<data>
  <record>
    <name>Alice</name>
    <age>30</age>
    <city>Hanoi</city>
  </record>
</data>
```

---

### 📊 CSV → HTML Table

**Output:**
```html
<table>
  <thead>
    <tr><th>name</th><th>age</th><th>city</th></tr>
  </thead>
  <tbody>
    <tr><td>Alice</td><td>30</td><td>Hanoi</td></tr>
  </tbody>
</table>
```

---

### 📊 CSV → Markdown Table

**Output:**
```markdown
| name | age | city |
|------|-----|------|
| Alice | 30 | Hanoi |
```

---

### 📊 JSON → CSV

1. Nếu JSON là array of objects: keys = headers, values = rows
2. Nếu JSON là nested: flatten hoặc hỏi user cách xử lý

---

### 📊 JSON → YAML

Chuyển đổi cú pháp trực tiếp:
- `{}` → mapping
- `[]` → sequence
- `"key": "value"` → `key: value`
- Bỏ dấu ngoặc, thêm indent

---

### 📊 JSON → XML

- Object → element
- Array → repeated elements
- Key → tag name
- Value → text content

---

### 📊 YAML → JSON

Ngược lại với JSON → YAML:
- mapping → `{}`
- sequence → `[]`
- `key: value` → `"key": "value"`

---

### 📊 XML → JSON

- Elements → keys
- Text content → values
- Attributes → `@attribute` keys
- Repeated elements → arrays

---

### 📊 TSV ↔ CSV

- TSV dùng tab `\t` separator
- CSV dùng comma `,` separator
- Đổi delimiter trực tiếp

---

## Xử Lý Đặc Biệt

### Bảng biểu (Tables)
- Nhận diện bảng từ PDF/text dựa trên alignment và spacing
- Format thành Markdown table hoặc HTML table tùy output format
- Giữ nguyên cấu trúc cột/hàng

### Tiếng Việt
- Luôn dùng encoding UTF-8
- Giữ nguyên dấu tiếng Việt
- Với LaTeX: thêm `\usepackage[vietnamese]{babel}`

### File lớn
- Nếu nội dung quá dài, chia thành nhiều phần
- Thông báo cho user biết đang xử lý phần nào
- Xuất từng phần hoặc tạo file artifact

### Metadata
- Giữ lại metadata gốc nếu có (title, author, date)
- Thêm vào header của output format tương ứng:
  - Markdown: YAML frontmatter `---`
  - HTML: `<meta>` tags
  - LaTeX: `\title{}`, `\author{}`, `\date{}`

## Quy Tắc Quan Trọng

1. **KHÔNG BAO GIỜ dùng sandbox** — xử lý mọi thứ trực tiếp trong chat
2. **KHÔNG BAO GIỜ chạy lệnh shell** — không pandoc, không python script
3. **KHÔNG BAO GIỜ yêu cầu cài đặt tool** — skill này độc lập hoàn toàn
4. **Đọc nội dung từ system context** — file upload đã được trích xuất sẵn
5. **Xuất kết quả trực tiếp** — trong code block hoặc tạo file artifact để download
6. **Giữ nguyên nội dung** — không thêm, không bớt, không sửa nội dung gốc
7. **Giữ cấu trúc** — heading, list, table, bold/italic phải được preserve
8. **Encoding UTF-8** — luôn luôn, đặc biệt quan trọng cho tiếng Việt
9. **Hỏi rõ nếu mơ hồ** — nếu không rõ format đích, hỏi trước khi convert
10. **Thông báo kết quả** — cho user biết đã convert xong, format gì, bao nhiêu nội dung

## Ví Dụ Sử Dụng

### Ví dụ 1: PDF → Markdown
**User:** "Chuyển file báo cáo.pdf sang markdown"
**Agent:** Đọc nội dung PDF từ system context → phân tích cấu trúc → format thành Markdown → xuất code block ```markdown

### Ví dụ 2: CSV → JSON
**User:** Upload file data.csv, "Chuyển sang JSON"
**Agent:** Đọc CSV từ context → parse headers và rows → tạo JSON array → xuất code block ```json

### Ví dụ 3: Markdown → HTML
**User:** Paste nội dung Markdown, "Export sang HTML"
**Agent:** Parse Markdown syntax → chuyển thành HTML5 tags → wrap trong template → xuất code block ```html

### Ví dụ 4: JSON → YAML
**User:** Upload config.json, "Đổi sang YAML"
**Agent:** Đọc JSON từ context → chuyển syntax sang YAML → xuất code block ```yaml
