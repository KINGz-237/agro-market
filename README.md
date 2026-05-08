# 🌾 Agro Market

A professional full-stack mobile/web marketplace app connecting farmers, sellers, wholesale buyers, and retail buyers. Built with React 19, Tailwind CSS, Framer Motion, and Firebase.

> **New here?** Start with the **[Setup Guide](./SETUP.md)** to run the project locally, then check the **[Usage Guide](./USAGE.md)** for a feature walkthrough.

---

## 🚀 Quick Start

```bash
cd agro-market
npm install
npm start
```

The app runs at `http://localhost:3000`. No Firebase config required — it works in **Demo Mode** out of the box!

---

## ✨ Features

### Multi-Role System
- **Role Registration:** Users register as Seller, Buyer, or Both
- **Role Switcher Toggle:** Instantly switch between seller and buyer modes
- **Context-Aware UI:** Dashboard, navigation, and actions adapt to active role

### Seller Features
- 📦 Post products with title, description, price, quantity, available date, category, and up to 5 images
- 📋 View incoming orders with Pending/Approved/Rejected status
- ✅ Approve or reject orders with slide-to-confirm actions
- 💰 Receive payment notifications
- 📄 Send digital approval receipts (PDF)
- 💬 In-app chat with buyers

### Buyer Features
- 🔍 Browse products with category filters and search
- 🖼️ Horizontal image carousels with Swiper
- 📜 Infinite vertical scroll for product feed
- 🛒 Place orders with quantity, delivery address, and preferred date
- 💳 Multiple payment options: Stripe, PayPal, M-Pesa, Orange Money, Cash on Delivery
- 📄 Download and share PDF receipts
- 💬 In-app chat with sellers
- ⭐ Rate and review sellers

### Universal UI/UX
- 🌗 Dark/Light mode toggle with system preference detection
- 📱 Responsive design: mobile-first with tablet/desktop layouts
- 🎬 Animated page transitions with Framer Motion
- ↔️ Swipeable tabs (Pending/Approved/Completed)
- ➡️ Slide-to-confirm action buttons
- 🔔 Real-time push notifications
- 🔄 Pull-to-refresh
- 🧭 Bottom navigation bar + Floating Action Button
- 🎯 Toast notification system

## 🏗️ Architecture

### Tech Stack
- **Frontend:** React 19, React Router v6, Tailwind CSS, Framer Motion
- **Backend:** Firebase (Auth, Firestore, Storage, Cloud Messaging)
- **Payments:** Stripe, PayPal, M-Pesa, Orange Money, Cash on Delivery
- **PDF Generation:** jsPDF + html2canvas
- **Image Carousel:** Swiper
- **Validation:** React Hook Form + Zod

### Data Models

#### User
```javascript
{
  uid: string,
  name: string,
  email: string,
  phone: string,
  role: ['seller' | 'buyer'],
  profileImage: string,
  createdAt: string,
  rating: { average: number, count: number }
}
```

#### Product
```javascript
{
  id: string,
  sellerId: string,
  title: string,
  description: string,
  price: number,
  quantity: number,
  availableDate: string,
  category: string,
  images: string[],
  createdAt: string
}
```

#### Order
```javascript
{
  id: string,
  productId: string,
  buyerId: string,
  sellerId: string,
  quantity: number,
  totalPrice: number,
  deliveryAddress: string,
  preferredDate: string,
  status: 'pending' | 'approved' | 'rejected' | 'delivered' | 'completed',
  paymentMethod: string,
  paidAt: string | null,
  createdAt: string
}
```

#### Receipt
```javascript
{
  id: string,
  orderId: string,
  sellerId: string,
  buyerId: string,
  timestamp: string,
  receiptNumber: string,
  digitalSignature: string
}
```

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- Firebase project
- Stripe account (optional, for payments)

### Installation

1. **Clone and navigate to project:**
```bash
cd agro-market
```

2. **Install dependencies:**
```bash
npm install
```

3. **Configure environment variables:**
Create `.env` file with your Firebase config:
```env
REACT_APP_FIREBASE_API_KEY=your_api_key
REACT_APP_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
REACT_APP_FIREBASE_PROJECT_ID=your_project_id
REACT_APP_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
REACT_APP_FIREBASE_APP_ID=your_app_id
REACT_APP_FIREBASE_MEASUREMENT_ID=your_measurement_id

REACT_APP_STRIPE_PUBLIC_KEY=pk_test_your_key
```

4. **Start development server:**
```bash
npm start
```

The app will open at `http://localhost:3000`.

### Firebase Setup

1. Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable Authentication (Email/Password)
3. Create Cloud Firestore database
4. Enable Firebase Storage
5. Deploy Firestore rules:
```bash
firebase deploy --only firestore:rules
```

### Firestore Indexes

Create composite indexes for:
- `orders`: `buyerId` + `createdAt` (descending)
- `orders`: `sellerId` + `createdAt` (descending)
- `products`: `category` + `createdAt` (descending)
- `messages`: `participants` + `timestamp` (ascending)

## 📁 Project Structure

```
src/
├── firebase/
│   └── config.js          # Firebase initialization
├── context/
│   ├── AuthContext.jsx    # Authentication & role management
│   ├── ThemeContext.jsx   # Dark/light mode
│   └── NotificationContext.jsx  # Toast notifications
├── hooks/
│   ├── useFirestore.js    # Real-time Firestore hooks
│   └── useInfiniteScroll.js     # Infinite scroll hook
├── components/
│   ├── AppBar.jsx         # Top app bar
│   ├── BottomNav.jsx      # Bottom navigation
│   ├── ProductCard.jsx    # Product card component
│   ├── OrderCard.jsx      # Order card component
│   ├── ImageCarousel.jsx  # Swiper image carousel
│   ├── SwipeableTabs.jsx  # Swipeable tabs
│   ├── SlideToConfirm.jsx # Slide-to-confirm button
│   ├── ChatBubble.jsx     # Chat message bubble
│   ├── RatingStars.jsx    # Rating component
│   ├── PullToRefresh.jsx  # Pull-to-refresh wrapper
│   ├── LoadingOverlay.jsx # Loading overlay
│   └── FloatingActionButton.jsx # FAB
├── screens/
│   ├── AuthScreen.jsx     # Login/Register
│   ├── DashboardScreen.jsx # Main dashboard
│   ├── BuyerPanelScreen.jsx # Product browsing
│   ├── SellerPanelScreen.jsx # Seller dashboard
│   ├── ProductDetailScreen.jsx # Product details
│   ├── ProductFormScreen.jsx # Add/Edit product
│   ├── OrderListScreen.jsx # Orders with tabs
│   ├── PaymentScreen.jsx  # Checkout & payment
│   ├── ReceiptScreen.jsx  # Digital receipt
│   ├── ChatScreen.jsx     # In-app chat
│   └── ProfileScreen.jsx  # User profile
├── utils/
│   ├── validators.js      # Zod validation schemas
│   └── formatters.js      # Formatting utilities
├── App.js                 # Router & layout
├── index.js               # Entry point
└── index.css              # Tailwind directives

firestore.rules            # Security rules
public/
└── manifest.json          # PWA manifest
```

## 🔒 Security

### Firestore Rules
- Users can only read/write their own profile
- Products are public read, sellers can only edit their own
- Orders: buyers/sellers can only see their own orders
- Messages: participants only
- Receipts: order participants only

### Validations
- Stock validation: cannot order more than available
- Date validation: cannot select past dates
- Receipt locked until payment confirmed
- Image limit: maximum 5 images per product

## 🎨 Design System

### Colors
- Primary: Agro green palette (`#16a34a` to `#14532d`)
- Dark mode: Full dark mode support with `class` strategy
- Semantic colors for statuses (pending, approved, rejected)

### Responsive Breakpoints
- Mobile: < 768px (bottom nav, full-width cards)
- Tablet: 768px - 1024px (grid layouts, side nav)
- Desktop: > 1024px (max-width containers, hover states)

## 🧪 Testing

Run tests:
```bash
npm test
```

## 📦 Building for Production

```bash
npm run build
```

Creates optimized production build in `build/` folder.

## 📝 License

MIT License - feel free to use for commercial or personal projects.

---

Built with ❤️ for the agricultural community.
