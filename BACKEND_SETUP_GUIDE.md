# Backend Alternative: Node.js Email Service Setup

If you prefer to have a backend server sending emails instead of EmailJS, follow this guide.

## Option 1: Simple Node.js Backend (Easy Setup)

### Prerequisites
- Node.js installed (https://nodejs.org/)
- Gmail account (or any email service)

### Step 1: Create Backend Folder
```
cd your-project-folder
mkdir backend
cd backend
```

### Step 2: Initialize Node Project
```
npm init -y
npm install express nodemailer cors dotenv
```

### Step 3: Create `.env` File
Create a file named `.env` in the backend folder:
```
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-app-password
OWNER_EMAIL=owner@example.com
PORT=3000
```

**For Gmail:**
1. Enable 2-Factor Authentication on your Gmail account
2. Go to https://myaccount.google.com/apppasswords
3. Select Mail and Windows (or your OS)
4. Copy the app password and paste it in `.env`

### Step 4: Create `server.js`
```javascript
const express = require('express');
const nodemailer = require('nodemailer');
const cors = require('cors');
require('dotenv').config();

const app = express();
app.use(cors());
app.use(express.json());

// Email transporter setup
const transporter = nodemailer.createTransport({
    service: 'gmail',
    auth: {
        user: process.env.EMAIL_USER,
        pass: process.env.EMAIL_PASSWORD
    }
});

// Send order email to owner
app.post('/api/send-order-email', async (req, res) => {
    const { orderId, name, email, phone, address, city, pincode, items, total, paymentId } = req.body;

    const itemsList = items.map(item => 
        `${item.name} (Qty: ${item.quantity}) - ₹${item.price * item.quantity}`
    ).join('\n');

    const ownerEmailHTML = `
        <h2>New Order Received!</h2>
        <hr>
        <h3>Order Details</h3>
        <p><strong>Order ID:</strong> ${orderId}</p>
        <p><strong>Date:</strong> ${new Date().toLocaleString()}</p>
        <p><strong>Payment ID:</strong> ${paymentId}</p>

        <h3>Customer Information</h3>
        <p><strong>Name:</strong> ${name}</p>
        <p><strong>Email:</strong> ${email}</p>
        <p><strong>Phone:</strong> ${phone}</p>

        <h3>📍 Delivery Address</h3>
        <p>
            ${address}<br>
            ${city} - ${pincode}
        </p>

        <h3>📦 Order Items</h3>
        <pre>${itemsList}</pre>

        <h3>💰 Total Amount: ₹${total}</h3>

        <hr>
        <p><em>Please prepare and ship this order accordingly.</em></p>
    `;

    const customerEmailHTML = `
        <h2>Thank You for Your Order!</h2>
        <p>Hi ${name},</p>
        <p>Your order has been successfully confirmed.</p>

        <h3>Order Details</h3>
        <p><strong>Order ID:</strong> ${orderId}</p>

        <h3>📍 Delivery Address</h3>
        <p>
            ${address}<br>
            ${city} - ${pincode}
        </p>

        <h3>📦 Items Ordered</h3>
        <pre>${itemsList}</pre>

        <h3>💰 Total Amount: ₹${total}</h3>

        <hr>
        <p>Your order will be shipped soon. We'll send you tracking details shortly.</p>
        <p>Thank you for supporting KNOT MUSEE!</p>
    `;

    try {
        // Send to owner
        await transporter.sendMail({
            from: process.env.EMAIL_USER,
            to: process.env.OWNER_EMAIL,
            subject: `New Order #${orderId} - ${name}`,
            html: ownerEmailHTML
        });

        // Send to customer
        await transporter.sendMail({
            from: process.env.EMAIL_USER,
            to: email,
            subject: `Your Order #${orderId} Confirmed - KNOT MUSEE`,
            html: customerEmailHTML
        });

        res.json({ success: true, message: 'Emails sent successfully' });
    } catch (error) {
        console.error('Error sending emails:', error);
        res.status(500).json({ success: false, error: error.message });
    }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`);
});
```

### Step 5: Start Server
```
node server.js
```

### Step 6: Update Frontend Code
Replace the payment handler in `checkout-details.html` with:

```javascript
handler: function(response) {
    const orders = getOrders();
    const orderId = 'KM' + Date.now();
    const orderData = {
        orderId,
        name,
        email,
        phone,
        address,
        city,
        pincode,
        items: cart,
        total,
        date: new Date().toLocaleString(),
        status: 'Confirmed',
        paymentId: response.razorpay_payment_id
    };
    orders.push(orderData);
    setOrders(orders);
    setCart([]);
    renderCheckoutCart();

    // Send order to backend
    fetch('http://localhost:3000/api/send-order-email', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({
            orderId,
            name,
            email,
            phone,
            address,
            city,
            pincode,
            items: cart,
            total,
            paymentId: response.razorpay_payment_id
        })
    })
    .then(response => response.json())
    .then(data => {
        console.log('Emails sent:', data);
    })
    .catch(error => {
        console.error('Error:', error);
    });

    showCheckoutMessage(`Payment successful! Order ID: ${orderId}. Confirmation sent to ${email}`, true);
}
```

---

## Summary

**EmailJS Method (Recommended for beginners):**
- ✅ No server setup needed
- ✅ Free up to 200 emails/month
- ✅ Takes 5 minutes to set up
- ❌ Depends on external service

**Node.js Backend Method:**
- ✅ Full control over emails
- ✅ No third-party dependencies
- ✅ Can scale easily
- ❌ Requires server hosting

Choose the method that works best for your needs!

