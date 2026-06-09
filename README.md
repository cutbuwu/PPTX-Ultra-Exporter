# PPTX Ultra Exporter

A local Windows app for exporting PowerPoint decks to high-resolution raster PDFs
without lossy PDF image recompression.

It was made for oversized architecture/portfolio boards where PowerPoint's
native PDF export visibly compresses drawings, renders, or detailed images.

## What It Does

- Upload a `.pptx` through a local web UI.
- Export each slide through PowerPoint as a high-resolution PNG.
- Assemble those slide PNGs into a PDF using raw or lossless image streams.
- Optionally export only one of four horizontal panels before PDF assembly.

The result is a visual-fidelity PDF. Text and vector shapes become high
resolution pixels.

## Requirements

- Windows
- Microsoft PowerPoint installed
- Node.js 18 or newer
- Python 3 with Pillow and pypdf

If you are running this from Codex, the included scripts already point to the
bundled Codex Python. Friends using this from GitHub may need to edit
`tools/Export-PptxUltraRasterPdf.ps1` and `tools/Crop-PdfRightQuarter.ps1`, or
pass `-PythonPath`, if their Python is elsewhere.

Install Python packages:

```powershell
pip install pillow pypdf
```

## Run

```powershell
npm start
```

Then open:

```text
http://localhost:5177
```

On Windows you can also double-click:

```text
start-windows.bat
```

Finished PDFs are copied to your Windows `Downloads` folder. The browser
download button still works too.

## Quality Settings

- `300 DPI`: smaller and fast.
- `600 DPI`: good default for very detailed boards.
- `1200 DPI`: extreme zoom quality, large files.
- `Raw`: no compression in the PDF image streams, huge files.
- `Flate`: lossless compression, usually much smaller.

## Cropping

If your slide is four A1 panels side by side, choose:

- `Panel 1 of 4`
- `Panel 2 of 4`
- `Panel 3 of 4`
- `Panel 4 of 4`

The crop happens on the exported slide PNGs before PDF assembly. That keeps the
selected panel sharp while avoiding a huge hidden full-width image inside the
PDF.

## Notes

This app cannot recover quality already lost inside the PPTX. If PowerPoint
already compressed an image before export, reinsert the original image first.

For best source quality in PowerPoint:

- Open `File > Options > Advanced > Image Size and Quality`.
- Enable `Do not compress images in file`.
- Set default resolution to `High fidelity`.

## License

MIT


