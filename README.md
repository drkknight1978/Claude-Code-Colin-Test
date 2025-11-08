# PDF Stamper Web App

A client-side web application for adding stamps (images) to PDF documents. No server required - all processing happens in your browser!

## Features

- **PDF Preview**: View and navigate through PDF pages with zoom controls
- **Stamp Positioning**:
  - Pre-defined positions (top-left, top-right, center, bottom-left, bottom-right)
  - Custom positioning with drag-and-drop or coordinate input
- **Stamp Customization**:
  - Adjustable opacity (0-100%)
  - Adjustable scale (10-200%)
  - Real-time preview of changes
- **Flexible Application**:
  - Apply to current page only
  - Apply to all pages
  - Apply to specific page ranges (e.g., "1-3, 5, 7-9")
- **Privacy-Focused**: All processing happens locally in your browser - files never leave your device

## Usage

1. **Open the Application**
   - Simply open `pdf-stamper.html` in a modern web browser
   - No installation or setup required

2. **Load Files**
   - Click "Browse..." next to "PDF File" to select your PDF
   - Click "Browse..." next to "Stamp Image" to select your stamp image (PNG, JPG, etc.)

3. **Position Your Stamp**
   - Use the "Position Preset" dropdown for quick positioning
   - Or drag the stamp directly on the preview
   - Or enter custom X/Y coordinates

4. **Customize Appearance**
   - Adjust opacity slider for transparency
   - Adjust scale slider to resize the stamp
   - Use zoom controls to better view your work

5. **Apply & Download**
   - Choose which pages to stamp (current, all, or specific range)
   - Click "Process & Download"
   - Your stamped PDF will download automatically

## Technical Details

### Technologies Used

- **PDF.js** (v3.11.174) - For rendering PDF previews
- **pdf-lib** (v1.17.1) - For modifying PDF documents
- **Vanilla JavaScript** - No frameworks needed
- **HTML5 Canvas** - For real-time preview rendering

### Browser Compatibility

Works in all modern browsers that support:
- ES6+ JavaScript
- HTML5 Canvas
- FileReader API
- Blob API

Recommended browsers:
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+

### File Structure

```
pdf-stamper.html    # Complete standalone application
README.md          # This file
LICENSE            # License information
.gitignore         # Git ignore rules
```

## Privacy & Security

- **100% Client-Side**: All processing happens in your browser
- **No Data Upload**: Your files are never sent to any server
- **No Tracking**: No analytics or tracking code
- **Offline Capable**: Works without internet (except for initial CDN library loading)

## Limitations

- Large PDF files (>50MB) may be slow to process
- Maximum image resolution depends on browser memory
- Some PDF features (forms, signatures) may be affected by modification

## Development

This is a standalone HTML file with no build process. To modify:

1. Open `pdf-stamper.html` in your text editor
2. Make changes to HTML/CSS/JavaScript
3. Save and refresh in browser
4. No compilation or build step needed

## Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test in multiple browsers
5. Submit a pull request

## License

MIT License - see LICENSE file for details

## Troubleshooting

**PDF won't load**
- Ensure the PDF is not password-protected or corrupted
- Try a different PDF file
- Check browser console for errors

**Stamp appears in wrong position**
- Coordinates are based on PDF dimensions, not screen pixels
- Use the preview to verify positioning
- Remember: Y-axis starts from top in preview, bottom in PDF coordinates

**Downloaded PDF is corrupted**
- Ensure both files loaded successfully (green checkmarks)
- Try with a smaller/simpler PDF
- Clear browser cache and try again

**Performance issues**
- Use smaller stamp images (resize before uploading)
- Process fewer pages at once
- Close other browser tabs to free memory

## Credits

Built with:
- [PDF.js](https://mozilla.github.io/pdf.js/) by Mozilla
- [pdf-lib](https://pdf-lib.js.org/) by Andrew Dillon

## Version History

- **v1.0** - Initial release
  - Basic stamping functionality
  - Position presets and custom positioning
  - Opacity and scale controls
  - Page range support
