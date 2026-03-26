# BrightPath Initiative - API Reference & Developer Documentation

## Overview

This document provides technical details for developers extending or customizing the BrightPath Initiative website.

## Project Structure

```
ngo-website/
├── index.html              # Main SPA application
├── .htaccess              # Apache configuration
├── README.md              # User documentation
├── QUICKSTART.md          # Setup guide
├── API.md                 # This file
│
├── backend/
│   ├── config.php         # Configuration and database class
│   ├── submit_donation.php    # Donation processor
│   ├── submit_volunteer.php   # Volunteer processor
│   ├── submit_contact.php     # Contact processor
│   ├── test-connection.php    # Connection tester
│   └── export.php         # Data export utility
│
├── database/
│   ├── database.sql       # Schema definition
│   └── setup.php          # Setup wizard
│
├── js/
│   └── form-handler.js    # AJAX form submission
│
└── css/
    (Currently embedded in index.html)
```

## API Endpoints

### Donation Submission

**Endpoint:** `POST /backend/submit_donation.php`

**Request Parameters:**
```json
{
  "name": "Donor Name",
  "email": "donor@example.com",
  "amount": 100,
  "cause": "education",
  "monthly": "0 or 1",
  "anonymous": "0 or 1"
}
```

**Cause Values:**
- `general` - Where needed most (default)
- `education` - Education for All
- `water` - Clean Water Access
- `healthcare` - Healthcare Outreach
- `agriculture` - Sustainable Agriculture
- `women` - Women's Empowerment
- `emergency` - Emergency Relief

**Response (Success):**
```json
{
  "success": true,
  "message": "Thank you for your donation!...",
  "data": {
    "donation_id": 1,
    "transaction_id": "BPI-20260326123456-1234",
    "amount": 100
  }
}
```

**Response (Error):**
```json
{
  "success": false,
  "message": "Validation errors: Please enter a valid email address"
}
```

**Email Notifications:**
- Donor confirmation email with receipt
- Admin notification with transaction details

---

### Volunteer Registration

**Endpoint:** `POST /backend/submit_volunteer.php`

**Request Parameters:**
```json
{
  "name": "Volunteer Name",
  "email": "volunteer@example.com",
  "phone": "+1234567890",
  "location": "City, State",
  "interest": "education",
  "availability": "weekends",
  "experience": "Optional experience details",
  "motivation": "Why I want to volunteer"
}
```

**Interest Areas:**
- `education` - Education Programs
- `healthcare` - Healthcare Outreach
- `water` - Clean Water Projects
- `agriculture` - Sustainable Agriculture
- `women` - Women's Empowerment
- `emergency` - Emergency Relief
- `fundraising` - Fundraising & Events
- `admin` - Administrative Support

**Availability Options:**
- `weekends` - Weekends Only
- `weekdays` - Weekdays
- `flexible` - Flexible
- `summer` - Summer Break
- `occasional` - Occasional Events

**Response (Success):**
```json
{
  "success": true,
  "message": "Thank you for your application!...",
  "data": {
    "volunteer_id": 1,
    "email": "volunteer@example.com"
  }
}
```

**Email Notifications:**
- Confirmation email to applicant
- Admin notification for review

---

### Contact Form Submission

**Endpoint:** `POST /backend/submit_contact.php`

**Request Parameters:**
```json
{
  "name": "Sender Name",
  "email": "sender@example.com",
  "subject": "Inquiry Subject",
  "message": "Message content here..."
}
```

**Response (Success):**
```json
{
  "success": true,
  "message": "Thank you for your message!...",
  "data": {
    "message_id": 1
  }
}
```

**Email Notifications:**
- Acknowledgment email to sender with reference number
- Notification email to admin for response

---

## Database Schema

### donations Table

```sql
CREATE TABLE donations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    cause VARCHAR(100) DEFAULT 'general',
    is_monthly BOOLEAN DEFAULT FALSE,
    is_anonymous BOOLEAN DEFAULT FALSE,
    payment_status VARCHAR(50) DEFAULT 'pending',
    transaction_id VARCHAR(255) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

**Key Columns:**
- `id` - Unique donation identifier
- `transaction_id` - Payment processing reference
- `amount` - Donation amount in USD
- `is_monthly` - Monthly recurring flag
- `is_anonymous` - Privacy flag
- `payment_status` - completed, pending, failed

---

### volunteers Table

```sql
CREATE TABLE volunteers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    phone VARCHAR(20) NOT NULL,
    location VARCHAR(255) NOT NULL,
    area_of_interest VARCHAR(100) NOT NULL,
    availability VARCHAR(50) NOT NULL,
    experience TEXT,
    motivation TEXT NOT NULL,
    status VARCHAR(50) DEFAULT 'pending',
    application_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    reviewed_at TIMESTAMP NULL,
    reviewed_by INT NULL,
    notes TEXT,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

**Status Values:**
- `pending` - Awaiting review
- `approved` - Approved volunteer
- `rejected` - Application rejected
- `active` - Currently volunteering

---

### contact_messages Table

```sql
CREATE TABLE contact_messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    subject VARCHAR(255) NOT NULL,
    message LONGTEXT NOT NULL,
    is_read BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    responded_at TIMESTAMP NULL,
    response_message LONGTEXT,
    INDEX idx_is_read (is_read)
);
```

---

## Configuration

### backend/config.php

```php
// Database credentials
define('DB_HOST', 'localhost');
define('DB_USER', 'root');
define('DB_PASS', '');
define('DB_NAME', 'brightpath_ngo');

// Email settings
define('ADMIN_EMAIL', 'admin@brightpathinitiative.org');
define('VOLUNTEER_EMAIL', 'volunteer@brightpathinitiative.org');

// Site configuration
define('SITE_NAME', 'BrightPath Initiative');
define('SITE_URL', 'http://localhost/ngo-website');

// Error reporting (disable in production)
ini_set('display_errors', 1);
error_reporting(E_ALL);
```

---

## Helper Functions

### Database Functions

```php
// Get database connection
$db = new Database();
$conn = $db->connect();

// Close connection
$db->closeConnection();
```

### Input Validation

```php
// Sanitize input
$safe_input = sanitize($_POST['field']);

// Validate email
if (validateEmail($email)) {
    // Valid email
}

// Send JSON response
sendResponse(true, "Success message", ['data' => 'value']);
sendResponse(false, "Error message");
```

### Email Functions

```php
// Send email
sendEmail(
    'recipient@example.com',
    'Subject Line',
    '<html>HTML content here</html>'
);
```

---

## AJAX Form Handling

### Frontend (js/form-handler.js)

```javascript
// Submit donation form
async function submitDonationEnhanced(e) {
    e.preventDefault();
    const formData = new FormData(e.target);
    
    const response = await fetch('backend/submit_donation.php', {
        method: 'POST',
        body: formData
    });
    
    const result = await response.json();
    if (result.success) {
        // Handle success
    } else {
        // Handle error
    }
}
```

---

## Frontend Integration

### Integrating New Form

1. **Create HTML form** in `index.html`:
```html
<form onsubmit="submitNewForm(event)">
    <input type="text" name="field" required>
    <button type="submit">Submit</button>
</form>
```

2. **Create PHP processor** in `backend/submit_new.php`:
```php
<?php
require_once 'config.php';

if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
    sendResponse(false, 'Invalid method');
}

// Validate and process
$field = sanitize($_POST['field'] ?? '');

// Store in database
$sql = "INSERT INTO new_table (field) VALUES (?)";
$stmt = $conn->prepare($sql);
$stmt->bind_param("s", $field);
$stmt->execute();

sendResponse(true, 'Success!');
?>
```

3. **Add JavaScript handler** in `js/form-handler.js`:
```javascript
async function submitNewForm(e) {
    e.preventDefault();
    const formData = new FormData(e.target);
    
    const response = await fetch('backend/submit_new.php', {
        method: 'POST',
        body: formData
    });
    
    const result = await response.json();
    // Handle response
}
```

---

## Data Export

### Export Endpoint

**URL:** `/backend/export.php`

**Parameters:**
- `type` - donations | volunteers | contact
- `format` - csv | json
- `export` - 1 (triggers download)

**Example:**
```
GET /backend/export.php?type=donations&format=csv&export=1
```

---

## Security Considerations

### Input Validation
- All inputs sanitized with `sanitize()`
- Email validated with `validateEmail()`
- Length checks on text fields
- Type checking on numeric values

### SQL Injection Prevention
- Prepared statements with parameterized queries
- No direct string concatenation in SQL

### Email Security
- HTML-escaped email content
- Sender verification headers
- MIME type specification

### CORS Headers
```php
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: GET, POST');
header('Access-Control-Allow-Headers: Content-Type');
```

---

## Error Handling

### Common HTTP Status Codes
- `200` - Request successful
- `400` - Bad request (validation error)
- `404` - Not found
- `500` - Server error

### Error Response Format
```json
{
  "success": false,
  "message": "Human-readable error message"
}
```

---

## Testing

### Test Connection
Access: `/backend/test-connection.php`
- Verifies database connectivity
- Checks all tables exist
- Shows current statistics

### Manual Testing
1. Open browser developer tools (F12)
2. Go to Console tab
3. Submit forms and check responses
4. Verify database entries

### Automated Testing (Future)
Consider adding:
- PHPUnit tests for backend
- Jest tests for frontend
- Integration tests with test database

---

## Performance Optimization

### Database Indexes
All major query columns are indexed:
- `donations.email`
- `volunteers.email`
- `contact_messages.email`
- Status and date columns

### Caching
- Leverage browser caching (.htaccess)
- GZIP compression enabled
- Static assets cached for 1 month

### Query Optimization
- Use parameterized queries
- Limit result sets with LIMIT
- Index frequently queried columns

---

## Monitoring & Maintenance

### Log Files
PHP errors logged to:
- Server error_log
- PHP error_log
- Application-specific logs (future)

### Database Backup
Regular backups of `brightpath_ngo` database:
```bash
mysqldump -u root brightpath_ngo > backup.sql
```

### Monitoring Tasks
- Check error logs weekly
- Verify email delivery
- Review form submissions
- Monitor database size

---

## Version History

### v1.0 (Initial Release)
- Complete SPA website
- Donation processing
- Volunteer management
- Contact system
- Database backend
- Email notifications

---

## Support & Resources

### Documentation
- README.md - User guide
- QUICKSTART.md - Setup guide
- API.md - This file

### Tools
- /backend/test-connection.php - Connection tester
- /backend/export.php - Data exporter
- /database/setup.php - Database initializer

### Contact
- Support: admin@brightpathinitiative.org
- Development: development@brightpathinitiative.org

---

**API Version:** 1.0  
**Last Updated:** March 2026  
**Status:** Stable
