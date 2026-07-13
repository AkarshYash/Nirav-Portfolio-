# 🚀 Deployment Guide

Complete guide to deploy your portfolio website to various free hosting platforms.

## 📋 Pre-Deployment Checklist

- [ ] Update personal information in `index.html`
- [ ] Replace placeholder email with your actual email
- [ ] Update social media links (LinkedIn, GitHub, Twitter)
- [ ] Add your profile image (optional)
- [ ] Test locally in multiple browsers
- [ ] Check mobile responsiveness
- [ ] Verify all links work correctly
- [ ] Run accessibility checks

## 🌐 Deployment Options

### Option 1: GitHub Pages (Recommended for Beginners)

**Advantages:**
- 100% Free
- Custom domain support
- Automatic HTTPS
- Direct integration with GitHub

**Steps:**

1. **Create GitHub Account** (if you don't have one)
   - Go to [github.com](https://github.com)
   - Sign up for free

2. **Create a New Repository**
   ```bash
   # Repository name MUST be: yourusername.github.io
   # Example: niravpatel.github.io
   ```

3. **Upload Your Files**
   ```bash
   # Initialize git in your portfolio folder
   git init
   
   # Add all files
   git add .
   
   # Commit changes
   git commit -m "Initial portfolio deployment"
   
   # Add remote origin (replace with your username)
   git remote add origin https://github.com/yourusername/yourusername.github.io.git
   
   # Push to GitHub
   git push -u origin main
   ```

4. **Enable GitHub Pages**
   - Go to repository Settings
   - Scroll to "Pages" section
   - Source: Deploy from main branch
   - Save

5. **Access Your Site**
   - Your site will be live at: `https://yourusername.github.io`
   - May take 5-10 minutes for first deployment

**Custom Domain (Optional):**
```bash
# Add CNAME file with your domain
echo "www.yourdomain.com" > CNAME
git add CNAME
git commit -m "Add custom domain"
git push
```

---

### Option 2: Netlify (Easiest - Drag & Drop)

**Advantages:**
- Drag and drop deployment
- Automatic SSL
- Free custom domain
- Instant previews
- Form handling built-in

**Method A: Drag & Drop (No Git Required)**

1. Go to [netlify.com](https://netlify.com)
2. Sign up for free account
3. Click "Add new site" → "Deploy manually"
4. Drag your entire portfolio folder
5. Done! Your site is live instantly

**Method B: Git Integration**

1. Push your code to GitHub (see Option 1 steps 1-3)
2. Go to [netlify.com](https://netlify.com)
3. Click "Add new site" → "Import from Git"
4. Connect your GitHub account
5. Select your portfolio repository
6. Deploy settings:
   - Build command: (leave empty)
   - Publish directory: `.` (or leave empty)
7. Click "Deploy"

**Custom Domain:**
- Go to Site settings → Domain management
- Add custom domain (free `.netlify.app` or your own)

**Environment Variables (if needed):**
```bash
# Netlify Dashboard → Site settings → Environment variables
KEY=VALUE
```

---

### Option 3: Vercel (Best for Performance)

**Advantages:**
- Lightning-fast CDN
- Automatic deployments
- Preview URLs for every commit
- Analytics included

**Steps:**

1. **Install Vercel CLI** (optional)
   ```bash
   npm install -g vercel
   ```

2. **Deploy via CLI**
   ```bash
   # Navigate to your portfolio folder
   cd portfolio
   
   # Login to Vercel
   vercel login
   
   # Deploy
   vercel --prod
   
   # Follow prompts:
   # - Setup and deploy? Y
   # - Which scope? (your account)
   # - Link to existing project? N
   # - Project name? portfolio
   # - Directory? ./
   ```

3. **Deploy via Dashboard** (No CLI)
   - Go to [vercel.com](https://vercel.com)
   - Sign up with GitHub
   - Click "Add New" → "Project"
   - Import your repository
   - Deploy

**Configuration:**
- Framework Preset: Other
- Build Command: (leave empty)
- Output Directory: (leave empty)

---

### Option 4: Cloudflare Pages (Best Security)

**Advantages:**
- Global CDN
- Unlimited bandwidth
- DDoS protection
- Best security features

**Steps:**

1. Go to [pages.cloudflare.com](https://pages.cloudflare.com)
2. Sign up for free account
3. Click "Create a project"
4. Connect your Git provider (GitHub)
5. Select repository
6. Configure build:
   - Build command: (leave empty)
   - Build output: `.`
7. Deploy

---

### Option 5: AWS S3 + CloudFront (Advanced)

**Advantages:**
- Ultra-scalable
- Industry-standard
- Full control

**Steps:**

1. **Create S3 Bucket**
   ```bash
   # AWS Console → S3 → Create bucket
   # Bucket name: portfolio.yourdomain.com
   # Enable static website hosting
   ```

2. **Upload Files**
   ```bash
   aws s3 sync . s3://your-bucket-name --delete
   ```

3. **Configure CloudFront**
   - Create distribution
   - Origin: Your S3 bucket
   - Enable HTTPS

4. **Update DNS**
   - Point your domain to CloudFront distribution

---

## 🔧 Post-Deployment Tasks

### 1. Configure Custom Domain

**For GitHub Pages:**
```bash
# Add CNAME file
echo "www.yourdomain.com" > CNAME
git add CNAME
git commit -m "Add custom domain"
git push
```

**DNS Settings:**
```
Type: CNAME
Name: www
Value: yourusername.github.io
```

### 2. Enable HTTPS

All platforms provide automatic HTTPS. For custom domains:
- Wait for SSL certificate provisioning (5-30 minutes)
- Verify HTTPS is working
- Force HTTPS redirect (usually automatic)

### 3. Add Analytics

**Google Analytics:**
```html
<!-- Add before </head> in index.html -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

**Vercel Analytics:**
```bash
# Automatically available in Vercel dashboard
```

### 4. SEO Optimization

Update meta tags in `index.html`:
```html
<meta name="description" content="Your description here">
<meta property="og:title" content="Your Name - Portfolio">
<meta property="og:description" content="Your description">
<meta property="og:image" content="https://yoursite.com/preview.jpg">
<meta property="og:url" content="https://yoursite.com">
<meta name="twitter:card" content="summary_large_image">
```

### 5. Create Sitemap

Create `sitemap.xml`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://yoursite.com/</loc>
    <lastmod>2026-01-13</lastmod>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

---

## 🔄 Continuous Deployment

### GitHub Actions (Auto-deploy on push)

Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./
```

---

## 📊 Performance Monitoring

### Lighthouse CI

```bash
# Install Lighthouse
npm install -g @lhci/cli

# Run audit
lhci autorun --upload.target=temporary-public-storage
```

### Key Metrics to Monitor

- **Performance**: > 90
- **Accessibility**: > 95
- **Best Practices**: > 95
- **SEO**: > 90

---

## 🐛 Troubleshooting

### Issue: Site not loading

**Solutions:**
1. Check DNS propagation (can take 24-48 hours)
2. Verify files are uploaded correctly
3. Check browser console for errors
4. Clear browser cache

### Issue: HTTPS not working

**Solutions:**
1. Wait for SSL certificate provisioning (up to 24 hours)
2. Verify DNS settings are correct
3. Check hosting provider's SSL status

### Issue: Custom domain not working

**Solutions:**
1. Verify CNAME file exists (GitHub Pages)
2. Check DNS configuration
3. Wait for DNS propagation
4. Try accessing via HTTPS

### Issue: Images not loading

**Solutions:**
1. Check image paths are correct (relative paths)
2. Verify images are uploaded
3. Check file extensions (case-sensitive on Linux servers)

---

## 📞 Support

If you encounter issues:

1. **GitHub Pages**: [GitHub Pages Documentation](https://docs.github.com/pages)
2. **Netlify**: [Netlify Support Forum](https://answers.netlify.com)
3. **Vercel**: [Vercel Documentation](https://vercel.com/docs)
4. **Cloudflare**: [Cloudflare Support](https://support.cloudflare.com)

---

## ✅ Deployment Success Checklist

- [ ] Site is live and accessible
- [ ] HTTPS is working
- [ ] All links work correctly
- [ ] Images load properly
- [ ] Contact form works (if applicable)
- [ ] Mobile responsive
- [ ] Cross-browser compatible
- [ ] SEO meta tags added
- [ ] Analytics configured
- [ ] Custom domain connected (optional)
- [ ] Site speed is optimized (Lighthouse score > 90)

---

**🎉 Congratulations! Your portfolio is now live!**

Share your portfolio URL:
- LinkedIn
- Twitter
- GitHub profile
- Resume
- Email signature

---

*Need help? Open an issue on GitHub or reach out via email.*
