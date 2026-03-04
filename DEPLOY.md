# 🚀 Deploying pcgconsultants.in on GoDaddy

Since the site is **fully static** (HTML, CSS, JS only), you have a few deployment options:

---

## Option A: GoDaddy Website Hosting (Recommended for simplicity)

### Step 1 — Purchase Hosting
- Log into [GoDaddy](https://www.godaddy.com) → **My Products**
- If you only bought the **domain**, you'll also need hosting. Purchase **Web Hosting** (Economy plan is fine for a static site) or use **GoDaddy's Static/cPanel hosting**.

### Step 2 — Access cPanel / File Manager
1. Go to **My Products** → Under your hosting plan, click **Manage**
2. Open **cPanel** → Click **File Manager**
3. Navigate to `public_html/`

### Step 3 — Upload Your Files
Upload these files into `public_html/`:
```
index.html
style.css
script.js
pcg_logo.jpg
```
> 💡 You can drag and drop or use the **Upload** button in File Manager.

### Step 4 — Point Domain to Hosting
- If domain and hosting are on the same GoDaddy account, this is usually **automatic**.
- If not, go to **DNS Management** for `pcgconsultants.in` and set the **A Record** to your hosting server's IP address (found in your hosting dashboard).

### Step 5 — Enable SSL (HTTPS)
1. In cPanel → **SSL/TLS** or **Let's Encrypt**
2. Install a free SSL certificate for `pcgconsultants.in`
3. Force HTTPS redirect (in `.htaccess`):
```apache
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

---

## Option B: Deploy FREE on GitHub Pages (Point GoDaddy domain)

This is **free** and easier to maintain.

### Step 1 — Push code to GitHub
```bash
cd /Users/kmahajan/cvent/pcg
git init
git add .
git commit -m "Initial PCG Consultants website"
git remote add origin https://github.com/YOUR_USERNAME/pcgconsultants.git
git push -u origin main
```

### Step 2 — Enable GitHub Pages
1. Go to your repo → **Settings** → **Pages**
2. Under **Source**, select `main` branch, root folder `/`
3. Click **Save**

### Step 3 — Add Custom Domain in GitHub
1. In the **Pages** settings, enter: `pcgconsultants.in`
2. Check **Enforce HTTPS**

### Step 4 — Configure DNS on GoDaddy
Go to **GoDaddy** → **DNS Management** for `pcgconsultants.in`:

| Type  | Name | Value                    | TTL    |
|-------|------|--------------------------|--------|
| A     | @    | 185.199.108.153          | 600    |
| A     | @    | 185.199.109.153          | 600    |
| A     | @    | 185.199.110.153          | 600    |
| A     | @    | 185.199.111.153          | 600    |
| CNAME | www  | YOUR_USERNAME.github.io  | 600    |

> Replace `YOUR_USERNAME` with your GitHub username.

### Step 5 — Wait for DNS Propagation
- Takes **10 minutes to 48 hours** (usually under 30 minutes)
- Verify: `dig pcgconsultants.in` or visit the URL

---

## Option C: Deploy on Netlify/Vercel (Also free, fastest)

### Netlify
1. Go to [netlify.com](https://netlify.com) → Sign up / Log in
2. Drag and drop the project folder, OR connect your GitHub repo
3. Set custom domain: `pcgconsultants.in`
4. On GoDaddy DNS, add a **CNAME** record pointing `www` → `your-site.netlify.app`
5. For apex domain (`pcgconsultants.in`), use Netlify DNS or set **A Record** to Netlify's load balancer IP

### Vercel
1. Go to [vercel.com](https://vercel.com) → Import your GitHub repo
2. Add custom domain in project settings
3. Follow Vercel's DNS instructions for GoDaddy

---

## 📝 Making the Contact Form Work

The current form shows a browser alert. To receive actual emails, use one of these free services:

### Option 1: Formspree (Easiest)
1. Sign up at [formspree.io](https://formspree.io)
2. Create a form and get your endpoint URL
3. Update the `<form>` tag in `index.html`:
```html
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST" class="contact-form">
```
4. Remove the JS form handler in `script.js` (the `contactForm.addEventListener` block)

### Option 2: EmailJS
1. Sign up at [emailjs.com](https://emailjs.com)
2. Configure a service + template
3. Use their JS SDK to send emails directly from the browser

---

## ✅ Pre-launch Checklist

- [ ] Update contact email, phone numbers, and address in `index.html`
- [ ] Update stat numbers (projects delivered, clients, experience years)
- [ ] Update LinkedIn and WhatsApp links
- [ ] Replace placeholder content with actual PCG details
- [ ] Test on mobile devices
- [ ] Set up SSL certificate (HTTPS)
- [ ] Set up contact form backend (Formspree/EmailJS)
- [ ] Verify DNS propagation
- [ ] Test all links and navigation

---

## 📁 File Structure
```
pcg/
├── index.html       ← Main website
├── style.css        ← All styles
├── script.js        ← Interactivity
├── pcg_logo.jpg     ← Your logo
└── DEPLOY.md        ← This file
```
