---
name: xlsx
description: Use when the user asks for a spreadsheet (.xlsx), or asks to read, analyze or edit an uploaded Excel or CSV file.
user-invocable: true
---

# Excel spreadsheets

Use the code execution tool with Python. Save every output file under `/mnt/data/` so the user can download it.

- Analysis: read with `pandas.read_excel` or `pandas.read_csv`.
- New workbook with formats or charts: `xlsxwriter`.
- Edit an existing workbook and keep its formatting: `openpyxl`.

```python
import pandas as pd

df = pd.DataFrame({"Item": ["A", "B"], "Value": [1, 2]})
with pd.ExcelWriter("/mnt/data/data.xlsx", engine="xlsxwriter") as writer:
    df.to_excel(writer, sheet_name="Data", index=False)
    sheet = writer.sheets["Data"]
    sheet.set_column(0, 1, 18)
    sheet.write_formula(len(df) + 1, 1, f"=SUM(B2:B{len(df) + 1})")
```

Rules:
- Write totals and derived values as Excel formulas, not fixed numbers, so the sheet stays live.
- Freeze the header row and set column widths.

After saving, tell the user the file name and what each sheet holds.
