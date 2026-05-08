# 🚀 Setup Guide — Agro Market

> Get the project running on your local machine in under 5 minutes.

---

## Prerequisites

| Tool | Minimum Version | Download |
|------|----------------|----------|
| Node.js | 18.x | https://nodejs.org |
| npm | 9.x | Included with Node.js |
| Git | 2.x | https://git-scm.com |

Verify your environment:

```bash
node -v   # v18.x.x or higher
npm -v    # 9.x.x or higher
```

---

## 1. Clone & Install

```bash
cd agro-market
npm install
```

> **Tip:** If you encounter peer dependency warnings, run `npm install --legacy-peer-deps`.

---

## 2. Environment Configuration

Create a `.env` file in the project root:

```bash
cp .env.example .env   # If .env.example exists, otherwise create manually
```

### Required Variables

```env
# Firebase Configuration (Required for full functionality)
REACT_APP_FIREBASE_API_KEY=your_api_key
REACT_APP_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
REACT_APP_FIREBASE_PROJECT_ID=your_project_id
REACT_APP_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
REACT_APP_FIREBASE_APP_ID=your_app_id
REACT_APP_FIREBASE_MEASUREMENT_ID=your_measurement_id

# Optional: Firebase Cloud Messaging VAPID Key
REACT_APP_FIREBASE_VAPID_KEY=your_vapid_key

# Stripe (Optional — needed for card payments)
REACT_APP_STRIPE_PUBLIC_KEY=pk_test_your_key
```

### Demo Mode (No Firebase Required)

If you skip the Firebase config, the app automatically runs in **Demo Mode** with mock data:
- Pre-loaded demo user (`demo@example.com`)
- Simulated products, orders, and chat
- All UI features work without backend

> Look for the `[DEMO MODE]` console warning to confirm.

---

## 3. Start Development Server

```bash
npm start
```

The app will open automatically at **`http://localhost:3000`**.

### Common Issues

| Issue | Solution |
|-------|----------|
| Port 3000 in use | The CLI will prompt for another port (usually 3001) |
| `react-scripts` not found | Re-run `npm install` |
| Firebase permission errors | Check `.env` values or switch to Demo Mode |
| Module not found | Delete `node_modules` and `package-lock.json`, then `npm install` |

---

## 4. Firebase Setup (Production)

### 4.1 Create Project
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Click **Add Project** → name it `agro-market` (or your preferred name)
3. Enable Google Analytics (optional)

### 4.2 Enable Services

| Service | How to Enable |
|---------|---------------|
| **Authentication** | Build → Authentication → Sign-in method → Enable **Email/Password** |
| **Firestore** | Build → Firestore Database → Create database → Start in test mode |
| **Storage** | Build → Storage → Get started |
| **Cloud Messaging** | Build → Cloud Messaging → Enable |

### 4.3 Get Config Values
1. Project Settings → General → Your apps → Web app
2. Register app → Copy the `firebaseConfig` object
3. Map each value to the `.env` variables above

### 4.4 Deploy Security Rules

```bash
npm install -g firebase-tools
firebase login
firebase deploy --only firestore:rules
```

The rules file is located at `firestore.rules`.

### 4.5 Create Firestore Indexes

In Firebase Console → Firestore Database → Indexes, create these composite indexes:

| Collection | Fields | Query Scope |
|------------|--------|-------------|
| `orders` | `buyerId` (Ascending) + `createdAt` (Descending) | Collection |
| `orders` | `sellerId` (Ascending) + `createdAt` (Descending) | Collection |
| `products` | `category` (Ascending) + `createdAt` (Descending) | Collection |
| `messages` | `participants` (Array contains) + `timestamp` (Ascending) | Collection |

---

## 5. Available Scripts

```bash
npm start       # Start development server (hot reload)
npm run build   # Create optimized production build
npm test        # Run test suite in interactive mode
npm run eject   # Eject from react-scripts (⚠️ irreversible)
```

---

## 6. Project Structure Overview

```
agro-market/
├── public/                 # Static assets, PWA manifest
├── src/
│   ├── firebase/
│   │   └── config.js       # Firebase init + demo mode logic
│   ├── context/
│   │   ├── AuthContext.jsx # Auth state, role management
│   │   ├── ThemeContext.jsx# Dark/light/system theme
│   │   └── NotificationContext.jsx # Toast notifications
│   ├── hooks/              # Custom React hooks
│   ├── components/         # Reusable UI components
│   ├── screens/            # Page-level components
│   ├── utils/              # Validation & formatting helpers
│   ├── App.js              # Router configuration
│   ├── index.js            # Entry point
│   └── index.css           # Tailwind directives + global styles
├── firestore.rules         # Firestore security rules
├── tailwind.config.js      # Tailwind theme customization
└── package.json            # Dependencies & scripts
```

---

## 7. Tech Stack Summary

| Layer | Technology |
|-------|-----------|
| Framework | React 19 |
| Routing | React Router v6 |
| Styling | Tailwind CSS 3.4 |
| Animations | Framer Motion |
| Backend | Firebase (Auth, Firestore, Storage, FCM) |
| Payments | Stripe, PayPal, M-Pesa, Orange Money, COD |
| PDF | jsPDF + html2canvas |
| Validation | React Hook Form + Zod |
| Icons | Lucide React |

---

## Next Steps

- See **[USAGE.md](./USAGE.md)** for a walkthrough of app features.
- See **[README.md](./README.md)** for architecture and data model details.
- Check **[TODO.md](./TODO.md)** for upcoming enhancements.

---

**Happy coding! 🌾**

