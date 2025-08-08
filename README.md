# 📧 Email Sending from a Linux Workstation using `msmtp`

This guide explains how to configure a Linux workstation to send emails using **Gmail SMTP** via `msmtp`. This is a lightweight and simple alternative to running a full mail server, ideal for sending automated or script-based notifications.

---

## ✅ Prerequisites

Install the required packages:

```bash
sudo apt update
sudo apt install msmtp mailutils

🔐 Step 1: Generate a Gmail App Password
To use Gmail's SMTP server, you need an App Password (not your Gmail account password):

Go to: https://myaccount.google.com/apppasswords


🛠️ Step 2: Configure msmtp
Create the ~/.msmtprc file:

Add the following configuration (replace with your Gmail address and App Password):
account default
host smtp.gmail.com
port 587
auth on
user yourname@gmail.com
password your_app_password
from yourname@gmail.com
tls on
tls_starttls on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
logfile ~/.msmtp.log

🔐 Step 3: Set Secure File Permissions
chmod 600 ~/.msmtprc

🧯 Step 4: Backup Existing sendmail (Optional)
If a sendmail binary exists, back it up:
sudo mv /usr/sbin/sendmail /usr/sbin/sendmail.bak 2>/dev/null

🔗 Step 5: Use msmtp as sendmail
Link msmtp to sendmail so it can be used by applications expecting sendmail:
sudo ln -s /usr/bin/msmtp /usr/sbin/sendmail

✅ Step 6: Send a Test Email
Try sending a test email from the terminal:

echo "This is a test email from msmtp." | mail -s "Test Email" recipient@example.com
Check the log if needed:

cat ~/.msmtp.log

📎 Notes
This setup supports sending email only, not receiving.

Do not use your Gmail password—use an App Password for security.

Works well for sending emails from cron jobs, bash scripts, or system alerts.

🧪 Example Use Case in a Script
#!/bin/bash
MESSAGE="Backup completed successfully."
echo "$MESSAGE" | mail -s "Backup Notification" yourname@gmail.com
