# BrightPath Initiative - Deployment & Best Practices Guide

## 🚀 Production Deployment Checklist

### Pre-Deployment (Week Before)

- [ ] **Backup** - Create full backup of development system
- [ ] **Testing** - Complete end-to-end testing of all forms
- [ ] **Security Review** - Check all credentials and secrets
- [ ] **Performance** - Test with realistic data volumes
- [ ] **Browser Testing** - Verify on Chrome, Firefox, Safari, Edge

### Database Setup

- [ ] MySQL/MariaDB installed and running
- [ ] Database user created with limited privileges
- [ ] Database name set (brightpath_ngo recommended)
- [ ] Backup strategy in place (daily automated)
- [ ] Connection credentials updated in config.php

### Web Server Configuration

- [ ] PHP 7.4+ installed with required extensions:
  - `mysqli` for database
  - `json` for responses
  - `mail` or SMTP for emails
  - `openssl` for security
  - `gzip` for compression
  
- [ ] Apache/Nginx configured with:
  - HTTPS/SSL certificate
  - Proper permissions (755 directories, 644 files)
  - .htaccess enabled (Apache)
  - Gzip compression enabled

### Security Hardening

- [ ] Update `DB_PASS` in backend/config.php
- [ ] Change admin initial password
- [ ] Set `display_errors` to 0 (line 10 in config.php)
- [ ] Enable error_reporting to log file only
- [ ] Set proper file permissions (644 PHP, 755 dirs)
- [ ] Remove test files from production
- [ ] Disable directory listing (.htaccess)
- [ ] Add SSL certificate (https://)
- [ ] Set security headers in .htaccess
- [ ] Update CORS policy for your domain

### Email Configuration

- [ ] Configure mail server or SMTP:
  ```php
  // In backend/config.php, update sendEmail() function:
  // Option 1: Use PHP mail() (requires local mail server)
  // Option 2: Configure SMTP server credentials
  // Option 3: Integrate with services (SendGrid, Mailgun, AWS SES)
  ```

- [ ] Test email delivery:
  - Send test donation confirmation
  - Verify email arrives with no delays
  - Check spam/junk folders
  - Test all email types (donation, volunteer, contact)

### Domain & DNS

- [ ] Purchase domain name
- [ ] Point DNS to hosting server
- [ ] Set up email records (MX, SPF, DKIM)
- [ ] Verify SSL certificate installation
- [ ] Test https:// access

---

## 📊 Performance Optimization

### Database Query Optimization

```sql
-- Add indexes for frequently queried columns
CREATE INDEX idx_donation_created ON donations(created_at);
CREATE INDEX idx_volunteer_status ON volunteers(status);
CREATE INDEX idx_message_read ON contact_messages(is_read);

-- Monitor slow queries
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 0.5;

-- Analyze table performance
ANALYZE TABLE donations, volunteers, contact_messages;
```

### Caching Strategy

```php
// Enable browser caching (.htaccess)
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresDefault "access plus 1 week"
    ExpiresByType text/html "access plus 1 day"
    ExpiresByType application/javascript "access plus 1 month"
</IfModule>

// Enable Gzip compression
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/css application/javascript
</IfModule>
```

### Frontend Optimization

1. **Minify CSS and JavaScript**
2. **Compress images** (use modern formats like WebP)
3. **Lazy load** images and content
4. **Defer JavaScript** non-critical code
5. **Use CDN** for static assets

### Monitoring

```bash
# Monitor database size
SELECT 
    table_name,
    ROUND(((data_length + index_length) / 1024 / 1024), 2) as size_mb
FROM information_schema.tables
WHERE table_schema = 'brightpath_ngo'
ORDER BY size_mb DESC;

# Monitor slow queries
tail -f /var/log/mysql/slow.log

# Monitor server resources
top
df -h
free -h
```

---

## 🔐 Security Best Practices

### Input Validation

```php
// ✓ Use prepared statements
$stmt = $conn->prepare("SELECT * FROM donations WHERE email = ?");
$stmt->bind_param("s", $email);
$stmt->execute();

// ✗ Never use string concatenation
$query = "SELECT * FROM donations WHERE email = '$email'"; // DANGEROUS!
```

### Authentication

```php
// Hash admin passwords with bcrypt
$hash = password_hash($password, PASSWORD_BCRYPT);

// Verify password
if (password_verify($password, $hash)) {
    // Password correct
}
```

### HTTPS Configuration

```apache
# Force HTTPS redirect
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</IfModule>

# Add HSTS header
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
```

### Cross-Site Scripting (XSS) Prevention

```php
// Always escape output
echo htmlspecialchars($user_input, ENT_QUOTES, 'UTF-8');

// Use content security policy headers
header("Content-Security-Policy: default-src 'self'");
```

### Cross-Site Request Forgery (CSRF) Protection

```php
// Generate CSRF token
session_start();
$_SESSION['csrf_token'] = bin2hex(random_bytes(32));

// Validate token on submission
if (!hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'])) {
    die('CSRF token validation failed');
}
```

---

## 📈 Monitoring & Logging

### Error Logging

```php
// Configure error logging
ini_set('error_log', '/var/log/php/brightpath.log');
ini_set('log_errors', 1);
ini_set('display_errors', 0); // Never display in production

// Log errors
error_log("Donation error: " . $e->getMessage());
```

### Application Monitoring

```php
// Create monitoring log
function logEvent($level, $message, $data = array()) {
    $timestamp = date('Y-m-d H:i:s');
    $log_entry = "[$timestamp] [$level] $message";
    
    if (!empty($data)) {
        $log_entry .= " | " . json_encode($data);
    }
    
    error_log($log_entry);
}

// Usage
logEvent('INFO', 'Donation received', ['amount' => 100, 'donor' => 'John Doe']);
logEvent('ERROR', 'Database connection failed', ['error' => $error]);
```

### Database Monitoring

```sql
-- Monitor connection count
SHOW PROCESSLIST;

-- Check table sizes
SELECT table_name, ROUND(((data_length + index_length) / 1024 / 1024), 2) as size_mb
FROM information_schema.tables
WHERE table_schema = 'brightpath_ngo';

-- Monitor table fragmentation
CHECK TABLE donations;
OPTIMIZE TABLE donations;
```

---

## 🔄 Backup & Recovery

### Automated Backup Strategy

```bash
#!/bin/bash
# Daily backup script (backup.sh)

DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backups/brightpath"
DB_NAME="brightpath_ngo"
DB_USER="root"

# Create backup directory
mkdir -p $BACKUP_DIR

# Export database
mysqldump -u $DB_USER --password=$DB_PASS $DB_NAME > $BACKUP_DIR/db_$DATE.sql

# Compress backup
gzip $BACKUP_DIR/db_$DATE.sql

# Remove backups older than 30 days
find $BACKUP_DIR -name "db_*.sql.gz" -mtime +30 -delete

# Log backup
echo "Backup completed: $DATE" >> $BACKUP_DIR/backup.log
```

### Restore from Backup

```bash
# Extract backup
gunzip db_20260326_120000.sql.gz

# Restore database
mysql -u root --password=$PASS brightpath_ngo < db_20260326_120000.sql
```

---

## 📱 Mobile & Accessibility

### Mobile Testing

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="BrightPath Initiative">
<meta name="theme-color" content="#2c5f2d">
```

### Accessibility Checklist

- [ ] Proper semantic HTML structure
- [ ] ARIA labels for form inputs
- [ ] Color contrast ratios (4.5:1 minimum)
- [ ] Keyboard navigation support
- [ ] Screen reader testing
- [ ] Touch target sizes (minimum 44x44px)

---

## 📊 Analytics Integration

### Google Analytics Setup

```html
<!-- Add to index.html before </head> -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

### Track Form Submissions

```javascript
// Track donation submission
gtag('event', 'donation_submit', {
  'value': amount,
  'currency': 'USD',
  'cause': cause
});

// Track volunteer application
gtag('event', 'volunteer_apply', {
  'interest_area': area
});
```

---

## 💰 Payment Integration

### Stripe Integration (Example)

```php
// backend/process_payment.php
require_once 'config.php';
require_once 'vendor/autoload.php';

\Stripe\Stripe::setApiKey('sk_live_...');

try {
    $intent = \Stripe\PaymentIntent::create([
        'amount' => $amount * 100, // Convert to cents
        'currency' => 'usd',
        'payment_method' => $payment_method_id,
        'confirm' => true,
    ]);
    
    if ($intent->status == 'succeeded') {
        // Store donation in database
        // Send confirmation email
        sendResponse(true, 'Payment successful');
    }
} catch (\Stripe\Exception\CardException $e) {
    sendResponse(false, 'Card declined: ' . $e->getMessage());
}
```

---

## 🔔 Email Service Integration

### SendGrid Integration

```php
require 'vendor/autoload.php';
$email = new \SendGrid\Mail\Mail();

$email->setFrom("noreply@brightpath.org", "BrightPath Initiative");
$email->setSubject("Donation Confirmation");
$email->addTo($donor_email);
$email->addContent("text/html", $html_body);

$sendgrid = new \SendGrid(SENDGRID_API_KEY);
$response = $sendgrid->send($email);
```

### Mailgun Integration

```php
$mg = Mailgun\Mailgun::create('mg-api-key');

$mg->messages()->send('mail.brightpath.org', [
    'from'    => 'noreply@brightpath.org',
    'to'      => $donor_email,
    'subject' => 'Donation Confirmation',
    'html'    => $html_body
]);
```

---

## 📝 Documentation Standards

### Code Comments

```php
/**
 * Process donation submission and store in database
 * 
 * @param array $data Donation data from form
 * @return array Response with success status and message
 * @throws Exception If database error occurs
 */
function processDonation($data) {
    // Implementation
}
```

### README Sections

- Features and capabilities
- Installation instructions
- Configuration guide
- API documentation
- Troubleshooting section
- Contributing guidelines
- License information

---

## 🧪 Testing Strategy

### Unit Testing (PHPUnit)

```php
class DonationTest extends PHPUnit_Framework_TestCase {
    
    public function testValidateDonation() {
        $donation = ['amount' => 100, 'email' => 'test@example.com'];
        $this->assertTrue(validateDonation($donation));
    }
    
    public function testInvalidEmail() {
        $donation = ['amount' => 100, 'email' => 'invalid'];
        $this->assertFalse(validateDonation($donation));
    }
}
```

### Integration Testing

```php
public function testDonationWorkflow() {
    // Submit form
    $response = $this->post('/backend/submit_donation.php', $data);
    
    // Check database
    $db_entry = $this->getDatabase()->query('SELECT * FROM donations WHERE email = ?', $email);
    $this->assertNotEmpty($db_entry);
    
    // Check email sent
    $this->assertEmailSent($email, 'Thank You for Your Donation');
}
```

---

## 🚨 Incident Response

### Database Corruption

```bash
# Check database integrity
mysqlcheck -u root --password=$PASS brightpath_ngo

# Repair table if needed
mysqlcheck -u root --password=$PASS --repair brightpath_ngo
```

### Disk Space Issues

```bash
# Check available space
df -h

# Find large files
du -sh /*

# Clean up old backups
rm /backups/brightpath/db_*.sql.gz -mtime +30
```

### High CPU/Memory Usage

```bash
# List processes by CPU usage
ps aux --sort=-%cpu

# Kill problematic process
kill -9 {PID}

# Check for runaway queries
SHOW PROCESSLIST;
KILL {Thread_ID};
```

---

## 📅 Maintenance Schedule

### Daily
- [ ] Monitor error logs
- [ ] Check email delivery status
- [ ] Verify database connectivity

### Weekly
- [ ] Review form submissions
- [ ] Check database size growth
- [ ] Monitor server resources
- [ ] Backup database

### Monthly
- [ ] Update security patches
- [ ] Review analytics data
- [ ] Optimize database tables
- [ ] Test disaster recovery

### Quarterly
- [ ] Security audit
- [ ] Performance review
- [ ] Update documentation
- [ ] Plan feature improvements

---

## 🎓 Training & Knowledge Transfer

### Admin Training Topics

1. **Form Management**
   - Review submissions
   - Export data
   - Respond to inquiries

2. **Volunteer Management**
   - Review applications
   - Update status
   - Send messages

3. **Donor Management**
   - Track donations
   - Generate reports
   - Send thank you emails

4. **System Administration**
   - Monitor performance
   - Backup procedures
   - Emergency contacts

---

## 📞 Support & Maintenance Contracts

### Service Level Agreement (SLA)

- **99.9% Uptime** - System available 99.9% of time
- **1-Hour Response** - Critical issues addressed within 1 hour
- **24-Hour Resolution** - Non-critical issues resolved within 24 hours
- **7-Day Backup** - Backups kept for minimum 7 days

### Escalation Procedure

1. **Level 1** - Automated monitoring alerts
2. **Level 2** - Development team notification
3. **Level 3** - Management escalation
4. **Level 4** - Executive decision required

---

## 🎉 Launch Checklist

### 48 Hours Before

- [ ] Final full backup
- [ ] Complete testing cycle
- [ ] Notify team of launch
- [ ] Prepare communication materials
- [ ] Set up monitoring alerts

### Launch Day

- [ ] Team on standby
- [ ] Monitor error logs closely
- [ ] Test all forms multiple times
- [ ] Verify email delivery
- [ ] Check mobile access

### Post-Launch (1 Week)

- [ ] Daily monitoring
- [ ] Quick bug fixes
- [ ] Gather user feedback
- [ ] Document issues
- [ ] Plan improvements

---

**Deployment & Best Practices Guide v1.0**  
**Last Updated:** March 2026  
**Status:** Ready for Production
