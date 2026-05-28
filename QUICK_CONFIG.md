# ⚡ Quick Configuration Checklist

## For EmailJS Method (Easiest)

### Step 1: Get Your Credentials
- [ ] Create EmailJS account at https://www.emailjs.com/
- [ ] Create email templates: `order_to_owner` and `order_confirmation`
- [ ] Copy your **Public Key** from Account section
- [ ] Copy your **Service ID** from Email Services

### Step 2: Open checkout-details.html in Text Editor

### Step 3: Find and Replace (3 changes needed)

#### Change #1 - Line ~16
**Find:**
```javascript
emailjs.init("YOUR_EMAILJS_PUBLIC_KEY");
```

**Replace with:**
```javascript
emailjs.init("3pK8x7nQ9vR2mL5bJ8wH");  // Use your actual key
```

---

#### Change #2 - Line ~89
**Find:**
```javascript
to_email: "OWNER_EMAIL@example.com",
```

**Replace with:**
```javascript
to_email: "your-email@gmail.com",  // Your actual email to receive orders
```

---

#### Change #3 - Lines ~106 and ~118
**Find:**
```javascript
emailjs.send("YOUR_SERVICE_ID", "YOUR_OWNER_TEMPLATE_ID", ownerEmailParams)
```

**Replace with:**
```javascript
emailjs.send("service_abc123", "order_to_owner", ownerEmailParams)
```

---

**Find:**
```javascript
emailjs.send("YOUR_SERVICE_ID", "YOUR_CUSTOMER_TEMPLATE_ID", customerEmailParams)
```

**Replace with:**
```javascript
emailjs.send("service_abc123", "order_confirmation", customerEmailParams)
```

---

### Step 4: Save File
- Save checkout-details.html
- Done! ✅

### Step 5: Test
1. Add product to cart
2. Go to checkout
3. Fill address and complete payment
4. Check your email for order notification

---

## Credentials Location (Where to find them):

### In EmailJS Dashboard:
- **Public Key**: Account → Public Key
- **Service ID**: Email Services → Click service → Service ID
- **Template IDs**: Email Templates → Each template shows ID

---

## Email Example (What You'll Receive):

```
Subject: New Order #KM1234567890 - John Doe

New Order Received!

Order Details
Order ID: KM1234567890
Date: May 28, 2026 3:45 PM
Payment ID: pay_abc123xyz

Customer Information
Name: John Doe
Email: john@example.com
Phone: 9876543210

📍 Delivery Address
123 Main Street
New Delhi - 110001

📦 Order Items
Knitted Shawl (Qty: 2) - ₹2000

💰 Total Amount: ₹2000
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No emails sent | Check console (F12), verify credentials are correct |
| Emails in spam | Check spam folder, mark as not spam |
| Template not found | Verify template name exactly matches in code |
| CORS error | Email permissions on EmailJS might need checking |

---

## Still Not Working?

1. Check browser console: F12 → Console tab
2. Verify all 3 credentials are correct
3. Make sure EmailJS templates are created
4. Try test email in EmailJS dashboard first

Need help? Read the full EMAIL_SETUP_GUIDE.md

