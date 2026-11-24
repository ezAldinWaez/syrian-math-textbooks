---
name: ocr-document
description: Transcribe scanned PDFs or selected pages into one Markdown file with original figures, or merge existing OCR batches.
---

# OCR documents to Markdown

1. **Plan.** Count the PDF pages and sample the layout and scan quality. Split the requested range into consecutive, non-overlapping batches, preferably at section breaks. Use smaller batches for dense, figure-heavy, or difficult pages. Resize queued batches as pace or capacity changes. For a merge-only request, inventory the supplied pages and figures and go straight to step 3.
2. **Delegate.** Give each batch one worker, a separate folder, and the brief below. Prefer GPT-6.1 Terra or another model that reads page images. Run at most two workers by default, or one near a usage limit. If interrupted, pause launches until capacity returns, then resume each batch at its first unfinished page.
3. **Merge.** Assemble `<document>.md` in numeric PDF page order. Rejoin paragraphs, lists, and tables across page and batch breaks. If a marker would split a Markdown block, move it to the nearest block boundary, keeping markers in order. Normalize figure names and links into one flat `images/` folder. Verify each requested page appears once and each image link resolves inside the ZIP. Resolve gaps and overlaps where possible and report any that remain. If the expected end is unknown, report only the verified ranges. Deliver the Markdown and figures together in the ZIP.

## Batch brief

> Read **{pdf_path}**, PDF pages **{first}–{last}** inclusive, counting from 1. Work in **{batch_dir}**.
>
> Check each rendered page for LTR, RTL, or mixed text and column layout. Follow the reading order, including headings spanning columns. Preserve the source language, wording, symbols, captions, and footnotes. Use Markdown for headings, lists, and tables. Write math as inline or display LaTeX and compare each equation with the scan. Recheck doubtful passages and mark anything still unreadable with `(?)`. Omit decorative layout, running headers and footers, and printed page numbers.
>
> Start every page, including blanks, with `<!-- PDF page N -->`, replacing `N` with its 1-based PDF index. Extract or tightly crop figures and embed them in place. Use `images/p12_fig01.png` for the first figure on PDF page 12. Pad both numbers to at least two digits and restart figure numbering at 01 on each page. Transcribe readable body text directly.
>
> Save each page to a temporary file. After checking its text and saving its figures, atomically rename it to `page-N.md`. Only the renamed file marks completion. On retry, rebuild the unfinished page and its figures. Assemble completed pages in numeric order as `batch.md` and report its path, completed range, and blockers.
