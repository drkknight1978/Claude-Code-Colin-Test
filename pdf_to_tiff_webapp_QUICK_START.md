# Quick Start Guide - PDF to TIFF Web App

## 🚀 Instant Use (No Installation Required!)

1. Download `pdf_to_tiff_webapp.html` 
2. Double-click the file to open in your browser
3. Start converting files immediately!

That's it! The entire application runs in your browser.

## 🌐 What's Different from the Desktop App?

### Desktop App (Original)
- **Pros**: True TIFF Group4 compression, direct file system access
- **Cons**: Requires Python installation, dependencies, platform-specific

### Web App (New)
- **Pros**: Zero installation, works anywhere, privacy-focused (no uploads), cross-platform
- **Cons**: Outputs PNG instead of TIFF Group4*, downloads files individually

*For true TIFF Group4 compression, you'd need server-side processing

## 📖 Code Explanation

Since you're a computer science hobbyist, here's how the key parts work:

### 1. PDF Rendering (PDF.js)
```javascript
// PDF.js loads the PDF and renders it at your chosen DPI
const pdf = await pdfjsLib.getDocument({data: arrayBuffer}).promise;
const page = await pdf.getPage(1);

// Scale factor: 300 DPI / 72 PDF points per inch = 4.167x
const scale = targetDPI / 72;
const viewport = page.getViewport({scale: scale});

// Render to canvas at the scaled resolution
await page.render({canvasContext: context, viewport: viewport}).promise;
```

### 2. Median Filter (Despeckle)
```javascript
// For each pixel, collect surrounding pixels in a kernel window
for (let ky = -halfKernel; ky <= halfKernel; ky++) {
    for (let kx = -halfKernel; kx <= halfKernel; kx++) {
        // Get pixel value (with bounds checking)
        const px = Math.min(Math.max(x + kx, 0), width - 1);
        const py = Math.min(Math.max(y + ky, 0), height - 1);
        window.push(getPixelGrayscale(px, py));
    }
}

// Sort and pick the median (middle value)
window.sort((a, b) => a - b);
const median = window[Math.floor(window.length / 2)];
```

This effectively removes noise because:
- Salt-and-pepper noise creates outliers
- The median is resistant to outliers
- Edge pixels are preserved better than with averaging

### 3. Otsu's Threshold (Binarization)
```javascript
// Build histogram of grayscale values
const histogram = new Array(256).fill(0);
for (const pixel of grayArray) histogram[pixel]++;

// Try each possible threshold (0-255)
for (let threshold = 0; threshold < 256; threshold++) {
    // Calculate class weights and means
    const weightBackground = pixelsBelow / totalPixels;
    const weightForeground = pixelsAbove / totalPixels;
    const meanBackground = sumBelow / pixelsBelow;
    const meanForeground = sumAbove / pixelsAbove;
    
    // Between-class variance (what we want to maximize)
    const variance = weightBackground * weightForeground * 
                    (meanBackground - meanForeground) ** 2;
    
    if (variance > maxVariance) {
        bestThreshold = threshold;
    }
}
```

Otsu's method finds the threshold that maximizes separation between dark and light pixels, which produces optimal black/white conversion for documents.

### 4. Canvas to ImageData Pipeline
```javascript
// The Canvas API gives us direct pixel access
const imageData = context.getImageData(0, 0, width, height);

// imageData.data is a Uint8ClampedArray with RGBA values:
// [R, G, B, A, R, G, B, A, ...]
//  ↑ pixel 0  ↑ pixel 1

// To modify pixel at (x, y):
const index = (y * width + x) * 4;
imageData.data[index]     = red;    // R
imageData.data[index + 1] = green;  // G
imageData.data[index + 2] = blue;   // B
imageData.data[index + 3] = 255;    // A (opaque)

// Write back to canvas
context.putImageData(imageData, 0, 0);
```

## 🔧 Advanced: Adding True TIFF Support

If you want true TIFF Group4 output, you have options:

### Option 1: Use UTIF.js for TIFF Encoding
```javascript
// UTIF.js can encode TIFFs, but Group4 compression is tricky
const tiffData = UTIF.encode([imageData], {
    // Limited compression options in pure JS
});
```

### Option 2: Add a Simple Python Backend
```python
# Flask endpoint that uses PIL
@app.route('/convert-tiff', methods=['POST'])
def convert_tiff():
    img = Image.frombytes(...)
    img.save('output.tif', compression='group4', dpi=(300, 300))
    return send_file('output.tif')
```

### Option 3: Use WebAssembly
Compile libtiff or similar C library to WASM for client-side TIFF encoding.

## 🎨 Customization Ideas

### Add Color Options
```javascript
// Currently converts to grayscale, but you could preserve color:
if (preserveColor) {
    // Keep RGB channels instead of converting to gray
    return {r: data[idx], g: data[idx+1], b: data[idx+2]};
}
```

### Add Gaussian Blur
```javascript
function gaussianBlur(imageData, sigma) {
    // Generate Gaussian kernel
    // Apply convolution
    // Return blurred image
}
```

### Manual Threshold Slider
```html
<input type="range" id="threshold" min="0" max="255" value="128">
<script>
    // Use slider value instead of Otsu calculation
    const threshold = parseInt(document.getElementById('threshold').value);
</script>
```

## 📚 Libraries Used

1. **PDF.js** (https://mozilla.github.io/pdf.js/)
   - Mozilla's JavaScript PDF renderer
   - Powers Firefox's PDF viewer
   - Handles complex PDF operations

2. **UTIF.js** (https://github.com/photopea/UTIF.js)
   - Fast TIFF decoder in pure JavaScript
   - Used by Photopea online editor
   - Supports most TIFF formats

3. **Canvas API** (Native HTML5)
   - Direct pixel manipulation
   - Hardware accelerated
   - Universal browser support

## 🧪 Testing Your Changes

1. Make edits to the HTML file
2. Refresh your browser (Ctrl+F5 for hard refresh)
3. Test with various PDF/TIFF files
4. Check browser console (F12) for errors

## 🐛 Debugging Tips

```javascript
// Add console logging to track processing
console.log('Processing:', filename);
console.log('Image size:', imageData.width, 'x', imageData.height);
console.log('Otsu threshold:', threshold);

// Visualize intermediate steps
function debugCanvas(imageData, label) {
    const canvas = document.createElement('canvas');
    canvas.width = imageData.width;
    canvas.height = imageData.height;
    canvas.getContext('2d').putImageData(imageData, 0, 0);
    document.body.appendChild(canvas);
    console.log(label, canvas);
}
```

## 📊 Performance Considerations

### Time Complexity
- **Median Filter**: O(width × height × kernel²)
  - 3×3 kernel: ~9 comparisons per pixel
  - 7×7 kernel: ~49 comparisons per pixel
  - A 2550×3300 image = 8.4M pixels × 49 = 411M operations

- **Otsu Threshold**: O(width × height + 256 × 256)
  - Linear scan + 256 threshold evaluations
  - Much faster than median filter

### Memory Usage
- **ImageData**: width × height × 4 bytes (RGBA)
- 2550×3300 @ 300 DPI = ~32 MB per image
- Multiple canvases can consume significant RAM

### Optimization Ideas
```javascript
// Use Web Workers for parallel processing
const worker = new Worker('image-processor-worker.js');
worker.postMessage({imageData, options});
worker.onmessage = (e) => {
    const result = e.data;
    // Update UI
};
```

## 🎓 Learning Resources

- **Canvas Manipulation**: MDN Web Docs - Canvas API
- **Image Processing**: Stanford CS231n course materials
- **PDF Internals**: PDF Reference 1.7 (ISO 32000-1)
- **Algorithms**: "Digital Image Processing" by Gonzalez & Woods

---

Have fun experimenting with the code! 🚀
