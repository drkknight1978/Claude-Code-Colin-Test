# Railway Engineering Utilities

A collection of client-side web applications for engineering and document management tasks. All processing happens in your browser - no server required!

## 🔐 Authentication Required

**This application suite now requires password authentication to access all tools.**

- **Login Page**: `login.html`
- **Default Password**: `railway123`
- **Session Duration**: 24 hours

**⚠️ IMPORTANT**: Change the default password before deployment! See [AUTH_README.md](AUTH_README.md) for detailed instructions.

## 📋 Available Tools

### 1. PDF Stamper
Add stamps and watermarks to PDF documents with precision control.

**Features**:
- PDF preview with zoom controls
- Drag-and-drop stamp positioning
- Pre-defined position presets (corners, center)
- Custom positioning with X/Y coordinates
- Adjustable opacity (0-100%)
- Adjustable scale (10-200%)
- Apply to specific pages or page ranges
- Real-time preview of changes

### 2. Voltage Drop Calculator
Calculate voltage drop in electrical systems with iterative precision. Essential for railway power distribution and signal circuits.

**Features**:
- CSV-based conductor data upload (proprietary data protection)
- Iterative voltage drop calculation algorithm
- Support for custom conductor sizes and resistances
- Real-time calculation results with:
  - Voltage at load
  - Current drawn by load
  - Voltage drop (absolute and percentage)
  - Iteration count
- Input validation and error handling
- Railway-themed interface matching the application suite

### 3. Future Tools (Coming Soon)
- Technical Drawings Converter
- Rail Calculator (additional functionality)
- Data Converter

## 🚀 Quick Start

1. **Access the Application**
   - Open `index.html` in a modern web browser
   - You'll be redirected to the login page

2. **Login**
   - Enter password: `railway123` (change this!)
   - You'll be authenticated for 24 hours

3. **Select a Tool**
   - Choose from available utilities on the home page
   - Currently: PDF Stamper and Voltage Drop Calculator are available

4. **Logout**
   - Click the "Logout" button on any page to end your session

## 📁 File Structure

```
├── index.html           # Gateway page with tool selection
├── login.html          # Authentication page
├── auth.js             # Shared authentication module
├── pdf-stamper.html    # PDF stamping tool
├── calc.html           # Voltage drop calculator tool
├── classic_style.css   # Classic styling for calculator
├── README.md           # This file
├── AUTH_README.md      # Authentication setup guide
├── CHANGELOG.md        # Version history and changes
└── .gitignore          # Git ignore rules
```

## 🔒 Security Features

### Authentication System
- **Password Protection**: All tools require login
- **Session Management**: 24-hour sessions with automatic expiry
- **SHA-256 Hashing**: Passwords are hashed before storage/comparison
- **No Cleartext Storage**: Only password hashes stored in code
- **Logout Functionality**: End session from any page

### Privacy
- **100% Client-Side**: All processing happens in your browser
- **No Data Upload**: Your files never leave your device
- **No Tracking**: No analytics or tracking code
- **Local Storage Only**: Session data stored locally

## ⚙️ Configuration

### Changing the Password

**See [AUTH_README.md](AUTH_README.md) for complete instructions.**

Quick method:
```bash
# Generate password hash
echo -n "your_new_password" | sha256sum | cut -d' ' -f1

# Add the hash to login.html at line 280
```

### Session Duration

Edit `login.html` line 225:
```javascript
sessionDuration: 24 * 60 * 60 * 1000 // 24 hours in milliseconds
```

Change to desired duration:
- 1 hour: `1 * 60 * 60 * 1000`
- 12 hours: `12 * 60 * 60 * 1000`
- 7 days: `7 * 24 * 60 * 60 * 1000`

## 🛠️ PDF Stamper Usage

1. **Login** to access the tool
2. **Load Files**:
   - Select your PDF file
   - Select your stamp image (PNG, JPG, etc.)
3. **Position Your Stamp**:
   - Use preset positions or drag to position
   - Enter custom coordinates if needed
4. **Customize**:
   - Adjust opacity for transparency
   - Adjust scale to resize
   - Use zoom to preview better
5. **Apply & Download**:
   - Choose pages (current, all, or range like "1-3, 5, 7-9")
   - Click "Process & Download"
   - Stamped PDF downloads automatically

## ⚡ Voltage Drop Calculator Usage

1. **Login** to access the tool
2. **Upload Conductor Data**:
   - Prepare a CSV file with conductor sizes and resistances
   - Format: `Conductor Size (mm²),Resistance (Ω/km)`
   - Example:
     ```csv
     Conductor Size (mm²),Resistance (Ω/km)
     0.75,49.6
     1.00,36.2
     2.5,15.1
     ```
   - Click "Choose File" and select your CSV
3. **Configure Calculation**:
   - Select conductor size from the dropdown (populated from CSV)
   - Enter cable length in kilometers
   - Enter power load in VA (volt-amperes)
   - Enter supplied voltage in volts
4. **Calculate**:
   - Click "Calculate" button
   - View results including:
     - Voltage at load
     - Current drawn
     - Voltage drop (V and %)
     - Number of iterations required
5. **Adjust & Recalculate**:
   - Modify any input values
   - Click "Calculate" again for updated results

**CSV Format Notes**:
- First row is the header and will be skipped
- Each subsequent row should have: conductor size, resistance
- Conductor size can include units (e.g., "2.5mm²" or "2.5 (f)")
- Resistance must be a valid number in Ω/km

## 💻 Technical Details

### Technologies Used
- **PDF.js** (v3.11.174) - PDF rendering
- **pdf-lib** (v1.17.1) - PDF modification
- **Web Crypto API** - Password hashing (SHA-256)
- **LocalStorage API** - Session management
- **FileReader API** - CSV file parsing
- **Vanilla JavaScript** - No frameworks, no dependencies
- **HTML5 Canvas** - Preview rendering

### Browser Compatibility

**Minimum Requirements**:
- ES6+ JavaScript support
- HTML5 Canvas
- FileReader API
- Blob API
- Web Crypto API (for authentication)

**Recommended Browsers**:
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+

### Authentication Flow

```
User → index.html/pdf-stamper.html
       ↓
   auth.js checks session
       ↓
   No valid session?
       ↓
   Redirect to login.html
       ↓
   User enters password
       ↓
   Hash & validate
       ↓
   Create session (24h)
       ↓
   Redirect back to app
       ↓
   Access granted!
```

## 🔧 Development

### No Build Process
This is a collection of standalone HTML files requiring no build process.

To modify:
1. Open the relevant HTML file in your text editor
2. Make changes to HTML/CSS/JavaScript
3. Save and refresh in browser
4. No compilation needed

### Adding New Tools
1. Create new HTML file (e.g., `new-tool.html`)
2. Add authentication script:
   ```html
   <script src="auth.js"></script>
   ```
3. Add logout button to UI
4. Update `index.html` to link to new tool

### Testing Authentication
1. Clear localStorage: `localStorage.clear()`
2. Navigate to protected page → should redirect to login
3. Login → should redirect back
4. Check session persists on refresh
5. Test logout functionality

## ⚠️ Important Security Notes

1. **Client-Side Only**: This authentication is client-side and provides basic access control
2. **Not for Highly Sensitive Data**: For production apps with sensitive data, use server-side authentication
3. **Can Be Bypassed**: Determined users can bypass by manipulating localStorage
4. **Use Case**: Suitable for internal tools, light protection, deterring casual access
5. **Server-Side Recommended**: For real security requirements, implement backend authentication

## 🐛 Troubleshooting

### Authentication Issues

**Can't login with correct password**
- Check browser console for errors
- Verify password hash matches in `login.html` line 280
- Clear localStorage and try again

**Redirected to login repeatedly**
- Check that `auth.js` is loading properly
- Verify browser supports Web Crypto API
- Check browser console for JavaScript errors

**Session expires too quickly**
- Check `sessionDuration` setting in `login.html`
- Verify system clock is correct

### PDF Stamper Issues

**PDF won't load**
- Ensure PDF is not password-protected
- Check for file corruption
- Try a different PDF

**Stamp in wrong position**
- Remember: coordinates are based on PDF dimensions, not screen pixels
- Use preview to verify positioning
- Y-axis starts from top in preview, bottom in PDF

**Downloaded PDF corrupted**
- Ensure both files loaded successfully (green checkmarks)
- Try smaller/simpler PDF
- Clear browser cache

**Performance issues**
- Resize stamp images before uploading
- Process fewer pages at once
- Close other browser tabs

### Voltage Drop Calculator Issues

**CSV file won't load**
- Ensure file is in CSV format (not Excel .xlsx)
- Check that file has proper header row
- Verify comma separation (not semicolons or tabs)
- Ensure file encoding is UTF-8

**No conductors appear in dropdown**
- Verify CSV has valid data rows (not just header)
- Check that resistance values are valid numbers
- Ensure no blank lines in CSV
- Try re-uploading the CSV file

**Calculate button disabled**
- Upload a valid CSV file first
- Ensure at least one conductor loaded successfully
- Check browser console for errors

**Incorrect calculation results**
- Verify input values are in correct units:
  - Length in kilometers (not meters)
  - Power in VA (not watts for AC loads)
  - Voltage in volts
  - Resistance in Ω/km
- Check conductor selection matches intended size
- Ensure positive values for all inputs

## 📝 Version History

### v2.1 - Voltage Drop Calculator
- ✅ Added Voltage Drop Calculator tool (calc.html)
- ✅ Implemented CSV-based conductor data upload
- ✅ Iterative voltage drop calculation algorithm
- ✅ Railway-themed styling matching application suite
- ✅ Proprietary data protection (no hardcoded conductor values)
- ✅ Input validation and error handling
- ✅ Enhanced results display with voltage drop percentage
- ✅ Authentication integration
- ✅ Back navigation to main utilities page

### v2.0 - Authentication Update
- ✅ Added password-based authentication system
- ✅ Created login page with Railway Engineering theme
- ✅ Implemented session management (24-hour expiry)
- ✅ Protected all app pages (index.html, pdf-stamper.html)
- ✅ Added logout functionality
- ✅ SHA-256 password hashing (no cleartext storage)
- ✅ Shared authentication module (auth.js)
- ✅ Comprehensive documentation (AUTH_README.md)

### v1.0 - Initial Release
- Basic PDF stamping functionality
- Position presets and custom positioning
- Opacity and scale controls
- Page range support
- Railway Engineering themed gateway page

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Make your changes
4. Test in multiple browsers
5. Test authentication flow
6. Update documentation
7. Submit a pull request

## 📄 License

MIT License - see LICENSE file for details

## 🙏 Credits

Built with:
- [PDF.js](https://mozilla.github.io/pdf.js/) by Mozilla
- [pdf-lib](https://pdf-lib.js.org/) by Andrew Dillon
- Web Crypto API for secure hashing

## 📞 Support

For issues or questions:
- Check [AUTH_README.md](AUTH_README.md) for authentication setup
- Review troubleshooting section above
- Check browser console for error messages
- Ensure you're using a modern, supported browser

---

**Remember**: Change the default password (`railway123`) before deploying!
