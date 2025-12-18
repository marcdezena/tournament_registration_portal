# 🎮 Registration Approval System Implementation

## Overview

A comprehensive pending registration approval system has been implemented for the Tournament Management System. Players now submit detailed registration applications that require organizer approval, with automated email notifications at each stage of the process.

---

## ✨ Key Features

### 1. Enhanced Registration Form
Players provide comprehensive information during registration:
- **Phone Number** - Emergency contact information
- **Experience Level** - Skill categorization (Beginner → Professional)
- **Preferred Role** - Position preference for team-based tournaments
- **Additional Information** - Free-form details about playstyle, availability, etc.
- **Notes to Organizer** - Special requests or information

### 2. Approval Workflow
- ⏳ **Pending Status**: All registrations start as "pending"
- ✅ **Organizer Approval**: Manual review and acceptance required
- ❌ **Rejection with Feedback**: Optional reason provided to players
- 📊 **Participant Counts**: Only confirmed players count toward tournament capacity

### 3. Email Notifications
Professional HTML email templates for:
- 📧 **Registration Submitted** - Immediate confirmation upon registration
- 🎉 **Registration Approved** - Welcome message with tournament details
- 📝 **Registration Rejected** - Polite notification with organizer's reason

### 4. In-App Notifications
Complementary notification center alerts for:
- Registration submission confirmation
- Approval notifications
- Rejection notifications with reasons

### 5. Enhanced Organizer Dashboard
- View all participant information in one place
- Quick approve/reject buttons for pending applications
- Display of all submitted player details
- Organized layout showing contact info, experience, role, and notes

---

## 📦 Installation

### Prerequisites
- PHP 7.4 or higher
- MySQL database
- Composer (PHP dependency manager)
- SMTP email server access (Gmail, SendGrid, etc.)

### Step 1: Install Dependencies

```bash
cd "c:\xampp\htdocs\GitHub Repos\Tournament-Management-System"
composer install
```

This installs PHPMailer for email functionality.

### Step 2: Database Migration

Run the SQL migration to add new columns:

```bash
mysql -u root -p tournament_management < backend/database/add_registration_fields.sql
```

Or execute via phpMyAdmin/MySQL Workbench.

### Step 3: Email Configuration

1. Copy the example config:
```bash
cp backend/config/email_config.example.php backend/config/email_config.php
```

2. Edit `backend/config/email_config.php` with your SMTP credentials:

```php
'smtp' => [
    'host' => 'smtp.gmail.com',
    'port' => 587,
    'username' => 'your-email@gmail.com',
    'password' => 'your-app-password',  // Gmail App Password
    'encryption' => 'tls',
],
```

**Gmail Setup:**
1. Enable 2-Factor Authentication
2. Generate App Password: [Google Account Settings](https://myaccount.google.com/apppasswords)
3. Use the 16-character app password

### Step 4: Update Application URL

In `email_config.php`, set your app URL:
```php
'app_url' => 'http://localhost/Tournament-Management-System',
```

---

## 🎯 Usage Guide

### For Players

1. **Browse Tournaments**
   - Navigate to tournament listing
   - Click on a tournament to view details

2. **Register for Tournament**
   - Click "Join Tournament" button
   - Fill out the registration form:
     - Contact phone number (optional but recommended)
     - Select experience level
     - Enter preferred role (if team-based)
     - Add any additional information
     - Include notes for the organizer
   - Submit registration

3. **Wait for Approval**
   - Receive immediate email confirmation
   - Check in-app notifications
   - Wait for organizer to review application

4. **Get Notified**
   - Receive email when approved or rejected
   - If rejected, organizer's reason is included
   - Check tournament details page for status

### For Organizers

1. **View Pending Applications**
   - Navigate to "Manage Tournaments"
   - Click "View Participants" on your tournament
   - See all pending, confirmed, and rejected players

2. **Review Application Details**
   - View player contact information
   - Check experience level
   - Review preferred role
   - Read additional information and notes

3. **Approve Applications**
   - Click "Approve" button
   - Player receives approval email
   - Status changes to "confirmed"
   - Participant count incremented

4. **Reject Applications**
   - Click "Reject" button
   - Enter reason for rejection (optional)
   - Player receives rejection email with reason
   - Status changes to "rejected"

---

## 🗂️ File Structure

### New Files Created

```
backend/
├── classes/
│   └── EmailNotification.class.php       # Email handler class
├── config/
│   ├── email_config.php                  # SMTP settings (DO NOT COMMIT)
│   └── email_config.example.php          # Example configuration
├── database/
│   └── add_registration_fields.sql       # Database migration
└── templates/
    └── emails/
        ├── registration_submitted.php    # Submission email template
        ├── registration_approved.php     # Approval email template
        └── registration_rejected.php     # Rejection email template

composer.json                              # PHPMailer dependency
REGISTRATION_APPROVAL_SETUP.md            # Detailed setup guide
REGISTRATION_APPROVAL_SUMMARY.md          # Quick reference
```

### Modified Files

```
backend/api/tournament_api.php            # Added approval logic & email notifications
frontend/app/views/pages/home/
├── tournament-details.php                # Enhanced registration form
└── manage-tournaments.php                # Improved participant management
.gitignore                                # Protected email config
```

---

## 🔧 Configuration

### Email Settings

**Gmail (Recommended for Testing)**
```php
'smtp' => [
    'host' => 'smtp.gmail.com',
    'port' => 587,
    'encryption' => 'tls',
]
```

**SendGrid**
```php
'smtp' => [
    'host' => 'smtp.sendgrid.net',
    'port' => 587,
    'username' => 'apikey',
    'password' => 'SG.xxxxx',
]
```

**Office 365**
```php
'smtp' => [
    'host' => 'smtp.office365.com',
    'port' => 587,
]
```

### Debug Mode

Enable SMTP debugging:
```php
'settings' => [
    'debug' => true,  // Shows SMTP conversation in error log
]
```

---

## 🧪 Testing

### Quick Test Checklist

- [ ] **Database**: Migration successful, new columns exist
- [ ] **Dependencies**: PHPMailer installed via Composer
- [ ] **Configuration**: SMTP credentials set correctly
- [ ] **Registration**: Form displays all new fields
- [ ] **Pending Status**: New registrations have "pending" status
- [ ] **Email (Submit)**: Registration confirmation email received
- [ ] **Notification (Submit)**: In-app notification created
- [ ] **Organizer View**: All participant info visible
- [ ] **Approval**: Email sent, status updated to "confirmed"
- [ ] **Rejection**: Reason prompt appears, rejection email sent
- [ ] **Participant Count**: Only confirmed players counted

### Test Registration

1. Create/use test tournament
2. Register with test player account
3. Fill all optional fields
4. Submit registration
5. Check email inbox for confirmation
6. Login as organizer
7. Approve or reject
8. Verify email received

---

## 🚨 Troubleshooting

### Emails Not Sending

**Check SMTP Credentials**
```php
// Verify in email_config.php
'username' => 'correct-email@gmail.com',
'password' => 'correct-app-password',
```

**Enable Debug Mode**
```php
'settings' => [
    'debug' => true,
]
```

**Check Error Logs**
- Location: `C:\xampp\apache\logs\error.log`
- Look for PHPMailer errors

**Test SMTP Independently**
Create `test-email.php` in project root:
```php
<?php
require_once 'vendor/autoload.php';
require_once 'backend/api/database.php';
require_once 'backend/classes/EmailNotification.class.php';

$database = new Database();
$db = $database->getConnection();
$email = new EmailNotification($db);

$result = $email->sendRegistrationSubmitted(
    'your-email@example.com',
    'Test User',
    'Test Tournament',
    1
);

echo $result ? "✅ Email sent successfully!" : "❌ Email failed to send";
```

### Database Issues

**Verify Columns Exist**
```sql
DESCRIBE tournament_participants;
```

Should show: `phone_number`, `experience_level`, `player_role`, `additional_info`

**Re-run Migration**
```bash
mysql -u root -p tournament_management < backend/database/add_registration_fields.sql
```

### Permission Errors

Ensure organizer has correct permissions:
- Must be tournament creator OR
- Must have Admin role

---

## 🔐 Security Considerations

### Production Deployment

1. **Environment Variables**
   ```php
   'username' => $_ENV['SMTP_USERNAME'],
   'password' => $_ENV['SMTP_PASSWORD'],
   ```

2. **Never Commit Credentials**
   - `email_config.php` is in `.gitignore`
   - Use `email_config.example.php` as template

3. **Input Validation**
   - All user inputs sanitized
   - HTML escaped in emails
   - SQL parameterized queries

4. **Rate Limiting**
   - Consider limiting registration attempts
   - Prevent email spam abuse

5. **HTTPS Required**
   - Use SSL/TLS in production
   - Secure email transmission

---

## 📊 Database Schema

### New Columns in `tournament_participants`

| Column | Type | Description |
|--------|------|-------------|
| `phone_number` | VARCHAR(20) | Player contact number |
| `experience_level` | ENUM | beginner, intermediate, advanced, professional |
| `player_role` | VARCHAR(50) | Preferred position/role |
| `additional_info` | TEXT | Extra player information |

### Existing Registration Status

```sql
ENUM('pending', 'confirmed', 'waitlist', 'rejected', 'withdrawn')
DEFAULT 'pending'
```

---

## 🎨 Email Templates

All email templates are responsive HTML located in:
`backend/templates/emails/`

### Customization

Edit templates to match your branding:
- Colors and gradients
- Logo placement
- Content and messaging
- Links and CTAs

Templates use PHP for dynamic content:
```php
<?php echo htmlspecialchars($userName); ?>
<?php echo htmlspecialchars($tournamentName); ?>
```

---

## 📈 Future Enhancements

Potential improvements:
- [ ] Bulk approve/reject operations
- [ ] Custom email templates via admin panel
- [ ] SMS notifications integration
- [ ] Automated approval based on criteria
- [ ] Waitlist management
- [ ] Payment integration with approval
- [ ] Multi-language email support
- [ ] Email delivery tracking
- [ ] Rejection appeal system

---

## 📚 Additional Resources

- **Setup Guide**: `REGISTRATION_APPROVAL_SETUP.md` - Detailed installation
- **Quick Reference**: `REGISTRATION_APPROVAL_SUMMARY.md` - API changes & workflow
- **PHPMailer Docs**: https://github.com/PHPMailer/PHPMailer
- **Gmail App Passwords**: https://support.google.com/accounts/answer/185833

---

## 🙋 Support

For issues or questions:
1. Check error logs (`C:\xampp\apache\logs\error.log`)
2. Enable debug mode in email config
3. Verify SMTP credentials
4. Test database schema
5. Review PHPMailer documentation

---

## ✅ Implementation Complete

All features have been successfully implemented:
- ✅ Database schema updated
- ✅ Email notification system integrated
- ✅ Registration form enhanced
- ✅ Approval workflow functional
- ✅ Organizer dashboard improved
- ✅ Documentation complete

**Status**: Ready for testing and deployment! 🚀
