---
name: docx
description: Use when the user asks for a Word document (.docx), such as a report, letter, memo, resume or contract, or asks to read or edit an uploaded .docx file.
user-invocable: true
---

# Word documents

Use the code execution tool with Python. Save every output file under `/mnt/data/` so the user can download it.

## Create

Use `python-docx`. For fill-in templates, use `docxtpl`.

```python
from docx import Document
from docx.shared import Pt

doc = Document()
doc.add_heading("Title", level=0)
doc.add_paragraph("Body text.")
table = doc.add_table(rows=1, cols=2)
table.style = "Light Grid Accent 1"
table.rows[0].cells[0].text = "Item"
table.rows[0].cells[1].text = "Value"
doc.save("/mnt/data/report.docx")
```

Rules:
- Use real heading styles (`add_heading`), not bold paragraphs, so the document has a navigable outline.
- Use `List Bullet` and `List Number` paragraph styles for lists.
- Keep one font family. Set it once on the `Normal` style.

## Read or edit

- Plain text: `docx2python` or `python-docx` (`doc.paragraphs`, `doc.tables`).
- HTML or Markdown view: `mammoth`.
- To edit, open the uploaded file with `Document(path)`, change it, and save a new file under `/mnt/data/`. Keep the original styles.

After saving, tell the user the file name and give a short summary of the content.
