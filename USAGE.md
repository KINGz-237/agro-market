# 📖 Usage Guide — Agro Market

> A complete walkthrough of features for developers, testers, and end-users.

---

## Table of Contents

1. [First Launch](#first-launch)
2. [Authentication](#authentication)
3. [Dashboard & Role Switching](#dashboard--role-switching)
4. [Buyer Workflow](#buyer-workflow)
5. [Seller Workflow](#seller-workflow)
6. [Orders & Receipts](#orders--receipts)
7. [Chat](#chat)
8. [Settings](#settings)
9. [Keyboard Shortcuts & Tips](#keyboard-shortcuts--tips)

---

## First Launch

When you open the app for the first time:

1. **Demo Mode** — If no Firebase credentials are configured, the app loads with a demo account automatically.
2. **PWA Install Prompt** — A banner may appear (on supported browsers) to install the app to your home screen.
3. **Network Status Bar** — A colored bar at the top indicates online/offline status.

---

## Authentication

### Register
- Navigate to `/auth`
- Choose role(s): **Seller**, **Buyer**, or **Both**
- Fill name, email, phone, and password
- On success, you'll be redirected to the Dashboard

### Login
- Enter email and password
- Demo credentials (Demo Mode): `demo@example.com` / any password

### Logout
- Go to **Settings** → **Sign Out**
- Or use the logout action in the Profile screen

---

## Dashboard & Role Switching

The Dashboard (`/`) is your home screen. It adapts based on your active role.

### Role Switcher
If you registered as **Both**, a toggle lets you switch between:
- **Buyer Mode** — Browse products, place orders
- **Seller Mode** — Manage products, view incoming orders

Your active role persists across sessions in `localStorage`.

---

## Buyer Workflow

### Browse Products (`/browse`)
- **Infinite scroll** loads products as you scroll down
- **Category filters** narrow results
- **Search** (if implemented in your build)
- Tap any product to view details

### Product Detail (`/product/:id`)
- Horizontal **image carousel** (Swiper)
- Price, quantity available, seller info
- **Place Order** button

### Place Order
1. Enter desired **quantity**
2. Provide **delivery address**
3. Select **preferred delivery date**
4. Choose **payment method**:
   - Stripe (Card)
   - PayPal
   - M-Pesa
   - Orange Money
   - Cash on Delivery
5. Confirm → redirected to **Payment Screen**

### Payment (`/payment`)
- Complete payment based on selected method
- On success → **Receipt Screen** with PDF download option

---

## Seller Workflow

### Seller Panel (`/seller/products`)
- View all your posted products
- Tap **+** (FAB) to add a new product

### Add/Edit Product (`/product/new`)
- **Title** & **Description**
- **Price** per unit
- **Quantity** available
- **Available Date**
- **Category**
- **Images** — up to 5 photos (with compression)
- Save → product goes live immediately

### Manage Orders (`/orders`)
- **Swipeable tabs**: Pending | Approved | Completed
- Each order shows buyer info, quantity, total price
- **Slide to Confirm** actions:
  - Approve order
  - Reject order
- Approved orders generate a digital receipt

---

## Orders & Receipts

### Order List (`/orders`)
Accessible to both buyers and sellers, filtered by your role:
- **Buyers** see orders they placed
- **Sellers** see orders received

### Receipt (`/receipt/:id`)
- Generated after payment confirmation
- Shows order details, timestamp, digital signature
- **Download PDF** button (jsPDF + html2canvas)
- **Share** option (Web Share API where supported)

---

## Chat

### Start a Chat (`/chat/:userId`)
- Initiated from product detail or order card
- Real-time messaging (Firestore listeners)
- Chat bubbles with timestamps
- Unread indicators

---

## Settings (`/settings`)

The Settings screen is organized into sections:

| Section | Features |
|---------|----------|
| **App Updates** | Check for updates, auto-update toggle, version info |
| **Appearance** | Light / Dark / System theme, font size slider (80%–140%) |
| **Notifications** | Push, orders, chat, email, marketing toggles |
| **Privacy & Security** | Analytics, crash reports, privacy policy, data export |
| **Storage** | Cache size display, clear cache |
| **About** | App name, version, build number, licensed user |
| **Account** | Sign out, request account deletion |

### Theme Behavior
- **Light** — Forces light mode
- **Dark** — Forces dark mode
- **System** — Follows OS preference (`prefers-color-scheme`)

All preferences are persisted to `localStorage`.

---

## Keyboard Shortcuts & Tips

| Shortcut | Action |
|----------|--------|
| `Alt + ←` | Go back (same as browser back) |
| `Ctrl/Cmd + R` | Refresh page (pull-to-refresh on mobile) |

### Developer Tips
- Open browser DevTools → **Application** tab → **Local Storage** to inspect persisted settings
- Use **React DevTools** to inspect context values (`AuthContext`, `ThemeContext`)
- In Demo Mode, all Firebase calls are bypassed — great for UI testing

### Mobile-First Design
- The app is optimized for mobile viewport widths
- On desktop, content is centered with `max-w-3xl` containers
- Bottom navigation appears on all main screens
- Floating Action Button (FAB) provides quick actions

---

## Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| Blank screen after login | `logout` or `user` undefined | Ensure `useAuth()` is called in the component |
| Images not uploading | Firebase Storage not configured | Check `.env` or use Demo Mode |
| Payments failing | Stripe key missing | Add `REACT_APP_STRIPE_PUBLIC_KEY` |
| Notifications not showing | FCM not supported | Use Chrome/Edge on desktop/Android |
| Dark mode not toggling | `ThemeContext` missing | Wrap app in `<ThemeProvider>` |

---

## Feature Flags & Demo Mode

The app supports conditional features via environment and context:

```javascript
// Check if running in demo mode
import { isDemoMode } from './firebase/config';

if (isDemoMode) {
  // Use mock data
}
```

---

For technical architecture details, see **[README.md](./README.md)**.
For local setup instructions, see **[SETUP.md](./SETUP.md)**.

