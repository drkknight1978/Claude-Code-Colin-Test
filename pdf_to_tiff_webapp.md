# PDF to TIFF Converter - JavaScript Web Application

A fully client-side web application for converting and cleaning scanned engineering drawings. **All processing happens in your browser** - no server required!

## 🌟 Features

- **100% Client-Side Processing** - Your files never leave your computer
- **Multi-File Support** - Load and process multiple PDFs and TIFFs
- **Live Preview** - See original and processed output side-by-side
- **PDF Rendering** - Converts PDF pages to images at your chosen DPI (150-600)
- **Despeckle Filter** - Median filter with configurable kernel sizes (3×3, 5×5, 7×7)
- **Binarization** - Otsu's automatic thresholding for 1-bit output
- **Batch Processing** - Process all loaded files at once
- **Keyboard Navigation** - Use arrow keys (← →) to cycle through files
- **Drag & Drop** - Simple file loading interface

## 📋 How It Works

### Technologies Used

1. **PDF.js** - Mozilla's PDF rendering engine for converting PDF pages to canvas
2. **UTIF.js** - TIFF file decoder for reading TIFF images
3. **Canvas API** - HTML5 canvas for image manipulation and processing
4. **Vanilla JavaScript** - No framework dependencies, pure JS

### Processing Pipeline

```
PDF/TIFF Input
    ↓
Load & Render (at target DPI)
    ↓
[Optional] Despeckle (Median Filter)
    ↓
[Optional] Binarize (Otsu Threshold)
    ↓
Output Image
    ↓
Download as File
```

### Key Algorithms

**Median Filter (Despeckle)**
- Collects pixel values in a kernel window (3×3, 5×5, or 7×7)
- Sorts values and selects the median
- Effective at removing salt-and-pepper noise while preserving edges

**Otsu's Thresholding**
- Calculates optimal threshold to separate foreground/background
- Maximizes between-class variance in the histogram
- Produces clean 1-bit (black & white) images ideal for CAD underlays

## 🚀 Usage

### Running the Application

1. **Open in Browser** - Simply double-click `pdf_to_tiff_webapp.html`
2. **Or use a local server** (optional):
   ```bash
   # Python 3
   python -m http.server 8000
   
   # Then open http://localhost:8000/pdf_to_tiff_webapp.html
   ```

### Workflow

1. **Load Files**
   - Click the upload area or drag & drop PDF/TIFF files
   - Multiple files can be selected at once

2. **Navigate**
   - Click files in the list to preview them
   - Use "Previous" / "Next" buttons or arrow keys (← →)

3. **Configure Options**
   - ☑️ **PDF → TIFF Convert**: Enable binarization for PDFs
   - ☑️ **Despeckle**: Apply median filter noise reduction
   - **Kernel Size**: Choose filter strength (3=fast, 7=strong)
   - **Target DPI**: Set resolution (300 DPI recommended)

4. **Preview**
   - Click "Update Preview" to see the processed result
   - Options auto-update the preview when changed

5. **Process**
   - **Process Current File**: Convert the currently displayed file
   - **Process All Files**: Batch convert all loaded files
   - Files download automatically when complete

## 📐 Technical Details

### DPI Handling

PDF documents are rendered at your specified DPI by scaling from PDF points (72 pt/inch):
- **300 DPI**: `scale = 300/72 = 4.167x`
- For an 8.5" × 11" page: `2550 × 3300` pixels at 300 DPI

This preserves physical dimensions accurately for CAD underlays.

### Image Processing

**Median Filter Implementation**:
```javascript
// Collects pixels in kernel window
for (let ky = -halfKernel; ky <= halfKernel; ky++) {
    for (let kx = -halfKernel; kx <= halfKernel; kx++) {
        // Get pixel, convert to grayscale, add to window
    }
}
// Sort and select median value
window.sort((a, b) => a - b);
const median = window[Math.floor(window.length / 2)];
```

**Otsu's Method**:
```javascript
// Calculate histogram
// For each possible threshold:
//   - Calculate between-class variance
//   - Keep threshold with maximum variance
```

### File Output

Currently outputs as PNG format. For true TIFF Group4 compression, you would need:
- Server-side processing (Python with PIL, as in original app)
- Or advanced TIFF encoding libraries in JavaScript

The client-side approach prioritizes:
- ✅ Privacy (no file uploads)
- ✅ Speed (no network latency)  
- ✅ Simplicity (no server setup)

## 🔄 Differences from Desktop Version

| Feature | Desktop (Tkinter) | Web (JavaScript) |
|---------|-------------------|------------------|
| Processing | Local Python/PIL | Browser JavaScript |
| PDF Rendering | PyMuPDF | PDF.js |
| TIFF Output | Group4 compression | PNG output* |
| File System | Direct folder access | Download individual files |
| Dependencies | Python packages | CDN libraries |
| Multi-page PDFs | Page 1 only | Page 1 only |

*Note: True TIFF Group4 encoding would require additional server-side processing or complex JS libraries

## 💡 Advantages of Web Version

- **No Installation** - Works in any modern browser
- **Cross-Platform** - Windows, Mac, Linux, even mobile
- **Privacy** - Files never uploaded to a server
- **Updates** - Always latest version, no reinstall needed
- **Accessibility** - Share a single HTML file with colleagues

## 🎯 Use Cases

Perfect for:
- **CAD Professionals** - Cleaning scanned drawings for AutoCAD/MicroStation
- **Engineering Teams** - Converting legacy PDF drawings to raster underlays
- **Archives** - Batch processing scanned technical documents
- **Anyone** - Converting PDFs to high-quality images

## ⚡ Performance Tips

- **Smaller kernels** (3×3) process faster than larger ones
- **Lower DPI** (150-200) for quick previews, 300+ for production
- **Batch processing** may take time for many large files
- **Modern browsers** (Chrome, Firefox, Edge) work best

## 🔧 Customization

The application is a single HTML file with embedded CSS and JavaScript. You can easily customize:

- **Styling**: Edit the `<style>` section
- **DPI Options**: Modify the `targetDPI` select options
- **Processing**: Adjust algorithms in the `<script>` section
- **UI Layout**: Change the grid template in `.main-content`

## 🐛 Known Limitations

1. **Output Format**: Saves as PNG instead of TIFF Group4 (client-side limitation)
2. **Multi-page PDFs**: Only processes first page
3. **Large Files**: Browser memory constraints may limit very large PDFs
4. **Batch Download**: Files download one-by-one (no ZIP creation)

## 🔮 Future Enhancements

Potential additions:
- [ ] True TIFF Group4 encoding (requires complex library or server)
- [ ] Multi-page PDF support
- [ ] ZIP archive for batch downloads
- [ ] More filter options (Gaussian, bilateral)
- [ ] Manual threshold adjustment
- [ ] Color preservation options
- [ ] Canvas drawing tools for markup

## 📝 Browser Compatibility

Tested and working on:
- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Edge 90+
- ✅ Safari 14+

Requires support for:
- Canvas API
- File API
- Blob/URL APIs
- ES6+ JavaScript

## 📄 License

MIT License - Feel free to use, modify, and distribute!

## 🙏 Credits

Built using:
- [PDF.js](https://mozilla.github.io/pdf.js/) by Mozilla
- [UTIF.js](https://github.com/photopea/UTIF.js) by Photopea
- Original desktop app design and algorithms

---

**Note**: For production CAD workflows requiring true TIFF Group4 compression, consider using the original Python desktop application or setting up a server-based solution.
