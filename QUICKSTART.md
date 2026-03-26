# BrightPath Initiative - Quick Start Guide

## ⚡ 5-Minute Setup

### 1. **Copy Files to Web Server**
- Copy `ngo-website` folder to your web server root
  - XAMPP: `C:\xampp\htdocs\ngo-website`
  - WAMP: `C:\wamp\www\ngo-website`
  - MAMP: `/Applications/MAMP/htdocs/ngo-website`

### 2. **Start Services**
- Start Apache & MySQL from your development environment
- Open browser: `http://localhost`

### 3. **Initialize Database**
- Navigate to: `http://localhost/ngo-website/database/setup.php`
- Click setup or let it auto-run
- You'll see success message with statistics

### 4. **Access Website**
- Go to: `http://localhost/ngo-website/`
- Website is now live and functional!

---

## 🎯 Key Features Ready to Use

### Donation System ✅
- **5 Quick Amounts**: $25, $50, $100, $250, $500, $1000
- **Custom Amount**: Donor can enter any amount
- **Cause Selection**: Direct donation to specific initiatives
- **Monthly Recurring**: Support sustainability
- **Anonymous Donations**: Privacy option
- **Auto Confirmation**: Email sent to donor immediately

### Volunteer Platform ✅
- **8 Interest Areas**: Education, Healthcare, Water, Agriculture, Women, Emergency, Fundraising, Admin
- **Availability Options**: Weekends, Weekdays, Flexible, Summer, Occasional
- **Skills Assessment**: Track experience and motivation
- **Auto Status**: Applications tracked from pending to approval
- **Contact System**: Volunteer coordinator notified automatically

### Contact Management ✅
- **3-Section Form**: Name, Email, Subject, Message
- **Auto Response**: Sender gets acknowledgment email
- **Admin Notification**: Team alerted of inquiries
- **Spam Prevention**: Message length validation
- **Reference Numbers**: Track all communications

### Impact Tracking ✅
- **6 Active Causes**: Each with progress bar
- **Real-Time Stats**: Update donation amounts
- **Fund Allocation**: 85% programs, 10% admin, 5% fundraising
- **Animated Counters**: Engagement-friendly display

---

## 📝 Customize Your Site

### Change Organization Name
1. Open `index.html`
2. Find: `<title>BrightPath Initiative`
3. Replace with your NGO name
4. Update in other places: logo, footer, emails

### Update Contact Info
Search in `index.html` for:
- Phone: `+1 (555) 123-4567`
- Email: `info@brightpathinitiative.org`
- Address: `123 Hope Street`

### Modify Causes
Around **line 400** in index.html:
```html
<h3>Your New Cause</h3>
<p>Your description...</p>
```

Also add to donation form:
```html
<option value="your-cause">Your Cause Name</option>
```

### Change Colors
In `index.html` CSS, find:
```css
--primary: #2c5f2d;        /* Green - change to your color */
--accent: #ff6b35;         /* Orange - change to your color */
```

---

## 🔧 Testing Forms

### Test Donation Form
1. Navigate to "Donate" section
2. Select amount or enter custom
3. Fill donor details
4. Submit
5. Check console (F12) for success message

### Test Volunteer Form
1. Navigate to "Volunteer" section
2. Enter all required information
3. Submit
4. Check console for confirmation

### Test Contact Form
1. Navigate to "Contact" section
2. Fill form with test message
3. Submit
4. Check for success message

---

## 📊 Database Tables

### donations
Tracks all donations with:
- Donor info (name, email)
- Amount and cause
- Monthly/Anonymous flags
- Transaction ID
- Status and timestamps

### volunteers
Manages applications with:
- Personal info (name, email, phone, location)
- Interest area and availability
- Experience and motivation
- Status (pending/approved/rejected)
- Review tracking

### contact_messages
Stores inquiries with:
- Sender details
- Subject and message
- Read status
- Response tracking

### admin_users
Manages access with:
- Admin credentials
- Role-based permissions
- Activity tracking

---

## ✉️ Email Setup

### Using PHP Mail()
Default setup uses PHP's `mail()` function:
1. Requires local mail server
2. Works on most hosting providers
3. No additional configuration needed

### Using SMTP (Recommended)
For reliable email delivery:
1. Update `sendEmail()` function in `backend/config.php`
2. Add SMTP server credentials
3. Test email delivery

### Email Templates
Located in form processors:
- `backend/submit_donation.php` - Donation confirmations
- `backend/submit_volunteer.php` - Volunteer invites
- `backend/submit_contact.php` - Contact acknowledgments

---

## 🐛 Quick Fixes

### Forms not working?
```
1. Check browser console (F12 → Console tab)
2. Look for red error messages
3. Verify backend files exist
4. Check database connection
```

### Database error?
```
1. Open phpMyAdmin: http://localhost/phpmyadmin
2. Check if brightpath_ngo database exists
3. Verify user 'root' can access it
4. Update credentials in backend/config.php
```

### Styling looks wrong?
```
1. Clear browser cache (Ctrl+Shift+Delete)
2. Hard refresh page (Ctrl+Shift+R)
3. Check CSS is loading (F12 → Network tab)
```

---

## 📈 Next Steps

### Immediate (Day 1)
- [ ] Customize organization name and colors
- [ ] Update contact information
- [ ] Test all forms
- [ ] Review database setup

### Short-Term (Week 1)
- [ ] Configure email system
- [ ] Update cause descriptions
- [ ] Add your team photos
- [ ] Customize testimonials

### Medium-Term (Month 1)
- [ ] Set up admin panel
- [ ] Train team on system
- [ ] Go live with real donations
- [ ] Start accepting volunteers

### Long-Term (Quarter 1)
- [ ] Add payment gateway integration
- [ ] Implement monthly recurring donations
- [ ] Create volunteer CRM
- [ ] Set up analytics

---

## 🎓 Learning Resources

### Modify Frontend
- `index.html` - All pages and styles
- `js/form-handler.js` - Form handling logic

### Modify Backend
- `backend/config.php` - Database and email config
- `backend/submit_*.php` - Form processors

### Database
- `database/database.sql` - Schema definition
- `database/setup.php` - Setup script

---

## 💡 Pro Tips

- **Mobile Testing**: Use browser DevTools (F12) to preview mobile
- **Form Testing**: Check console (F12) for error details
- **Database Backup**: Export tables weekly
- **Email Testing**: Use temp email service like Mailtrap
- **Analytics**: Add Google Analytics to track site traffic
- **Deployment**: Use Git for version control

---

## 🚀 Ready to Deploy?

1. **Backup Everything** - Copy ngo-website folder
2. **Test Thoroughly** - Go through all forms
3. **Update Config** - Change emails and database
4. **Set SSL** - Ensure HTTPS on live server
5. **Go Live** - Deploy to production server

---

## 📞 Stuck?

**Check These:**
1. browser console (F12 → Console)
2. Web server error logs
3. PHP error logs
4. Database connection status
5. File permissions on server

**Common Issues:**
- Forms not submitting → Check form-handler.js is loaded
- Database error → Verify setup.php ran successfully
- Emails not sending → Check email config in config.php
- Styling wrong → Hard refresh page (Ctrl+Shift+R)

---

**Your NGO website is ready to launch!** 🎉

For more help, see README.md or check the inline code comments.
