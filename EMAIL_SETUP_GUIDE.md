# Email Configuration Guide for KNOT MUSEE Orders

This guide will help you set up automatic email notifications when customers place orders with their delivery addresses.

## Quick Setup (5 minutes)

### Step 1: Create an EmailJS Account
1. Go to [EmailJS.com](https://www.emailjs.com/)
2. Click "Sign Up Free" (no credit card required)
3. Create your account and verify your email

### Step 2: Add Your Email Service
1. In EmailJS dashboard, go to **Email Services**
2. Click **Add Service** → Select **Gmail** (or your email provider)
3. Follow the instructions to connect your email
4. Copy your **Service ID** (format: `service_xxxxx`)

### Step 3: Create Email Templates

#### Template 1: Owner Notification (Order Details with Address)
1. Go to **Email Templates** in EmailJS
2. Click **Create New Template**
3. Name it: `order_to_owner`
4. Set **To Email**: `{{to_email}}`
5. Set **Subject**: `New Order #{{order_id}} - {{customer_name}}`
6. Copy this template content into the **HTML Content**:

```html
<h2>New Order Received!</h2>
<hr>
<h3>Order Details</h3>
<p><strong>Order ID:</strong> {{order_id}}</p>
<p><strong>Date:</strong> {{order_date}}</p>
<p><strong>Payment ID:</strong> {{payment_id}}</p>

<h3>Customer Information</h3>
<p><strong>Name:</strong> {{customer_name}}</p>
<p><strong>Email:</strong> {{customer_email}}</p>
<p><strong>Phone:</strong> {{customer_phone}}</p>

<h3>📍 Delivery Address</h3>
<p>
  {{delivery_address}}<br>
  {{city}} - {{pincode}}
</p>

<h3>📦 Order Items</h3>
<pre>{{items_list}}</pre>

<h3>💰 Total Amount: ₹{{total_amount}}</h3>

<hr>
<p><em>Please prepare and ship this order accordingly.</em></p>
```

7. Click **Save**

#### Template 2: Customer Confirmation
1. Click **Create New Template** again
2. Name it: `order_confirmation`
3. Set **To Email**: `{{to_email}}`
4. Set **Subject**: `Your Order #{{order_id}} Confirmed - KNOT MUSEE`
5. Copy this template content:

```html
<h2>Thank You for Your Order!</h2>
<p>Hi {{customer_name}},</p>
<p>Your order has been successfully confirmed.</p>

<h3>Order Details</h3>
<p><strong>Order ID:</strong> {{order_id}}</p>

<h3>📍 Delivery Address</h3>
<p>
  {{delivery_address}}<br>
  {{city}} - {{pincode}}
</p>

<h3>📦 Items Ordered</h3>
<pre>{{items_list}}</pre>

<h3>💰 Total Amount: ₹{{total_amount}}</h3>

<hr>
<p>Your order will be shipped soon. We'll send you tracking details shortly.</p>
<p>Thank you for supporting KNOT MUSEE!</p>
```

6. Click **Save**

### Step 4: Get Your Credentials
1. In EmailJS dashboard, go to **Account** section
2. Copy your **Public Key** (format: `xxxxxxxxxxxxxxxxxxxxx`)

### Step 5: Update Your Website
1. Open `checkout-details.html` in your text editor
2. Find the line with: `emailjs.init("YOUR_EMAILJS_PUBLIC_KEY");`
   - Replace `YOUR_EMAILJS_PUBLIC_KEY` with your actual Public Key

3. Find the line with: `emailjs.send("YOUR_SERVICE_ID", "YOUR_OWNER_TEMPLATE_ID", ownerEmailParams)`
   - Replace `YOUR_SERVICE_ID` with your Service ID
   - Replace `YOUR_OWNER_TEMPLATE_ID` with `order_to_owner`

4. Find the line with: `emailjs.send("YOUR_SERVICE_ID", "YOUR_CUSTOMER_TEMPLATE_ID", customerEmailParams)`
   - Replace `YOUR_SERVICE_ID` with your Service ID (same as above)
   - Replace `YOUR_CUSTOMER_TEMPLATE_ID` with `order_confirmation`

5. Find the line: `to_email: "OWNER_EMAIL@example.com",`
   - Replace `OWNER_EMAIL@example.com` with your actual email where you want to receive orders

### Example (After Setup):
```javascript
emailjs.init("3pK8x7nQ9vR2mL5bJ8wH");  // Your public key

// Later in the code:
emailjs.send("service_abc123", "order_to_owner", ownerEmailParams)
emailjs.send("service_abc123", "order_confirmation", customerEmailParams)
```

---

## What Happens When a Customer Orders:

1. ✅ Customer fills in shipping address
2. ✅ Customer completes payment with Razorpay
3. 📧 **Owner receives email** with:
   - Customer name, phone, email
   - **Full delivery address** (address, city, PIN code)
   - Order items and quantity
   - Total amount and payment ID

4. 📧 **Customer receives confirmation email** with:
   - Order ID
   - Delivery address confirmation
   - Items ordered
   - Total amount

---

## Testing

After setup, place a test order:
1. Go to your website
2. Add products to cart
3. Go to checkout
4. Fill in test address
5. Complete payment (use Razorpay test card if needed)
6. Check your email inbox for the order notification

---

## Troubleshooting

**Emails not sending?**
- Check EmailJS console for errors (F12 → Console tab)
- Verify all three credentials are correctly replaced in the code
- Ensure your email service is connected in EmailJS
- Check spam/promotions folder

**Need Help?**
- EmailJS Docs: https://www.emailjs.com/docs/
- Contact EmailJS Support: support@emailjs.com

---

## Free Plan Limits
- EmailJS Free Plan: 200 emails/month
- Perfect for small businesses starting out
- Upgrade anytime if you need more

