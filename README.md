# PPTX to PDF, Maximum Image Fidelity

There are now two exporters in this folder.

Use `Export-PptxUltraRasterPdf.ps1` if PowerPoint's native PDF export visibly
compresses your images. It renders each slide as a high-resolution PNG and then
builds a PDF from those rendered slide images without lossy JPEG recompression.

Use `Export-PptxLosslessPdf.ps1` only if you want to try PowerPoint's native
high-fidelity fixed-format export.

## Best Visual Quality

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\Export-PptxUltraRasterPdf.ps1 -PptxPath "C:\path\to\deck.pptx"
```

By default, the PDF is written next to the deck as:

```text
deck.ultra-raster-600dpi.pdf
```

For even more zoom/detail, try 1200 DPI:

```powershell
.\Export-PptxUltraRasterPdf.ps1 -PptxPath "C:\path\to\deck.pptx" -Dpi 1200
```

This can create a very large PDF.

By default, `Export-PptxUltraRasterPdf.ps1` embeds raw RGB slide images. That
means no lossy image recompression, but the PDF can be huge. To use lossless
Flate compression instead:

```powershell
.\Export-PptxUltraRasterPdf.ps1 -PptxPath "C:\path\to\deck.pptx" -Compression Flate
```

## Native PowerPoint Export

To try PowerPoint's native PDF export:

```powershell
.\Export-PptxLosslessPdf.ps1 `
  -PptxPath "C:\path\to\deck.pptx" `
  -PdfPath "C:\path\to\deck.lossless.pdf"
```

To open the PDF after export:

```powershell
.\Export-PptxLosslessPdf.ps1 -PptxPath "C:\path\to\deck.pptx" -OpenAfterExport
```

## Strict mode

If you want the tool to fail when the PPTX contains JPEG media, use:

```powershell
.\Export-PptxLosslessPdf.ps1 -PptxPath "C:\path\to\deck.pptx" -Strict
```

JPEGs are already lossy inside the PPTX. The script can preserve the deck's
source quality, but it cannot restore pixels that PowerPoint or another tool
already compressed before export.

## What the native tool does

- Uses Microsoft PowerPoint's `ExportAsFixedFormat` API.
- Uses print-intent PDF export.
- Exports slides directly, not handouts.
- Avoids PDF/A, which can force extra conversions.
- Avoids bitmap-substituting missing fonts.
- Scans `ppt/media` and reports embedded image formats before export.

## Important limits

No PDF exporter can guarantee mathematically lossless output for every possible
PowerPoint object. Transparency, effects, masks, 3D transforms, video stills,
or unsupported fonts can cause PowerPoint to render parts of a slide.

`Export-PptxUltraRasterPdf.ps1` is a visual-fidelity tool. It converts each
slide to pixels first, so text and vector shapes will no longer remain editable
or infinitely scalable in the PDF. Increase `-Dpi` if you need cleaner zoom.

For the cleanest source deck before exporting:

- In PowerPoint, set `File > Options > Advanced > Image Size and Quality`.
- Check `Do not compress images in file`.
- Set default resolution to `High fidelity`.
- Reinsert any images that were previously compressed by PowerPoint.

===== made with Codex, model: gpt-5.5 =====
