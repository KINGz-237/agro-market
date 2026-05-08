# Splash Screen Implementation Plan

**Information Gathered:**
- App name: "Agro Market - Farm to Table Marketplace" (manifest.json)
- Logo: public/logo512.png (512x512)
- App.js: Router + PageLayout + ProtectedRoute structure
- Uses React.createElement (no JSX? ), framer-motion available

**Plan:**
1. [x] Create SplashScreen.jsx component (done)
2. [x] Edit src/App.js:\n   - Import SplashScreen \n   - Add useState showSplash = true\n   - useEffect set false after 2.5s\n   - Conditional render: showSplash ? SplashScreen : RouterContent
3. Smooth transition with AnimatePresence

**Dependent Files:** src/App.js

**Followup:** Test npm start
