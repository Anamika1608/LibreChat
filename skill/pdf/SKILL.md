---
name: pdf
description: Use when the user asks to create a PDF, or to read, extract, merge, split or fill an uploaded PDF file.
user-invocable: true
---

# PDF files

Use the code execution tool with Python. Save every output file under `/mnt/data/` so the user can download it.

## Create

Use `reportlab` Platypus for documents with flowing text and tables.

```python
from reportlab.lib.pagesizes import A4
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.lib import colors
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle

styles = getSampleStyleSheet()
doc = SimpleDocTemplate("/mnt/data/report.pdf", pagesize=A4)
table = Table([["Item", "Value"], ["A", "1"]])
table.setStyle(TableStyle([("GRID", (0, 0), (-1, -1), 0.5, colors.grey)]))
doc.build([
    Paragraph("Title", styles["Title"]),
    Paragraph("Body text.", styles["BodyText"]),
    Spacer(1, 12),
    table,
])
```

For charts, draw them with `matplotlib`, save a PNG, and add it with `reportlab.platypus.Image`.

## Read and change

- Extract text: `pdfminer.six` (`from pdfminer.high_level import extract_text`).
- Merge, split, rotate, read form fields: `PyPDF2` (`PdfReader`, `PdfWriter`).
- Page images: `pdf2image`.

After saving, tell the user the file name and give a short summary of the content.
