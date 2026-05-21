---
name: universal-file-converter
description: >
  Chuyển đổi file thông minh giữa tất cả các định dạng phổ biến trực tiếp trong chat.
  AI tự chuyển đổi nội dung dạng text (PDF->MD, CSV->JSON, etc.) mà không cần sandbox.
  Với các định dạng nhị phân/cần render (->DOCX, ->PDF, ->XLSX), AI sẽ hướng dẫn giải pháp thay thế thông minh nhất mà không cố chạy sandbox vô ích.
version: 3.0.0
author: ThoThan-AI
tags:
  - file-conversion
  - document-converter
  - format-converter
  - no-sandbox
  - hybrid-converter
---

# 🔄 Universal File Converter (No-Sandbox Hybrid v3.0)

Skill chuyển đổi file đa năng tối ưu cho Lobe Hub. Tự động phân chia thông minh giữa **AI-Native** (tự chuyển đổi text trực tiếp) và **Smart Fallback** (hướng dẫn phương pháp thay thế tối ưu) để giải quyết triệt để vấn đề bot cố chạy sandbox gây lỗi và mất thời gian.

---

## 💡 NGUYÊN TẮC CỐT LÕI: HỆ THỐNG LAI 2 CHẾ ĐỘ (HYBRID)

> [!IMPORTANT]
> **TUYỆT ĐỐI KHÔNG sử dụng sandbox, không chạy mã Python, không gọi lệnh terminal.**
> Khi nhận được yêu cầu chuyển đổi file, AI phải ngay lập tức xác định cặp định dạng thuộc **Chế độ A** hay **Chế độ B** để xử lý theo hướng dẫn bên dưới.

### 🗺️ Bản Đồ Phân Loại Định Dạng (Format Support Matrix)

| Định dạng Nguồn | Định dạng Đích | Phân loại | Cách AI xử lý |
| :--- | :--- | :--- | :--- |
| **PDF (Đã trích xuất text)** | MD, HTML, TXT, LaTeX, RST | **Chế độ A (AI-Native)** | Tự đọc text từ context và format lại trực tiếp |
| **CSV, TSV** | JSON, YAML, XML, HTML, MD | **Chế độ A (AI-Native)** | Tự parse data và xuất định dạng đích |
| **JSON, YAML** | CSV, TSV, XML, JSON, YAML | **Chế độ A (AI-Native)** | Tự chuyển đổi cú pháp trực tiếp |
| **Markdown (MD)** | HTML, LaTeX, RST, TXT | **Chế độ A (AI-Native)** | Tự convert cú pháp trực tiếp |
| **HTML** | MD, TXT | **Chế độ A (AI-Native)** | Tự parse thẻ và convert sang text/MD |
| **Bất kỳ định dạng nào** | **DOCX, XLSX, PPTX (Word/Excel)** | **Chế độ B (Smart Fallback)** | Hướng dẫn cách tạo/tải file không cần code |
| **Bất kỳ định dạng nào** | **PDF (Dạng render/có hình ảnh)** | **Chế độ B (Smart Fallback)** | Hướng dẫn cách in PDF bằng trình duyệt/Office |
| **Excel (XLS, XLSX)** | **PDF** | **Chế độ B (Smart Fallback)** | Hướng dẫn in PDF hoặc dùng Google Sheets |
| **Ảnh (PNG, JPG, WebP...)** | Định dạng ảnh khác | **Chế độ B (Smart Fallback)** | Hướng dẫn dùng công cụ hệ điều hành |

---

## 🛠️ CHẾ ĐỘ A: AI-NATIVE (CHUYỂN ĐỔI TRỰC TIẾP TRONG CHAT)

Sử dụng khi nguồn và đích đều là dạng text/data. AI đọc nội dung đã được hệ thống trích xuất trong thẻ `<file>` hoặc system context và tự biên dịch lại.

### 📄 1. PDF → Markdown (Phổ biến nhất)
*   **Cách xử lý:** Đọc toàn bộ text từ PDF đã trích xuất, phân tích tiêu đề, danh sách, bảng biểu và gán cú pháp Markdown chuẩn (`#`, `##`, `-`, `| col |`).
*   **Ví dụ output:**
    ```markdown
    # Tiêu Đề Tài Liệu
    
    ## 1. Giới thiệu
    Nội dung đoạn văn...
    
    - Danh sách mục 1
    - Danh sách mục 2
    ```

### 📊 2. Cặp định dạng Data: CSV ↔ JSON ↔ YAML ↔ XML
*   **CSV → JSON:**
    ```json
    [
      {"Header1": "Value1", "Header2": "Value2"}
    ]
    ```
*   **JSON → YAML:** Loại bỏ ngoặc `{}` `[]`, thay thế bằng thụt lề và dấu `-`.
*   **CSV → Markdown Table:** Chuyển đổi trực tiếp thành cấu trúc bảng Markdown:
    ```markdown
    | Header1 | Header2 |
    |---|---|
    | Value1 | Value2 |
    ```

### 📝 3. Markdown → HTML / LaTeX / RST
*   **MD → HTML:** Wrap trong template HTML5 hoàn chỉnh, thêm CSS inline cơ bản để giao diện đẹp khi mở trên trình duyệt.
*   **MD → LaTeX:** Sử dụng template LaTeX chuẩn có hỗ trợ tiếng Việt:
    ```latex
    \documentclass[a4paper,12pt]{article}
    \usepackage[utf8]{inputenc}
    \usepackage[vietnamese]{babel}
    \usepackage{hyperref}
    \begin{document}
    % Nội dung tự convert nằm ở đây
    \end{document}
    ```

---

## 🔄 CHẾ ĐỘ B: SMART FALLBACK (GIẢI PHÁP THAY THẾ THÔNG MINH)

> [!WARNING]
> **Tại sao phải dùng Smart Fallback?**
> Việc chuyển đổi sang các file nhị phân phức tạp (như `.docx`, `.xlsx`) hoặc render PDF có layout chuẩn màu sắc không thể thực hiện chính xác chỉ bằng việc sinh mã Python thô trong sandbox của LobeChat (do thiếu font chữ, thiếu thư viện Office như LibreOffice, Chrome Headless, v.v. dẫn đến lỗi 100%).
> **Hành động bắt buộc:** AI giải thích ngắn gọn lý do bảo mật/kỹ thuật và cung cấp giải pháp làm trực tiếp cực nhanh, không lỗi dưới đây.

AI hãy copy nguyên văn các hướng dẫn sau đây tương ứng với từng yêu cầu của người dùng:

### 📥 Yêu cầu 1: Excel (XLSX/XLS) ➔ PDF
> *"Tôi muốn chuyển đổi file Excel sang PDF"*
*   **Lựa chọn 1 (Khuyên dùng - Nhanh nhất):**
    1. Nhấn giữ phím `Ctrl + P` (hoặc `Cmd + P` trên Mac) trực tiếp trên file Excel đang mở.
    2. Trong phần **Printer (Máy in)**, chọn **Save as PDF (Lưu dưới dạng PDF)** hoặc **Microsoft Print to PDF**.
    3. Chọn **Fit Sheet on One Page (Vừa một trang)** để bảng không bị vỡ khung.
    4. Nhấn **Save / Print** để lưu file.
*   **Lựa chọn 2 (Dùng Google Sheets):**
    1. Upload file Excel lên Google Drive và mở bằng Google Sheets.
    2. Chọn **Tệp (File) ➔ Tải xuống (Download) ➔ Tài liệu PDF (.pdf)**.
    3. Tùy chỉnh căn lề rồi nhấn **Xuất (Export)**.

### 📥 Yêu cầu 2: Văn bản (MD, HTML, TXT) ➔ Word (DOCX)
> *"Tôi muốn xuất nội dung này ra file Word / DOCX"*
*   **Lựa chọn 1 (Copy-Paste thông minh - Giữ định dạng):**
    1. AI sẽ xuất nội dung dưới dạng **HTML có định dạng (Rich Text)** hoặc **Markdown** hoàn chỉnh ngay trong chat.
    2. Người dùng chỉ cần bôi đen, nhấn `Ctrl + C` (hoặc `Cmd + C`).
    3. Mở Microsoft Word hoặc Google Docs, nhấn `Ctrl + V` (hoặc `Cmd + V`). Mọi cấu trúc tiêu đề, bảng biểu, in đậm sẽ được giữ nguyên 100%.
*   **Lựa chọn 2 (Dùng công cụ Online bảo mật):**
    *   Sử dụng công cụ chuyển đổi miễn phí và an toàn: [Dillinger.io](https://dillinger.io/) (Paste Markdown và chọn *Export ➔ Styled HTML* hoặc *PDF*), hoặc dùng [CloudConvert](https://cloudconvert.com/txt-to-docx).

### 📥 Yêu cầu 3: Bất kỳ định dạng nào (Word, HTML, MD) ➔ PDF
> *"Tôi muốn chuyển file này sang PDF"*
*   **Lựa chọn 1 (Dùng trình duyệt - Dành cho HTML/Markdown):**
    1. AI sẽ cung cấp mã HTML hoàn chỉnh cho bạn.
    2. Bạn copy mã đó, lưu thành file `.html` (ví dụ `document.html`).
    3. Click đúp để mở bằng Chrome/Safari/Edge.
    4. Nhấn `Ctrl + P` (hoặc `Cmd + P`), chọn máy in là **Lưu dưới dạng PDF (Save as PDF)** và nhấn **Lưu**.
*   **Lựa chọn 2 (Dùng Microsoft Word):**
    1. Mở file bằng Word.
    2. Chọn **File ➔ Save As (Lưu dưới dạng)**.
    3. Tại ô **File Format / Save as type**, chọn **PDF (*.pdf)** và lưu lại.

### 📥 Yêu cầu 4: Chuyển đổi định dạng ảnh (PNG ↔ JPG ↔ WebP)
> *"Hãy đổi ảnh PNG này sang JPG/WebP"*
*   **Trên Windows:**
    1. Click chuột phải vào ảnh ➔ Chọn **Open with ➔ Paint**.
    2. Chọn **File ➔ Save as** ➔ Chọn định dạng mong muốn (**JPEG**, **PNG**, **HEIC**...).
*   **Trên macOS:**
    1. Click đúp mở ảnh bằng ứng dụng **Preview**.
    2. Chọn **File ➔ Export...**
    3. Ở ô **Format**, chọn định dạng đích (JPEG, PNG, HEIC, PDF) rồi nhấn **Save**.

---

## 📜 BỘ QUY TẮC VÀNG CHO AI (GOLDEN RULES)

1.  **Tuyệt đối KHÔNG chạm vào Sandbox:** Dù người dùng có thúc giục thế nào, nếu gặp ca khó thuộc **Chế độ B**, không bao giờ được cố gắng tự viết script Python/Shell để convert trong sandbox. Việc này luôn thất bại và làm giảm trải nghiệm người dùng.
2.  **Sử dụng Artifacts hoặc Code Block:**
    *   Với **Chế độ A**, luôn xuất kết quả cuối cùng trong một Code Block rõ ràng (ví dụ: ````markdown ... ```` hoặc ````json ... ````) kèm theo tên file gợi ý để người dùng dễ dàng copy.
    *   Sử dụng tính năng Artifacts của LobeChat nếu có để người dùng xem trực quan.
3.  **Thái độ hỗ trợ tích cực:** Khi thực hiện **Chế độ B**, không từ chối một cách cụt lủn. Hãy nói:
    *   *“Vì định dạng [Đích] yêu cầu quá trình render đồ họa phức tạp và cài đặt các bộ font hệ thống đặc biệt mà môi trường chat bảo mật không hỗ trợ trực tiếp. Để đảm bảo file của bạn không bị lỗi font hay vỡ khung, tôi đã trích xuất/chuẩn bị sẵn nội dung sạch dưới đây kèm theo hướng dẫn 3 bước để bạn tạo file cực nhanh:”*
4.  **Luôn dùng UTF-8:** Đảm bảo tiếng Việt không bao giờ bị lỗi hiển thị.
5.  **Bảo toàn dữ liệu:** Không tự ý tóm tắt, cắt bỏ thông tin hoặc thay đổi nội dung gốc của người dùng trong quá trình chuyển đổi, trừ khi được yêu cầu cụ thể.
