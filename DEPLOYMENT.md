# Deployment Instructions

## What You Have

A single HTML file (`ai-assessments.html`) containing:
- AI Readiness Assessment (fully functional)
- AI Controls Framework Assessment (placeholder - needs full code integration)
- Footer with attribution and contact links
- Email capture component (placeholder API endpoint)
- Basic routing between assessments

## What You Need to Customize BEFORE Deploying

### 1. Update Footer Links (Line ~90-100)

Replace these placeholders:
```html
<!-- FIND THIS: -->
<a href="https://linkedin.com/in/your-profile" target="_blank">LinkedIn</a>
<a href="mailto:your-email@example.com">Email</a>

<!-- REPLACE WITH: -->
<a href="https://linkedin.com/in/YOUR-ACTUAL-PROFILE" target="_blank">LinkedIn</a>
<a href="mailto:YOUR-ACTUAL-EMAIL@example.com">Email</a>
```

Also update:
```html
Built by <strong>Bill [Your Last Name], CPA/CISA</strong>
```

### 2. Add Your API Endpoint (Line ~130)

The email capture currently points to a placeholder:
```javascript
await fetch('/api/submit-assessment', {
```

You'll need to:
- Deploy the serverless function (see below)
- Update this URL to your actual endpoint
- Or use a service like Formspree for quick setup

### 3. Integrate Full Controls Framework

Currently the Controls Framework shows a placeholder. To add the full version:
1. Open `ai-controls-framework.jsx` from your artifacts
2. Copy the entire component code
3. Replace the `AIControlsFramework` function (starts around line ~800)

## Deployment Options

### Option 1: Quick Deploy to Vercel (Recommended)

**Step 1: Install Vercel CLI**
```bash
npm install -g vercel
```

**Step 2: Create Project Directory**
```bash
mkdir ai-assessments
cd ai-assessments
mv /path/to/ai-assessments.html index.html
```

**Step 3: Deploy**
```bash
vercel
```

Follow prompts:
- Project name: `ai-assessments`
- Build settings: None (it's a static file)
- Deploy: Yes

You'll get a URL like: `ai-assessments.vercel.app`

**Step 4: Add Custom Domain (Optional)**
```bash
vercel domains add assessments.yourdomain.com
```

Then add the DNS records Vercel provides.

### Option 2: Deploy to Netlify

**Step 1: Create Project Directory**
```bash
mkdir ai-assessments
cd ai-assessments
mv /path/to/ai-assessments.html index.html
```

**Step 2: Install Netlify CLI**
```bash
npm install -g netlify-cli
```

**Step 3: Deploy**
```bash
netlify deploy
```

For production:
```bash
netlify deploy --prod
```

### Option 3: GitHub Pages

**Step 1: Create GitHub Repo**
```bash
git init
git add index.html
git commit -m "Initial commit"
git remote add origin https://github.com/YOUR-USERNAME/ai-assessments.git
git push -u origin main
```

**Step 2: Enable GitHub Pages**
- Go to repo Settings
- Pages section
- Source: main branch
- Save

URL will be: `YOUR-USERNAME.github.io/ai-assessments`

## Adding the Backend (Phase 2)

When you're ready to capture emails and send notifications:

### Option A: Formspree (No Code Required)

1. Sign up at formspree.io
2. Create a form
3. Get your endpoint: `https://formspree.io/f/YOUR-FORM-ID`
4. Update the fetch URL in the HTML file

### Option B: Vercel Serverless Function

**File: `api/submit-assessment.js`**
```javascript
export default async function handler(req, res) {
  // Enable CORS
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'POST, OPTIONS');
  
  if (req.method === 'OPTIONS') {
    return res.status(200).end();
  }
  
  const { assessmentType, email, scores, timestamp } = req.body;
  
  // TODO: Add Airtable integration here
  // TODO: Add email notification here
  
  res.status(200).json({ success: true });
}
```

Deploy with:
```bash
vercel
```

Your API will be at: `your-app.vercel.app/api/submit-assessment`

## File Structure for Production

```
ai-assessments/
├── index.html                 # Your main file (rename from ai-assessments.html)
├── .claud.md                  # Development instructions (optional, won't be deployed)
├── README.md                  # For GitHub visitors (optional)
└── api/                       # Only needed if using Vercel Functions
    └── submit-assessment.js   # Serverless function
```

## Testing Locally Before Deploy

**Option 1: Python Simple Server**
```bash
cd ai-assessments
python -m http.server 8000
```
Visit: `http://localhost:8000`

**Option 2: Node.js http-server**
```bash
npm install -g http-server
http-server
```
Visit: `http://localhost:8080`

## Post-Deployment Checklist

- [ ] All footer links work and point to correct profiles
- [ ] Email capture either disabled or connected to backend
- [ ] Both assessments load correctly
- [ ] Mobile responsive (test on phone)
- [ ] No console errors (open browser DevTools)
- [ ] Page load time under 3 seconds
- [ ] Navigation between assessments works

## Analytics Setup (Optional)

### Plausible Analytics (Privacy-Friendly)

1. Sign up at plausible.io
2. Add your domain
3. Copy the script tag
4. Add to `<head>` section:

```html
<script defer data-domain="yourdomain.com" src="https://plausible.io/js/script.js"></script>
```

### Google Analytics (If You Prefer)

Add before `</head>`:
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

## Troubleshooting

**Problem: Page shows but styles are broken**
- Check browser console for errors
- Verify CDN links are loading (React, Babel)
- Try hard refresh: Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows)

**Problem: Email capture doesn't work**
- Expected until you add backend
- Either implement API or remove the component for now

**Problem: Controls assessment is just placeholder**
- Expected - you need to integrate the full component
- For now, you can remove the navigation button

**Problem: Mobile view looks wrong**
- Check viewport meta tag is present
- Test responsive breakpoints
- Verify flexbox/grid on smaller screens

## Next Steps

1. Deploy to Vercel/Netlify (5 minutes)
2. Test on your phone
3. Share URL with 1-2 trusted colleagues for feedback
4. Add backend if email capture shows demand
5. Integrate full Controls Framework if Readiness gets traction

## Need Help?

Common issues:
- **"Page won't load"** → Check browser console, verify all CDN links
- **"Can't deploy"** → Verify you're in the right directory, file is named `index.html`
- **"404 error"** → Check deployment logs, verify build settings
