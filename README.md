# Reference Status Checker

A modern web application for tracking and displaying the status of legal property transactions. Built with HTML, CSS (Tailwind), and vanilla JavaScript, this application provides real-time status checking against Google Sheets data.

## 🚀 Features

### Core Functionality
- **Real-time Status Tracking**: Check the current stage of any property transaction using reference numbers
- **Interactive Timeline**: Visual timeline showing all 17 stages of the property transaction process
- **File Information Display**: Shows vendor, purchaser, and current stage details
- **Special Status Handling**: Dedicated displays for cancelled, closed, and KIV (Keep In View) files
- **Responsive Design**: Works seamlessly on desktop, tablet, and mobile devices

### User Interface
- **Modern Design**: Clean, professional interface using Tailwind CSS
- **Loading States**: Smooth loading animations and user feedback
- **Error Handling**: Graceful error messages for failed searches or data loading issues
- **Auto-scroll**: Automatically scrolls to current stage in timeline
- **Search Validation**: Input validation and helpful error messages

## 📋 Transaction Stages

The application tracks 17 distinct stages of property transactions:

1. **Initial File Opening & Setup** - Case file opening and document preparation
2. **Consent & MOT Prep** - Consent applications and transfer preparation
3. **Agreement Execution** - SPA and Loan Agreement signing and stamping
4. **Developer & SPA Solicitor Correspondence** - Obtaining property documents
5. **Loan Agreement Execution** - Finalizing loan documentation
6. **FA Stamping** - Facility Agreement stamp duty payment
7. **Consent & Low-Cost Application** - Developer consent and housing approvals
8. **Caveat Registration** - Bank interest protection registration
9. **Redeem Preparation** - Document compilation for redemption
10. **Redeem Execution** - Bank disbursement for redemption
11. **Discharge of Charge Process** - Seller's bank discharge handling
12. **Stamp Duty & Adjudication** - Transfer document stamping
13. **Blanket Consent & PA** - Developer consent and Power of Attorney
14. **Land Office Presentation** - Title transfer document submission
15. **Bank Payment Processing (BPP)** - Final disbursement preparation
16. **Bank Disbursement BPP** - Balance purchase price release
17. **Finalization & Vacant Possession** - Key handover and document safekeeping

## 🛠️ Technical Architecture

### Frontend Technologies
- **HTML5**: Semantic markup structure
- **Tailwind CSS**: Utility-first CSS framework for styling
- **Vanilla JavaScript**: No framework dependencies
- **Google Fonts**: Inter font family for typography

### Data Source
- **Google Sheets**: CSV export from Google Sheets as data source
- **Real-time Updates**: Data refreshes on each page load
- **CSV Parsing**: Robust CSV parser handling quoted fields and special characters

### Key Components

#### Data Configuration
```javascript
const GOOGLE_SHEET_URL = 'https://docs.google.com/spreadsheets/d/e/...';
const SEARCH_COLUMN_INDEX = 1; // Column B - Reference numbers
const VENDOR_COLUMN_INDEX = 4; // Column E - Vendor information
const PURCHASER_COLUMN_INDEX = 6; // Column G - Purchaser information
const STAGES_COLUMN_INDEX = 14; // Column O - Current stage
```

#### Core Functions
- `loadSheetData()`: Fetches and parses CSV data from Google Sheets
- `parseCsv()`: Robust CSV parser with quote handling
- `handleSearch()`: Main search logic and result processing
- `renderTimeline()`: Generates interactive timeline display
- `renderSpecialStatus()`: Handles non-timeline statuses (Cancel/Close/KIV)
- `displayFileInfo()`: Shows file details in sidebar

## 📱 Usage

### For End Users

1. **Open the Application**: Navigate to `index.html` in a web browser
2. **Wait for Data Load**: The application automatically loads the latest data
3. **Enter Reference Number**: Input the full reference number (e.g., `AHS/SPA/01/01-14/IAM`)
4. **Search**: Click the "Search" button or press Enter
5. **View Results**: 
   - Timeline view shows current stage and progress
   - File details appear in the left sidebar
   - Special statuses show dedicated message cards

### For Administrators

#### Updating Data
1. **Google Sheets**: Update the connected Google Sheet with new data
2. **CSV Export**: Ensure the sheet is published as CSV
3. **Automatic Refresh**: Data updates automatically when users refresh the page

#### Customizing Stages
Edit the `STAGES_INFO` array in the JavaScript code to modify stage descriptions or add new stages.

## 🔧 Setup and Deployment

### Local Development
1. **Download Files**: Save `index.html` to your local machine
2. **Open Browser**: Double-click the file or open in a web browser
3. **No Server Required**: Works as a static file

### Web Deployment
1. **Upload Files**: Upload `index.html` to any web server
2. **HTTPS Recommended**: For production use, ensure HTTPS is enabled
3. **CORS Considerations**: Google Sheets must be published publicly

### Google Sheets Configuration
1. **Create Sheet**: Set up a Google Sheet with the required columns
2. **Publish**: File → Share → Publish to web → CSV format
3. **Update URL**: Replace the `GOOGLE_SHEET_URL` in the code

## 📊 Data Structure

The application expects the following column structure in the Google Sheet:

| Column | Index | Purpose | Example |
|--------|-------|---------|---------|
| A | 0 | (Unused) | - |
| B | 1 | Reference Number | AHS/SPA/01/01-14/IAM |
| C | 2 | (Unused) | - |
| D | 3 | (Unused) | - |
| E | 4 | Vendor | Vendor Name |
| F | 5 | (Unused) | - |
| G | 6 | Purchaser | Purchaser Name |
| H-O | 7-13 | (Unused) | - |
| O | 14 | Current Stage | S5, Cancel, Close, KIV |

## 🎨 Customization

### Styling
- **Colors**: Modify Tailwind classes for brand colors
- **Fonts**: Change Google Fonts import for different typography
- **Layout**: Adjust grid columns and spacing in CSS classes

### Functionality
- **Search Logic**: Modify `handleSearch()` for different matching rules
- **Stage Definitions**: Update `STAGES_INFO` array for different processes
- **Data Columns**: Adjust column indices for different sheet structures

## 🔒 Security Considerations

- **Public Data**: Google Sheet must be publicly accessible
- **No Authentication**: Application doesn't require user login
- **Client-side Processing**: All data processing happens in the browser
- **CORS**: Ensure Google Sheets allows cross-origin requests

## 🐛 Troubleshooting

### Common Issues

**Data Not Loading**
- Check Google Sheets URL is correct and publicly accessible
- Verify CSV export is enabled
- Check browser console for CORS errors

**Search Not Working**
- Ensure reference numbers match exactly (case-sensitive)
- Check column indices match your sheet structure
- Verify data format in Google Sheets

**Styling Issues**
- Ensure Tailwind CSS CDN is accessible
- Check for CSS conflicts in browser developer tools

### Browser Compatibility
- **Modern Browsers**: Chrome, Firefox, Safari, Edge (latest versions)
- **JavaScript**: ES6+ features required
- **CSS**: Tailwind CSS 3.x required

## 📈 Performance

- **Lightweight**: Single HTML file with CDN dependencies
- **Fast Loading**: Optimized for quick initial page load
- **Efficient Search**: Linear search through CSV data
- **Minimal Network**: Only loads data when needed

## 🤝 Contributing

To contribute to this project:

1. **Fork the Repository**: Create your own copy
2. **Make Changes**: Implement your improvements
3. **Test Thoroughly**: Ensure all functionality works
4. **Submit Pull Request**: Share your changes

## 📄 License

This project is proprietary software. All rights reserved.

## 📞 Support

**Version**: 1.0.0  
**Last Updated**: December 2024  
**Maintained By**: RBX Team
