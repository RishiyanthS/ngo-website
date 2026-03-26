# 🚀 Installation Checklist - Print & Follow

## Step 1: Prepare Your Computer ☐

- [ ] Download and install XAMPP/WAMP/MAMP (free development server)
- [ ] Start Apache module
- [ ] Start MySQL module
- [ ] Verify services are running (green indicators)

**Recommended:**
- XAMPP for Windows: https://www.apachefriends.org/
- WAMP for Windows: https://www.wampserver.com/
- MAMP for Mac: https://www.mamp.info/

---

## Step 2: Copy Files ☐

### Option A: From USB Drive
- [ ] Copy `ngo-website` folder from USB
- [ ] Paste into web root:
  - Windows XAMPP: `C:\xampp\htdocs\`
  - Windows WAMP: `C:\wamp\www\`
  - Mac MAMP: `/Applications/MAMP/htdocs/`

### Option B: From Downloaded ZIP
- [ ] Extract `ngo-website.zip`
- [ ] Move folder to web root (see Option A locations)

**Verify:** Confirm folder structure
```
htdocs/ngo-website/
├── index.html
├── backend/
├── database/
└── [other folders]
```

---

## Step 3: Access Setup Wizard ☐

1. [ ] Open browser (Chrome, Firefox, Safari, Edge)
2. [ ] Go to: `http://localhost/ngo-website/database/setup.php`
3. [ ] Click "Setup" or wait for auto-setup
4. [ ] **Save the success page** (screenshot or print)
5. [ ] Check "Setup Complete!" message ✓

**If error appears:**
- [ ] Go to: `http://localhost/phpmyadmin`
- [ ] Check MySQL is running
- [ ] Try setup.php again

---

## Step 4: Test the Website ☐

1. [ ] Go to: `http://localhost/ngo-website/`
2. [ ] Verify homepage loads with content
3. [ ] Check all page links work:
   - [ ] Home
   - [ ] About Us
   - [ ] Causes
   - [ ] Impact
   - [ ] Donate
   - [ ] Volunteer
   - [ ] Contact

---

## Step 5: Test Donation Form ☐

1. [ ] Click "Donate Now" or go to Donate section
2. [ ] Select amount ($50)
3. [ ] Fill in name and email
4. [ ] Click "Complete Donation"
5. [ ] Verify success message appears ✓
6. [ ] Check email (check spam folder) for confirmation

**Expected:** Donation confirmation email in inbox

---

## Step 6: Test Volunteer Form ☐

1. [ ] Go to Volunteer section
2. [ ] Fill all required fields (*):
   - [ ] Full Name
   - [ ] Email
   - [ ] Phone
   - [ ] Location
   - [ ] Interest Area
   - [ ] Availability
   - [ ] Motivation
3. [ ] Click "Submit Application"
4. [ ] Verify success message ✓
5. [ ] Check email for confirmation

**Expected:** Confirmation email in inbox

---

## Step 7: Test Contact Form ☐

1. [ ] Go to Contact section
2. [ ] Fill in all fields:
   - [ ] Name
   - [ ] Email
   - [ ] Subject
   - [ ] Message
3. [ ] Click "Send Message"
4. [ ] Verify success message ✓
5. [ ] Check email for acknowledgment

**Expected:** Acknowledgment email in inbox

---

## Step 8: Customize Your Site ☐

### Change Organization Name
1. [ ] Open `index.html` with text editor (Notepad, VS Code, etc.)
2. [ ] Find: `BrightPath Initiative`
3. [ ] Replace with your NGO name (⌘F to find)
4. [ ] Save file (Ctrl+S)
5. [ ] Refresh browser to see changes

### Related Places to Update
- [ ] Hero section heading
- [ ] Navigation menu
- [ ] Footer
- [ ] Email addresses
- [ ] Page titles

### Change Contact Information
Find and replace:
- [ ] Phone: `+1 (555) 123-4567`
- [ ] Email: `info@brightpathinitiative.org`
- [ ] Address: `123 Hope Street`

### Change Colors (Optional)
1. [ ] Open `index.html`
2. [ ] Find: `:root {` (around line 30)
3. [ ] Change color codes:
   - `--primary: #2c5f2d;` (green) → your color hex
   - `--accent: #ff6b35;` (orange) → your color hex
4. [ ] Save and refresh

**Color Tool:** https://www.color-hex.com/

---

## Step 9: Database Connection Test ☐

1. [ ] Go to: `http://localhost/ngo-website/backend/test-connection.php`
2. [ ] Check all items have ✓ marks:
   - [ ] Database Connection
   - [ ] donations table
   - [ ] volunteers table
   - [ ] contact_messages table
   - [ ] admin_users table

**If any item shows ✗:**
- [ ] Run setup again: `database/setup.php`
- [ ] Check MySQL is running
- [ ] Restart Apache and MySQL

---

## Step 10: Data Export Test ☐

1. [ ] Go to: `http://localhost/ngo-website/backend/export.php`
2. [ ] Try exporting as CSV:
   - [ ] Select "Donations"
   - [ ] Click "Download Data"
   - [ ] File should download (brightpath_donations_*.csv)
3. [ ] Try exporting as JSON:
   - [ ] Select "Donations"
   - [ ] Select "JSON" format
   - [ ] Download file

**Files should open in Excel/spreadsheet or text editor**

---

## Step 11: Email Configuration (Optional) ☐

If emails don't arrive:

1. [ ] Open `backend/config.php`
2. [ ] Find line: `define('ADMIN_EMAIL', ...)`
3. [ ] Change to your email address
4. [ ] Find line: `define('VOLUNTEER_EMAIL', ...)`
5. [ ] Change to volunteer coordinator email
6. [ ] Save file
7. [ ] Test form submissions again

**Popular Email Services:**
- Gmail
- Outlook
- Yahoo
- Your hosting provider's email

---

## Step 12: Verify All Forms Store Data ☐

### Check Donations
1. [ ] Submit a test donation
2. [ ] Go to: `http://localhost/phpmyadmin`
3. [ ] Click `brightpath_ngo` database
4. [ ] Click `donations` table
5. [ ] Verify your test donation appears ✓

### Check Volunteers
1. [ ] Submit test volunteer form
2. [ ] In phpMyAdmin, click `volunteers` table
3. [ ] Verify application appears ✓

### Check Contact Messages
1. [ ] Submit test contact form
2. [ ] In phpMyAdmin, click `contact_messages` table
3. [ ] Verify message appears ✓

---

## Step 13: Browser Testing ☐

Test on different browsers:

- [ ] Google Chrome
- [ ] Firefox
- [ ] Safari (if on Mac)
- [ ] Microsoft Edge
- [ ] Mobile browser (phone/tablet)

**Check for:**
- [ ] Content displays properly
- [ ] Forms work and submit
- [ ] Buttons are clickable
- [ ] Colors look good
- [ ] Text is readable

---

## Step 14: Mobile Testing ☐

1. [ ] Open browser DevTools (F12)
2. [ ] Click mobile device icon (or Ctrl+Shift+M)
3. [ ] Test different screen sizes:
   - [ ] iPhone 12 (390x844)
   - [ ] iPad (820x1180)
   - [ ] Android (360x800)
4. [ ] Verify all pages responsive:
   - [ ] Menu works on mobile
   - [ ] Forms are easy to fill
   - [ ] Buttons are large enough
   - [ ] No horizontal scrolling

---

## Step 15: Final Verification ☐

### Security Check
- [ ] No error messages displayed
- [ ] No sensitive data visible
- [ ] Database credentials only in config.php
- [ ] File permissions correct (644 files, 755 dirs)

### Performance Check
- [ ] Website loads in under 2 seconds
- [ ] Animations are smooth
- [ ] Forms submit quickly
- [ ] No browser console errors (F12)

### Functionality Check
- [ ] All navigation links work
- [ ] All forms submit successfully
- [ ] Email confirmations arrive
- [ ] Data saves to database
- [ ] Export function works

---

## 🎉 Ready to Launch!

If all checkboxes are ✓, your website is ready!

### Next Steps:

**For Local Use Only:**
- Enjoy your website on localhost
- Test with real data
- Track submissions in database
- Export reports

**For Production (Go Live):**
- [ ] Purchase domain name
- [ ] Get hosting with PHP & MySQL
- [ ] Update credentials in config.php
- [ ] Run setup.php on live server
- [ ] Configure SSL/HTTPS
- [ ] Set up email service
- [ ] Update links in documentation
- [ ] Go live!

---

## 📞 Need Help?

### Check These Resources:
1. [ ] README.md - Complete setup guide
2. [ ] QUICKSTART.md - Quick reference
3. [ ] API.md - Developer documentation
4. [ ] DEPLOYMENT.md - Production guide

### Common Issues:

**Forms not working?**
- [ ] Check browser console (F12)
- [ ] Verify PHP files exist in backend/
- [ ] Test database connection: test-connection.php

**Database errors?**
- [ ] Run setup.php again
- [ ] Check MySQL is running
- [ ] Verify credentials in config.php

**Email not arriving?**
- [ ] Check spam/junk folder
- [ ] Verify email config in config.php
- [ ] Test with different email provider

**Website not loading?**
- [ ] Verify Apache is running
- [ ] Check folder in correct location
- [ ] Try accessing: localhost/phpmyadmin

---

## ✅ Completion Sign-Off

**I have successfully:**
- [ ] Completed all 15 steps
- [ ] Tested all forms
- [ ] Verified data storage
- [ ] Customized the site
- [ ] Confirmed everything works

**Date Completed:** _______________

**By:** _______________

**Questions or Issues?**
Contact: admin@brightpathinitiative.org

---

**BrightPath Initiative Setup v1.0**  
**Status: Ready for Use** ✓  
**Print Friendly: Yes**
