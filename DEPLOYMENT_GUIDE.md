# Deployment Guide - Reference Status Checker

## Overview

This guide covers deployment options for the Reference Status Checker application, from local development to production environments.

## Prerequisites

### Required Software
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Text editor (VS Code, Sublime Text, etc.)
- Git (for version control)

### Optional Software
- Local web server (for development)
- Web hosting service (for production)
- Google Sheets account (for data management)

## Local Development Setup

### Option 1: Direct File Access
```bash
# 1. Download or clone the project
git clone [repository-url]
cd reference-status-checker

# 2. Open in browser
# Double-click index.html or drag to browser
```

**Pros**: Simple, no server required
**Cons**: Limited by CORS policies, some features may not work

### Option 2: Local Web Server

#### Using Python
```bash
# Python 3
python -m http.server 8000

# Python 2
python -m SimpleHTTPServer 8000
```

#### Using Node.js
```bash
# Install http-server globally
npm install -g http-server

# Start server
http-server -p 8000
```

#### Using PHP
```bash
php -S localhost:8000
```

**Access**: `http://localhost:8000`

### Option 3: Live Server (VS Code Extension)
1. Install "Live Server" extension in VS Code
2. Right-click `index.html`
3. Select "Open with Live Server"

## Google Sheets Configuration

### Step 1: Create Google Sheet
1. Go to [Google Sheets](https://sheets.google.com)
2. Create new spreadsheet
3. Set up columns according to data structure:

| Column | Purpose | Example Data |
|--------|---------|--------------|
| A | (Unused) | - |
| B | Reference Number | AHS/SPA/01/01-14/IAM |
| C | (Unused) | - |
| D | (Unused) | - |
| E | Vendor | ABC Properties Sdn Bhd |
| F | (Unused) | - |
| G | Purchaser | John Doe |
| H-O | (Unused) | - |
| O | Current Stage | S5, Cancel, Close, KIV |

### Step 2: Publish Sheet
1. **File** → **Share** → **Publish to web**
2. Select **Entire Document**
3. Choose **CSV** format
4. Click **Publish**
5. Copy the generated URL

### Step 3: Update Application
```javascript
// In index.html, update the URL constant
const GOOGLE_SHEET_URL = 'YOUR_PUBLISHED_SHEET_URL';
```

## Web Hosting Deployment

### Option 1: Static Hosting Services

#### GitHub Pages
```bash
# 1. Create GitHub repository
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/repo-name.git
git push -u origin main

# 2. Enable GitHub Pages
# Go to repository Settings → Pages
# Select source branch (main)
# Save
```

**URL**: `https://username.github.io/repo-name`

#### Netlify
1. **Drag & Drop**: Drag `index.html` to [Netlify](https://netlify.com)
2. **Git Integration**: Connect GitHub repository
3. **Custom Domain**: Configure if needed

#### Vercel
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel
```

### Option 2: Traditional Web Hosting

#### cPanel Hosting
1. **File Manager**: Upload `index.html` to public_html
2. **Domain**: Point domain to hosting
3. **SSL**: Enable HTTPS certificate

#### AWS S3 + CloudFront
```bash
# 1. Create S3 bucket
aws s3 mb s3://your-bucket-name

# 2. Upload files
aws s3 cp index.html s3://your-bucket-name/

# 3. Configure static website hosting
aws s3 website s3://your-bucket-name --index-document index.html

# 4. Set up CloudFront distribution
```

### Option 3: CDN Services

#### Cloudflare Pages
1. Connect GitHub repository
2. Configure build settings (not needed for static files)
3. Deploy automatically

## Production Configuration

### HTTPS Setup
```html
<!-- Ensure all external resources use HTTPS -->
<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

### Performance Optimization

#### Minification (Optional)
```bash
# Install minification tools
npm install -g html-minifier

# Minify HTML
html-minifier --collapse-whitespace --remove-comments index.html -o index.min.html
```

#### Compression
```nginx
# Nginx configuration
gzip on;
gzip_types text/html text/css application/javascript;
```

### Security Headers
```nginx
# Nginx security headers
add_header X-Frame-Options "SAMEORIGIN";
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1; mode=block";
add_header Referrer-Policy "strict-origin-when-cross-origin";
```

## Environment-Specific Configurations

### Development Environment
```javascript
// Development configuration
const CONFIG = {
    debug: true,
    cacheBusting: true,
    errorLogging: true
};
```

### Production Environment
```javascript
// Production configuration
const CONFIG = {
    debug: false,
    cacheBusting: true,
    errorLogging: false
};
```

## Monitoring and Analytics

### Google Analytics
```html
<!-- Add to <head> section -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

### Error Monitoring
```javascript
// Add error tracking
window.addEventListener('error', function(e) {
    console.error('Application Error:', e.error);
    // Send to error tracking service
});
```

## Backup and Recovery

### Data Backup Strategy
1. **Google Sheets**: Enable version history
2. **Application Code**: Use Git version control
3. **Configuration**: Document all settings

### Recovery Procedures
1. **Code Recovery**: Clone from Git repository
2. **Data Recovery**: Restore from Google Sheets version history
3. **Configuration Recovery**: Update constants in code

## Troubleshooting

### Common Deployment Issues

#### CORS Errors
**Problem**: Cannot load Google Sheets data
**Solution**: Ensure Google Sheet is publicly accessible

#### 404 Errors
**Problem**: Page not found
**Solution**: Check file paths and server configuration

#### SSL Certificate Issues
**Problem**: Mixed content warnings
**Solution**: Ensure all resources use HTTPS

### Performance Issues

#### Slow Loading
- Check Google Sheets response time
- Optimize CSV data size
- Enable compression

#### Search Performance
- Consider pagination for large datasets
- Implement search indexing
- Cache frequently accessed data

## Maintenance

### Regular Tasks
1. **Data Updates**: Monitor Google Sheets for new data
2. **Security Updates**: Keep dependencies updated
3. **Performance Monitoring**: Check loading times
4. **User Feedback**: Monitor for issues

### Update Procedures
1. **Code Updates**: Deploy new version
2. **Data Structure Changes**: Update column indices
3. **Feature Additions**: Test thoroughly before deployment

## Scaling Considerations

### For Large Datasets
- Implement server-side search
- Add pagination
- Use database instead of Google Sheets

### For High Traffic
- Use CDN for static assets
- Implement caching
- Consider load balancing

## Security Checklist

- [ ] HTTPS enabled
- [ ] Security headers configured
- [ ] Input validation implemented
- [ ] Error messages don't expose sensitive data
- [ ] Regular security updates
- [ ] Access logs monitored

## Performance Checklist

- [ ] Page load time < 3 seconds
- [ ] Search response time < 1 second
- [ ] Mobile responsive design
- [ ] Compression enabled
- [ ] Caching configured
- [ ] Images optimized

## Support and Documentation

### User Documentation
- Create user guide
- Add FAQ section
- Provide contact information

### Technical Documentation
- API documentation
- Configuration guide
- Troubleshooting guide

### Maintenance Documentation
- Deployment procedures
- Backup procedures
- Update procedures 