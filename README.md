# BrightPath Initiative - NGO/Charity Website

A comprehensive, professional NGO website featuring donation campaigns, volunteer management, and community engagement tools.

## 🌟 Features

### Frontend Components
- **Single Page Application (SPA)** with smooth navigation between sections
- **Responsive Design** - Works perfectly on desktop, tablet, and mobile devices
- **Donation Progress Bars** - Visual representation of fundraising goals for each cause
- **Testimonials Slider** - Auto-rotating carousel showcasing community stories
- **Interactive Statistics** - Animated counters for impact metrics
- **Modern UI/UX** - Professional gradient design with consistent branding

### Pages Included
1. **Home** - Hero section with impact statistics and testimonials
2. **About** - Organization story and core values
3. **Causes** - Six active causes with progress tracking
4. **Impact** - Comprehensive impact metrics and fund allocation breakdown
5. **Donate** - Flexible donation options with multiple causes
6. **Volunteer** - Volunteer registration and opportunity matching
7. **Contact** - Contact form and organization information

### Backend Features
- **Donation Processing** - Secure form submission and storage
- **Volunteer Management** - Application tracking with status updates
- **Contact Management** - Message collection and response tracking
- **Email Notifications** - Automated confirmations and admin alerts
- **Database Storage** - All submissions securely stored with detailed tracking
- **Admin Support** - Tables for managing all submissions

## 📋 Project Structure

```
ngo-website/
├── index.html                 # Main single-page application
├── js/
│   └── form-handler.js       # AJAX form submission handler
├── backend/
│   ├── config.php           # Database configuration & helpers
│   ├── submit_donation.php  # Donation form processor
│   ├── submit_volunteer.php # Volunteer registration processor
│   └── submit_contact.php   # Contact form processor
└── database/
    ├── database.sql         # Database schema
    └── setup.php           # Database initialization script
```

## 🚀 Installation & Setup

### Prerequisites
- PHP 7.4 or higher
- MySQL 5.7 or higher
- Web server (Apache, Nginx, etc.)
- Local development environment (XAMPP, WAMP, MAMP, or similar)

### Step 1: Set Up Local Environment

If using XAMPP on Windows:
1. Place the `ngo-website` folder in `C:\xampp\htdocs\`
2. Start Apache and MySQL from XAMPP Control Panel

### Step 2: Configure Database

1. Open your browser and go to `http://localhost/phpmyadmin`
2. Navigate to `http://localhost/ngo-website/database/setup.php`
3. The setup script will:
   - Create the `brightpath_ngo` database
   - Create all necessary tables
   - Set up initial admin user
   - Initialize donation statistics

### Step 3: Update Configuration (Optional)

If your database credentials differ from defaults:

Edit `backend/config.php` (lines 6-8):
```php
define('DB_HOST', 'localhost');    // Your database host
define('DB_USER', 'root');         // Your database username
define('DB_PASS', '');             // Your database password
define('DB_NAME', 'brightpath_ngo');
```

Also update email configuration (lines 10-11):
```php
define('ADMIN_EMAIL', 'your-email@example.com');
define('VOLUNTEER_EMAIL', 'volunteer-email@example.com');
```

### Step 4: Access the Website

- Visit: `http://localhost/ngo-website/`
- Website should be fully functional

## 💾 Database Schema

### Donations Table
Stores all donation records:
- Donor name, email, and donation amount
- Cause direction (which initiative to support)
- Monthly recurring donation tracking
- Anonymous donation flag
- Payment status and transaction ID

### Volunteers Table
Manages volunteer applications:
- Personal information (name, email, phone, location)
- Area of interest and availability
- Experience and motivation
- Application status (pending, approved, rejected)
- Review tracking with notes

### Contact Messages Table
Stores inquiry submissions:
- Sender information
- Subject and message content
- Read status for admin dashboard
- Response tracking

### Admin Users Table
Manages system administrators:
- Username and email
- Secure password storage
- Role-based access control
- Last login timestamp

## 🎨 Frontend Features Explained

### Donation Progress Bars
Each cause shows:
- Raised amount vs. goal
- Percentage progress
- Animated fill on page load
- Direct donation links

### Testimonials Slider
- Auto-rotates every 5 seconds
- Manual navigation with dots
- Smooth fade transitions
- Features donor and volunteer testimonials

### Animated Statistics
- Counter animation from 0 to target
- Runs when viewing relevant sections
- Formatted with thousand separators

### Responsive Elements
- Mobile-optimized navigation
- Touch-friendly buttons
- Flexible grid layouts
- Optimized form inputs

## 📧 Email Functionality

The system sends automated emails for:

### Donations
- **To Donor**: Donation confirmation with receipt details
- **To Admin**: New donation notification with transaction details

### Volunteers
- **To Applicant**: Application confirmation and timeline
- **To Coordinator**: New application with full details

### Contact
- **To Sender**: Message acknowledgment with reference number
- **To Admin**: Message notification for response

Setup email in `backend/config.php` if using a mail server.

## 🔒 Security Features

- Input sanitization on all form fields
- Email validation with regex
- Prepared SQL statements to prevent injection
- CSRF protection ready
- Secure password hashing (bcrypt-ready)
- CORS headers support

## 📊 Usage Statistics

Track NGO metrics:
- **Lives Impacted**: 50,000+
- **Projects Completed**: 125
- **Active Volunteers**: 2,500
- **Funds Raised**: $5M+
- **Communities Served**: 87

All stats are editable in the HTML (search for "data-target").

## 🎯 Form Customization

### Add New Causes
Edit the causes grid in index.html around line 400. Add:
```html
<div class="cause-card">
    <div class="cause-image">📌</div>
    <div class="cause-content">
        <h3>Your Cause Name</h3>
        <p>Description...</p>
        <div class="progress-container">
            <div class="progress-info">
                <span>$XX,XXX raised</span>
                <span>$YYY,YYY goal</span>
            </div>
            <div class="progress-bar">
                <div class="progress-fill" data-progress="XX"></div>
            </div>
        </div>
    </div>
</div>
```

Also add option to donate form:
```html
<option value="your-cause-slug">Your Cause Name</option>
```

### Modify Volunteer Areas
Edit volunteer form select options around line 450:
```html
<option value="your-area">Your Area Name</option>
```

## 🛠️ Troubleshooting

### Forms Not Submitting
1. Check browser console (F12) for JavaScript errors
2. Verify PHP files are in correct location
3. Ensure database is connected in config.php
4. Check web server error logs

### Database Connection Errors
1. Verify MySQL is running
2. Check credentials in config.php
3. Ensure brightpath_ngo database exists
4. Run setup.php again

### Emails Not Sending
1. Configure SMTP or use mail() function
2. Update ADMIN_EMAIL and VOLUNTEER_EMAIL
3. Check server email configuration
4. Verify firewall allows email ports

### Progress Bars Not Animating
1. Open browser console for errors
2. Check CSS animations are enabled
3. Verify data-progress attributes exist
4. Clear browser cache and refresh

## 📱 Mobile Optimization

The website is fully responsive with:
- Mobile-first design approach
- Touch-optimized buttons (minimum 44x44px)
- Flexible grid layouts
- Readable font sizes on all devices
- Optimized images and loading

## 🔄 Deployment Checklist

Before going live:
- [ ] Change all email addresses in config.php
- [ ] Update database password
- [ ] Set `display_errors` to 0 in config.php
- [ ] Configure SMTP for emails
- [ ] Add SSL certificate
- [ ] Set up admin panel
- [ ] Test all forms
- [ ] Review donation processing

## 📝 Admin Panel (Coming Soon)

Features planned:
- Dashboard with metrics
- Donation management
- Volunteer application review
- Contact message responses
- Statistics updates
- Email templates

## 🤝 Contributing

To enhance this website:
1. Create backup of current system
2. Make changes in test environment
3. Test thoroughly before deployment
4. Document any modifications

## 📞 Support

For issues or questions:
- Email: admin@brightpathinitiative.org
- Phone: +1 (555) 123-4567
- Website: http://localhost/ngo-website

## 📄 License

This project is provided as-is for non-profit organizations.

---

**Last Updated**: March 2026  
**Version**: 1.0  
**Status**: Production Ready
