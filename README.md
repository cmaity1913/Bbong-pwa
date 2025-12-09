# Bbong PWA Setup Guide

## 🎯 উদ্দেশ্য
এই guide follow করলে আপনার Blogger site এ PWA install popup আসবে এবং users সরাসরি app এর মতো install করতে পারবে।

---

## 📁 Step 1: Icons তৈরি করুন

নিচের size গুলোতে আপনার logo/icon তৈরি করুন (PNG format):

```
icons/
├── icon-72.png   (72x72 px)
├── icon-96.png   (96x96 px)
├── icon-128.png  (128x128 px)
├── icon-144.png  (144x144 px)
├── icon-152.png  (152x152 px)
├── icon-192.png  (192x192 px)
├── icon-384.png  (384x384 px)
└── icon-512.png  (512x512 px)
```

**Icon তৈরি করার সহজ উপায়:**
1. https://www.canva.com এ যান
2. 512x512 custom size এ একটা simple icon তৈরি করুন
3. এরপর https://realfavicongenerator.net এ upload করে সব size generate করুন

---

## 📤 Step 2: GitHub Repository তৈরি করুন

1. **GitHub এ যান:** https://github.com/new

2. **নতুন Repository তৈরি করুন:**
   - Repository name: `bbong-pwa` (অথবা যেকোনো নাম)
   - Public রাখুন (Private হলে GitHub Pages কাজ করবে না)
   - "Add a README file" check করুন
   - "Create repository" click করুন

3. **Files Upload করুন:**
   - "Add file" → "Upload files" click করুন
   - এই folder থেকে upload করুন:
     - `manifest.json`
     - `sw.js`
     - `icons/` folder (সব icons সহ)
   - "Commit changes" click করুন

---

## 🌐 Step 3: GitHub Pages Enable করুন

1. Repository র Settings এ যান
2. বাম দিকে "Pages" click করুন
3. "Source" এ "Deploy from a branch" select করুন
4. "Branch" এ `main` select করুন এবং `/ (root)` রাখুন
5. "Save" click করুন

**কিছুক্ষণ wait করুন (1-2 minute)**

6. Page refresh করলে উপরে একটা URL দেখাবে:
   ```
   https://YOUR-USERNAME.github.io/bbong-pwa/
   ```

---

## ✏️ Step 4: manifest.json Update করুন

GitHub Pages URL পাওয়ার পর, `manifest.json` এ icons এর URL update করুন।

আপনার GitHub Pages URL যদি হয়:
`https://your-username.github.io/bbong-pwa/`

তাহলে manifest.json এ সব icon src এভাবে হবে:
```json
"src": "https://your-username.github.io/bbong-pwa/icons/icon-192.png"
```

**অথবা relative URL রাখুন (recommended):**
যেহেতু manifest.json ও icons একই folder এ, তাই `icons/icon-192.png` রাখতে পারেন।

---

## 📝 Step 5: Blogger Theme এ Code যোগ করুন

**গুরুত্বপূর্ণ:** নিচের code আপনার `ecommerce_theme.xml` এর `<head>` section এ যোগ করুন:

```html
<!-- PWA Manifest -->
<link rel="manifest" href="https://YOUR-USERNAME.github.io/bbong-pwa/manifest.json" crossorigin="use-credentials"/>

<!-- Theme Colors for PWA -->
<meta name="theme-color" content="#6366f1"/>
<meta name="apple-mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>
<meta name="apple-mobile-web-app-title" content="Bbong"/>

<!-- Apple Touch Icons -->
<link rel="apple-touch-icon" href="https://YOUR-USERNAME.github.io/bbong-pwa/icons/icon-152.png"/>
<link rel="apple-touch-icon" sizes="180x180" href="https://YOUR-USERNAME.github.io/bbong-pwa/icons/icon-192.png"/>
```

**এবং `</body>` এর আগে Service Worker registration যোগ করুন:**

```html
<script>
// Register Service Worker
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('https://YOUR-USERNAME.github.io/bbong-pwa/sw.js', {
      scope: '/'
    })
    .then((registration) => {
      console.log('SW registered:', registration.scope);
    })
    .catch((error) => {
      console.log('SW registration failed:', error);
    });
  });
}
</script>
```

---

## ✅ Step 6: Test করুন

1. **Chrome DevTools এ check করুন:**
   - F12 press করুন
   - "Application" tab এ যান
   - "Manifest" দেখুন - সব ঠিক আছে কিনা
   - "Service Workers" দেখুন - registered কিনা

2. **Install Prompt দেখুন:**
   - Chrome এ address bar এ একটা install icon দেখা যাবে
   - অথবা 3-dot menu তে "Install Bbong" option আসবে

3. **Mobile এ test করুন:**
   - Android Chrome: Menu → "Add to Home screen" বা "Install app"
   - iOS Safari: Share → "Add to Home Screen"

---

## 🔧 Troubleshooting

**Install popup আসছে না?**
- HTTPS আছে কিনা check করুন (Blogger default এ HTTPS)
- manifest.json fetch হচ্ছে কিনা Network tab এ দেখুন
- Service Worker register হচ্ছে কিনা Console এ দেখুন
- Icon sizes সঠিক কিনা check করুন (minimum 192x192 দরকার)

**"Site cannot be installed" error?**
- manifest.json এ start_url সঠিক কিনা দেখুন
- Icons accessible কিনা browser এ directly open করে দেখুন

---

## 📱 কিভাবে Install হবে?

**Android (Chrome/Edge):**
1. Site visit করলে নিচে "Add to Home Screen" banner আসবে
2. অথবা Menu (⋮) → "Install app" / "Add to Home screen"
3. "Install" tap করলেই app install হবে!

**iOS (Safari):**
1. Share button (📤) tap করুন
2. "Add to Home Screen" tap করুন
3. "Add" tap করুন

**Desktop (Chrome/Edge):**
1. Address bar এ install (⊕) icon click করুন
2. "Install" click করুন

---

## 🎉 Done!

এখন আপনার site একটা real app এর মতো install হবে - home screen এ icon থাকবে, 
full screen এ open হবে, এবং native app এর মতো feel হবে!
