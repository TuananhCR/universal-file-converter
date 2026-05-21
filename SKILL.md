---
name: universal-file-converter
description: >
  Chuyển đổi file giữa tất cả các định dạng phổ biến. Sử dụng skill này khi người dùng yêu cầu
  convert, chuyển đổi, export, xuất file từ bất kỳ định dạng nào sang định dạng khác.
  Hỗ trợ: Markdown (md), DOCX, PDF, HTML, TXT, LaTeX, EPUB, RST, ODT, PPTX, XLSX, CSV, JSON, YAML, XML,
  PNG, JPG, SVG, WEBP. Bao gồm chuyển đổi tài liệu, dữ liệu, và hình ảnh.
  Use when user says: "chuyển đổi file", "convert file", "xuất ra pdf", "đổi sang docx",
  "markdown to word", "pdf to markdown", "csv to json", "chuyển định dạng", "export as".
version: 1.0.0
author: ThoThan-AI
tags:
  - file-conversion
  - document-converter
  - pandoc
  - pdf
  - markdown
  - docx
  - format-converter
---

# 🔄 Universal File Converter

Skill chuyển đổi file đa năng — một skill duy nhất xử lý tất cả các chuyển đổi định dạng file.

## Khi Nào Sử Dụng

Sử dụng skill này khi người dùng yêu cầu:
- Chuyển đổi file từ định dạng này sang định dạng khác
- Export/xuất tài liệu sang định dạng khác
- Convert giữa các format: MD, DOCX, PDF, HTML, TXT, CSV, JSON, YAML, EPUB, LaTeX, PPTX, XLSX, hình ảnh...
- Batch convert (chuyển đổi hàng loạt nhiều file)

## Bảng Chuyển Đổi Hỗ Trợ

### Tài liệu (Document Conversions)

| Từ ↓ \ Sang → | MD | DOCX | PDF | HTML | TXT | LaTeX | EPUB | RST | ODT |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Markdown** | — | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **DOCX** | ✅ | — | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **PDF** | ✅* | ✅* | — | ✅* | ✅ | ✗ | ✗ | ✗ | ✗ |
| **HTML** | ✅ | ✅ | ✅ | — | ✅ | ✅ | ✅ | ✅ | ✅ |
| **TXT** | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✗ | ✅ | ✅ |
| **LaTeX** | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ | ✅ |
| **EPUB** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ |
| **RST** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| **ODT** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — |

> ✅* = PDF input cần `pdftotext` hoặc xử lý đặc biệt (xem mục PDF bên dưới)

### Dữ liệu (Data Conversions)

| Từ ↓ \ Sang → | CSV | JSON | YAML | XML | XLSX | TSV |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **CSV** | — | ✅ | ✅ | ✅ | ✅ | ✅ |
| **JSON** | ✅ | — | ✅ | ✅ | ✅ | ✅ |
| **YAML** | ✅ | ✅ | — | ✅ | ✗ | ✅ |
| **XML** | ✅ | ✅ | ✅ | — | ✗ | ✅ |
| **XLSX** | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| **TSV** | ✅ | ✅ | ✅ | ✅ | ✅ | — |

## Quy Trình Chuyển Đổi

### Bước 1: Xác định input/output

Xác định:
- **File nguồn**: Đường dẫn file cần chuyển đổi
- **Định dạng đích**: Format mong muốn
- **Yêu cầu đặc biệt**: Styling, template, encoding, v.v.

### Bước 2: Kiểm tra công cụ

Kiểm tra Pandoc đã được cài đặt chưa:
```bash
pandoc --version
```

Nếu chưa có, hướng dẫn cài đặt:
```bash
# macOS
brew install pandoc

# Ubuntu/Debian
sudo apt-get install pandoc

# Windows (Chocolatey)
choco install pandoc
```

Cho xuất PDF, cần thêm LaTeX engine:
```bash
# macOS
brew install --cask mactex
# hoặc bản nhẹ:
brew install basictex

# Ubuntu/Debian
sudo apt-get install texlive-xetex texlive-fonts-recommended
```

### Bước 3: Thực hiện chuyển đổi

## Lệnh Chuyển Đổi Chi Tiết

### 📄 Document Conversions (dùng Pandoc)

#### Markdown → DOCX
```bash
pandoc input.md -f gfm -t docx -o output.docx --standalone
```

#### Markdown → PDF
```bash
pandoc input.md -f gfm -o output.pdf --pdf-engine=xelatex -V mainfont="Arial" -V geometry:margin=2.5cm
```

#### Markdown → HTML
```bash
pandoc input.md -f gfm -t html5 -o output.html --standalone --metadata title="Document"
```

#### Markdown → EPUB
```bash
pandoc input.md -f gfm -t epub -o output.epub --metadata title="Book Title"
```

#### Markdown → LaTeX
```bash
pandoc input.md -f gfm -t latex -o output.tex --standalone
```

#### Markdown → PowerPoint (PPTX)
```bash
pandoc input.md -f gfm -t pptx -o output.pptx
```

#### DOCX → Markdown
```bash
pandoc input.docx -f docx -t gfm -o output.md --wrap=none --extract-media=./media
```

#### DOCX → PDF
```bash
pandoc input.docx -f docx -o output.pdf --pdf-engine=xelatex
```

#### DOCX → HTML
```bash
pandoc input.docx -f docx -t html5 -o output.html --standalone --extract-media=./media
```

#### HTML → Markdown
```bash
pandoc input.html -f html -t gfm -o output.md --wrap=none
```

#### HTML → DOCX
```bash
pandoc input.html -f html -t docx -o output.docx --standalone
```

#### HTML → PDF
```bash
pandoc input.html -f html -o output.pdf --pdf-engine=xelatex
```

#### EPUB → Markdown
```bash
pandoc input.epub -f epub -t gfm -o output.md --wrap=none
```

#### EPUB → DOCX
```bash
pandoc input.epub -f epub -t docx -o output.docx
```

#### LaTeX → PDF
```bash
pandoc input.tex -f latex -o output.pdf --pdf-engine=xelatex
```

#### LaTeX → DOCX
```bash
pandoc input.tex -f latex -t docx -o output.docx
```

#### RST → Markdown
```bash
pandoc input.rst -f rst -t gfm -o output.md
```

#### ODT → Markdown
```bash
pandoc input.odt -f odt -t gfm -o output.md
```

#### ODT → DOCX
```bash
pandoc input.odt -f odt -t docx -o output.docx
```

#### TXT → DOCX
```bash
pandoc input.txt -f plain -t docx -o output.docx --standalone
```

#### TXT → PDF
```bash
pandoc input.txt -f plain -o output.pdf --pdf-engine=xelatex
```

### 📊 PDF Input (Xử lý đặc biệt)

PDF là format chỉ đọc, cần tool riêng để trích xuất nội dung:

#### PDF → TXT (dùng pdftotext)
```bash
pdftotext input.pdf output.txt
# Hoặc giữ layout:
pdftotext -layout input.pdf output.txt
```

#### PDF → Markdown (dùng Python)
```python
# Cài đặt: pip install pymupdf
import fitz  # PyMuPDF

doc = fitz.open("input.pdf")
md_content = ""
for page in doc:
    md_content += page.get_text("text") + "\n\n---\n\n"

with open("output.md", "w", encoding="utf-8") as f:
    f.write(md_content)
```

#### PDF → DOCX (dùng Python)
```python
# Cài đặt: pip install pdf2docx
from pdf2docx import Converter

cv = Converter("input.pdf")
cv.convert("output.docx")
cv.close()
```

#### PDF → HTML (dùng Python)
```python
# Cài đặt: pip install pymupdf
import fitz

doc = fitz.open("input.pdf")
html = "<html><body>"
for page in doc:
    html += page.get_text("html")
html += "</body></html>"

with open("output.html", "w", encoding="utf-8") as f:
    f.write(html)
```

### 📊 Data Conversions (dùng Python)

#### CSV → JSON
```python
import csv, json

with open("input.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    data = list(reader)

with open("output.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)
```

#### JSON → CSV
```python
import csv, json

with open("input.json", "r", encoding="utf-8") as f:
    data = json.load(f)

if isinstance(data, list) and len(data) > 0:
    with open("output.csv", "w", encoding="utf-8", newline="") as f:
        writer = csv.DictWriter(f, fieldnames=data[0].keys())
        writer.writeheader()
        writer.writerows(data)
```

#### JSON → YAML
```python
import json, yaml

with open("input.json", "r", encoding="utf-8") as f:
    data = json.load(f)

with open("output.yaml", "w", encoding="utf-8") as f:
    yaml.dump(data, f, allow_unicode=True, default_flow_style=False)
```

#### YAML → JSON
```python
import json, yaml

with open("input.yaml", "r", encoding="utf-8") as f:
    data = yaml.safe_load(f)

with open("output.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)
```

#### CSV → XLSX
```python
# Cài đặt: pip install openpyxl
import csv
from openpyxl import Workbook

wb = Workbook()
ws = wb.active
with open("input.csv", "r", encoding="utf-8") as f:
    for row in csv.reader(f):
        ws.append(row)
wb.save("output.xlsx")
```

#### XLSX → CSV
```python
# Cài đặt: pip install openpyxl
import csv
from openpyxl import load_workbook

wb = load_workbook("input.xlsx")
ws = wb.active
with open("output.csv", "w", encoding="utf-8", newline="") as f:
    writer = csv.writer(f)
    for row in ws.iter_rows(values_only=True):
        writer.writerow(row)
```

#### XML → JSON
```python
import json
import xml.etree.ElementTree as ET

def xml_to_dict(element):
    result = {}
    for child in element:
        if len(child) > 0:
            result[child.tag] = xml_to_dict(child)
        else:
            result[child.tag] = child.text
    return result

tree = ET.parse("input.xml")
data = xml_to_dict(tree.getroot())

with open("output.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)
```

### 🖼️ Image Conversions (dùng Python)

#### Chuyển đổi giữa PNG, JPG, WEBP, BMP, TIFF
```python
# Cài đặt: pip install Pillow
from PIL import Image

img = Image.open("input.png")
# Nếu chuyển sang JPG, cần convert RGB (bỏ alpha channel)
if img.mode in ("RGBA", "LA", "P"):
    img = img.convert("RGB")
img.save("output.jpg", quality=95)
# Hoặc: img.save("output.webp"), img.save("output.bmp"), img.save("output.tiff")
```

#### SVG → PNG (dùng cairosvg)
```python
# Cài đặt: pip install cairosvg
import cairosvg
cairosvg.svg2png(url="input.svg", write_to="output.png", scale=2)
```

## Tùy Chọn Nâng Cao

### Custom Styling cho DOCX
```bash
# Dùng reference doc để áp dụng style tùy chỉnh
pandoc input.md -f gfm -t docx -o output.docx --reference-doc=template.docx
```

### Table of Contents (Mục lục)
```bash
# Thêm mục lục tự động
pandoc input.md -f gfm -o output.pdf --toc --toc-depth=3 --pdf-engine=xelatex
pandoc input.md -f gfm -t docx -o output.docx --toc --toc-depth=3
```

### Đánh số heading
```bash
pandoc input.md -f gfm -o output.pdf --number-sections --pdf-engine=xelatex
```

### Font tiếng Việt cho PDF
```bash
pandoc input.md -f gfm -o output.pdf --pdf-engine=xelatex \
  -V mainfont="Noto Sans" \
  -V sansfont="Noto Sans" \
  -V monofont="Noto Sans Mono" \
  -V geometry:margin=2cm
```

### Batch Convert (Chuyển đổi hàng loạt)
```bash
# Chuyển tất cả .md sang .docx trong thư mục
for f in *.md; do
  pandoc "$f" -f gfm -t docx -o "${f%.md}.docx" --standalone
done

# Chuyển tất cả .docx sang .md
for f in *.docx; do
  pandoc "$f" -f docx -t gfm -o "${f%.docx}.md" --wrap=none
done
```

## Quy Tắc Quan Trọng

1. **Luôn kiểm tra file nguồn tồn tại** trước khi chuyển đổi
2. **Giữ lại file gốc** — không ghi đè file nguồn
3. **Encoding UTF-8** — luôn dùng UTF-8 cho tiếng Việt
4. **Thông báo kết quả** — cho người dùng biết file đã được tạo ở đâu
5. **Xử lý lỗi** — nếu công cụ chưa cài, hướng dẫn cài đặt
6. **PDF input** — PDF cần xử lý đặc biệt, không dùng Pandoc trực tiếp
7. **Ưu tiên Pandoc** cho document conversions vì nó đáng tin cậy nhất
8. **Ưu tiên Python** cho data conversions (CSV, JSON, YAML, XML, XLSX)
9. **Hỏi rõ** nếu không chắc định dạng đích mong muốn

## Ví Dụ Sử Dụng

**Người dùng:** "Chuyển file báo cáo.md sang docx"
**Agent:** Chạy `pandoc báo_cáo.md -f gfm -t docx -o báo_cáo.docx --standalone`

**Người dùng:** "Convert tất cả file CSV trong thư mục sang JSON"
**Agent:** Dùng Python script để batch convert CSV → JSON

**Người dùng:** "Xuất tài liệu này ra PDF với mục lục"
**Agent:** Chạy `pandoc input.md -f gfm -o output.pdf --toc --pdf-engine=xelatex -V mainfont="Noto Sans"`
