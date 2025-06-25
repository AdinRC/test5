# API Documentation - Reference Status Checker

## Overview

This document provides detailed technical documentation for the Reference Status Checker application's JavaScript API, data structures, and configuration options.

## Configuration Constants

### Data Source Configuration

```javascript
const GOOGLE_SHEET_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vQhVOvcKbqZZuAuYE-FgLykZX5PRpeTMgJ1SpAAKAcZRRTb8Bt8cPOSQXj0EWL6iC7eQRofXmggke-T/pub?gid=0&single=true&output=csv';
```

**Purpose**: URL for the Google Sheets CSV export
**Format**: Google Sheets published CSV URL
**Requirements**: Sheet must be publicly accessible

### Column Index Configuration

```javascript
const SEARCH_COLUMN_INDEX = 1;    // Column B - Reference numbers
const VENDOR_COLUMN_INDEX = 4;    // Column E - Vendor information  
const PURCHASER_COLUMN_INDEX = 6; // Column G - Purchaser information
const STAGES_COLUMN_INDEX = 14;   // Column O - Current stage
```

**Purpose**: Define which columns contain specific data
**Indexing**: 0-based array indexing
**Flexibility**: Can be modified for different sheet structures

## Data Structures

### Stage Information Object

```javascript
{
    id: number,           // Stage number (1-17)
    title: string,        // Stage title
    description: string   // Detailed stage description
}
```

**Example**:
```javascript
{
    id: 5,
    title: 'Loan Agreement Execution',
    description: 'The firm finalizes execution of the Loan Agreement and submits it as per the bank\'s legal documentation process.'
}
```

### Search Result Object

```javascript
{
    ref: string,      // Reference number
    stage: string,    // Current stage (e.g., "S5", "Cancel", "Close", "KIV")
    vendor: string,   // Vendor name
    purchaser: string // Purchaser name
}
```

**Example**:
```javascript
{
    ref: "AHS/SPA/01/01-14/IAM",
    stage: "S5",
    vendor: "ABC Properties Sdn Bhd",
    purchaser: "John Doe"
}
```

## Core Functions

### `loadSheetData()`

**Purpose**: Fetches and parses CSV data from Google Sheets

**Returns**: `Promise<void>`

**Behavior**:
- Disables search interface during loading
- Shows loading animation
- Fetches CSV data with cache-busting timestamp
- Parses CSV using `parseCsv()` function
- Updates UI state on completion
- Handles errors gracefully

**Error Handling**:
- Network errors display user-friendly message
- Console logging for debugging
- Graceful fallback to error state

**Example Usage**:
```javascript
// Called automatically on page load
document.addEventListener('DOMContentLoaded', loadSheetData);
```

### `parseCsv(csvText: string)`

**Purpose**: Robust CSV parser with quote handling

**Parameters**:
- `csvText`: Raw CSV string from Google Sheets

**Returns**: `Array<Array<string>>` - 2D array of parsed data

**Features**:
- Handles quoted fields with embedded commas
- Supports escaped quotes (`""`)
- Normalizes line endings
- Trims whitespace from cells

**Example**:
```javascript
const csvData = 'col1,col2\n"quoted,field",normal';
const parsed = parseCsv(csvData);
// Result: [['col1', 'col2'], ['quoted,field', 'normal']]
```

### `handleSearch()`

**Purpose**: Main search logic and result processing

**Returns**: `void`

**Process Flow**:
1. Validates data availability
2. Normalizes search term (trim, lowercase)
3. Performs exact match search
4. Determines result type (timeline vs special status)
5. Renders appropriate display
6. Updates file information sidebar

**Search Logic**:
- Case-insensitive matching
- Exact string comparison
- Linear search through all rows

**Example Usage**:
```javascript
searchBtn.addEventListener('click', handleSearch);
refInput.addEventListener('keyup', (event) => {
    if (event.key === 'Enter') handleSearch();
});
```

### `renderTimeline(currentStage: string)`

**Purpose**: Generates interactive timeline display

**Parameters**:
- `currentStage`: Stage identifier (e.g., "S5")

**Returns**: `void`

**Features**:
- Visual timeline with 17 stages
- Color-coded status indicators
- Auto-scroll to current stage
- Responsive design

**Stage Indicators**:
- **Completed**: Green circle with checkmark
- **Current**: Blue circle with white dot and ring
- **Pending**: Gray circle

**Timeline Structure**:
```html
<div class="relative flex items-start">
    <div class="absolute left-[9px] top-6 h-full w-0.5 bg-green-500"></div>
    <div class="z-10">
        <!-- Stage indicator -->
    </div>
    <div class="ml-4 pb-8">
        <!-- Stage content -->
    </div>
</div>
```

### `renderSpecialStatus(status: string)`

**Purpose**: Handles non-timeline statuses (Cancel/Close/KIV)

**Parameters**:
- `status`: Status string from data

**Returns**: `void`

**Status Types**:
- **Cancel**: Red theme with X icon
- **Close**: Green theme with checkmark icon  
- **KIV**: Yellow theme with pause icon

**Display Structure**:
```html
<div class="p-8 text-center flex flex-col items-center justify-center h-full bg-red-50 rounded-lg">
    <div class="text-red-700">
        <!-- Status icon -->
    </div>
    <h3 class="mt-4 text-2xl font-bold text-red-700">File Cancelled</h3>
    <p class="mt-2 text-md text-gray-600">Description</p>
</div>
```

### `displayFileInfo(result: SearchResult)`

**Purpose**: Shows file details in left sidebar

**Parameters**:
- `result`: Search result object

**Returns**: `void`

**Updates Elements**:
- `#vendor-info`: Vendor name
- `#purchaser-info`: Purchaser name  
- `#current-stage-info`: Current stage
- `#file-info-container`: Shows/hides container

## Event Handlers

### DOM Ready Handler
```javascript
document.addEventListener('DOMContentLoaded', loadSheetData);
```
**Purpose**: Initialize application on page load

### Search Button Handler
```javascript
searchBtn.addEventListener('click', handleSearch);
```
**Purpose**: Trigger search on button click

### Enter Key Handler
```javascript
refInput.addEventListener('keyup', (event) => {
    if (event.key === 'Enter') handleSearch();
});
```
**Purpose**: Trigger search on Enter key press

## Error Handling

### Network Errors
- Displays user-friendly error message
- Logs detailed error to console
- Maintains application stability

### Data Parsing Errors
- Graceful handling of malformed CSV
- Fallback to error state
- User notification of issues

### Search Errors
- Validation of input data
- Clear error messages for no results
- Proper state management

## Performance Considerations

### Data Loading
- Single fetch on page load
- Cache-busting with timestamp
- Minimal network requests

### Search Performance
- Linear search through data
- Optimized for typical dataset sizes
- No unnecessary re-renders

### UI Performance
- Efficient DOM updates
- Minimal reflows/repaints
- Smooth animations

## Browser Compatibility

### Required Features
- ES6+ JavaScript support
- Fetch API
- CSS Grid and Flexbox
- CSS Custom Properties

### Supported Browsers
- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+

## Security Considerations

### Data Access
- Public Google Sheets only
- No authentication required
- Client-side processing

### Input Validation
- Basic input sanitization
- No XSS vulnerabilities
- Safe DOM manipulation

## Debugging

### Console Logging
```javascript
console.error("Error loading sheet data:", error);
```

### Error States
- Visual error indicators
- User-friendly messages
- Graceful degradation

### Development Tools
- Browser developer tools
- Network tab for API calls
- Console for error messages 