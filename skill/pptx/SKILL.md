---
name: pptx
description: Use when the user asks for a slide deck or presentation (.pptx), or asks to read or edit an uploaded .pptx file.
user-invocable: true
---

# PowerPoint decks

Use the code execution tool with Python and `python-pptx`. Save every output file under `/mnt/data/` so the user can download it.

```python
from pptx import Presentation
from pptx.util import Inches, Pt

prs = Presentation()
prs.slide_width, prs.slide_height = Inches(13.333), Inches(7.5)

slide = prs.slides.add_slide(prs.slide_layouts[0])
slide.shapes.title.text = "Deck title"
slide.placeholders[1].text = "Subtitle"

slide = prs.slides.add_slide(prs.slide_layouts[1])
slide.shapes.title.text = "Key points"
body = slide.placeholders[1].text_frame
body.text = "First point"
body.add_paragraph().text = "Second point"

prs.save("/mnt/data/deck.pptx")
```

Rules:
- One idea per slide. At most 6 bullets per slide.
- Put charts in as PNG images made with `matplotlib` (`slide.shapes.add_picture`).
- Use the speaker notes (`slide.notes_slide.notes_text_frame`) for detail.

To read an uploaded deck, loop over `prs.slides` and each `shape.has_text_frame`.

After saving, tell the user the file name and list the slide titles.
