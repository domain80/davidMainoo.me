# Deployment Guide - davidMainoo.me

This guide covers everything you need to do to deploy your portfolio and blog website to production.

## Table of Contents

1. [Pre-Deployment Checklist](#pre-deployment-checklist)
2. [Choosing a Hosting Platform](#choosing-a-hosting-platform)
3. [Deployment Steps by Platform](#deployment-steps-by-platform)
4. [Custom Domain Setup](#custom-domain-setup)
5. [Post-Deployment Checklist](#post-deployment-checklist)
6. [Performance Optimization](#performance-optimization)
7. [Monitoring & Analytics](#monitoring--analytics)
8. [Troubleshooting](#troubleshooting)

---

## Pre-Deployment Checklist

### 1. Content Review

- [ ] **Update personal information**
  - [ ] Name in `src/components/layout/Sidebar.astro` (line 17)
  - [ ] Bio text in `src/components/layout/Sidebar.astro` (lines 22-25)
  - [ ] Social media links (GitHub, LinkedIn) in `src/components/layout/Sidebar.astro` (lines 119-136)
  - [ ] Resume link path in `src/components/layout/Sidebar.astro` (line 140)
  - [ ] Copyright year in `src/components/layout/Sidebar.astro` and `src/components/blog/BlogSidebar.astro`

- [ ] **Update meta information**
  - [ ] Site description in `src/layouts/BaseLayout.astro` (line 13)
  - [ ] Favicon at `/public/favicon.svg`
  - [ ] Open Graph image (create and add to `/public/`)

- [ ] **Content preparation**
  - [ ] Replace placeholder blog posts in `src/content/blog/`
  - [ ] Add your actual projects to `src/content/projects/`
  - [ ] Update resume PDF at `/public/resume.pdf`

### 2. Configuration Updates

- [ ] **Update site URLs**
  - [ ] Replace "Let's talk" link in `src/components/blog/BlogSidebar.astro` (line 32)
  - [ ] Replace "Let's talk" link in `src/components/layout/Sidebar.astro` (line 30)
  - [ ] Update actual social media URLs (currently placeholders)

- [ ] **RSS Feed configuration**
  - [ ] Update site URL in `src/pages/rss.xml.ts`
  - [ ] Update author information

### 3. Code Quality Checks

```bash
# Run type checking (if you add this)
pnpm astro check

# Build the project to catch any errors
pnpm build

# Test the production build locally
pnpm preview
```

- [ ] Build completes without errors
- [ ] Preview works correctly on http://localhost:4321
- [ ] All pages load correctly
- [ ] Navigation works between pages
- [ ] Blog filtering and pagination work
- [ ] Dark mode toggle works
- [ ] Theme persists across page navigation

---

## Choosing a Hosting Platform

Your Astro site is a static build, so it can be deployed to any static hosting platform. Here are the best options:

### Recommended: Vercel (Easiest)

**Pros:**
- ✅ Zero configuration needed
- ✅ Automatic deploys from Git
- ✅ Free SSL certificate
- ✅ Built-in analytics
- ✅ Edge network (fast worldwide)
- ✅ Preview deployments for PRs

**Pricing:** Free tier is generous (100GB bandwidth/month)

### Alternative: Netlify

**Pros:**
- ✅ Similar to Vercel
- ✅ Good free tier
- ✅ Great build logs

**Pricing:** Free tier (100GB bandwidth/month)

### Alternative: Cloudflare Pages

**Pros:**
- ✅ Unlimited bandwidth (free)
- ✅ Very fast edge network
- ✅ Excellent DDoS protection

**Pricing:** Completely free

### Alternative: GitHub Pages

**Pros:**
- ✅ Completely free
- ✅ Simple setup

**Cons:**
- ❌ Slower than alternatives
- ❌ No automatic rebuilds from CMS

---

## Deployment Steps by Platform

### Option 1: Vercel (Recommended)

#### Step 1: Push to GitHub

```bash
# Make sure all changes are committed
git status

# Push to GitHub
git push origin main
```

#### Step 2: Deploy on Vercel

1. Go to [vercel.com](https://vercel.com) and sign up/login with GitHub
2. Click "Add New Project"
3. Import your `davidMainoo.me` repository
4. Vercel auto-detects Astro - **no configuration needed!**
5. Click "Deploy"

**Build Settings (auto-detected):**
- Framework Preset: `Astro`
- Build Command: `pnpm build` or `npm run build`
- Output Directory: `dist`
- Install Command: `pnpm install` or `npm install`

#### Step 3: Wait for Build

- First build takes ~1-2 minutes
- You'll get a temporary URL like `davidmainoo-me.vercel.app`

#### Step 4: Test Deployment

- [ ] Visit the Vercel URL
- [ ] Test all pages
- [ ] Check dark mode
- [ ] Test blog filtering
- [ ] Verify fonts load correctly

---

### Option 2: Netlify

#### Step 1: Push to GitHub

```bash
git push origin main
```

#### Step 2: Deploy on Netlify

1. Go to [netlify.com](https://netlify.com) and sign up/login
2. Click "Add new site" → "Import an existing project"
3. Connect to GitHub and select your repository
4. Configure build settings:
   - **Build command:** `pnpm build`
   - **Publish directory:** `dist`
5. Click "Deploy site"

#### Step 3: Wait for Build

- First build takes ~1-2 minutes
- You'll get a URL like `random-name-123.netlify.app`

---

### Option 3: Cloudflare Pages

#### Step 1: Push to GitHub

```bash
git push origin main
```

#### Step 2: Deploy on Cloudflare Pages

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com)
2. Navigate to "Workers & Pages" → "Create application" → "Pages"
3. Connect to GitHub and select your repository
4. Configure build settings:
   - **Production branch:** `main`
   - **Build command:** `pnpm build`
   - **Build output directory:** `dist`
5. Click "Save and Deploy"

---

## Custom Domain Setup

### Purchase a Domain

**Recommended registrars:**
- [Namecheap](https://www.namecheap.com) - Good prices, easy to use
- [Cloudflare Registrar](https://www.cloudflare.com/products/registrar/) - At-cost pricing
- [Google Domains](https://domains.google) - Simple interface

**Domain suggestions:**
- `davidmainoo.com` (if available)
- `davidmainoo.dev`
- `david-mainoo.com`

### Connect Domain to Vercel

1. Go to your project in Vercel → Settings → Domains
2. Add your custom domain (e.g., `davidmainoo.com`)
3. Vercel gives you DNS records to add
4. Go to your domain registrar's DNS settings
5. Add the DNS records provided by Vercel:
   - **A Record:** `76.76.21.21` (points to Vercel)
   - **CNAME Record (www):** `cname.vercel-dns.com`
6. Wait 5-60 minutes for DNS propagation
7. Vercel automatically provisions SSL certificate

### Connect Domain to Netlify

1. Go to Site settings → Domain management → Add custom domain
2. Add your domain
3. Update DNS records at your registrar:
   - **A Record:** Netlify's load balancer IP
   - **CNAME (www):** `your-site.netlify.app`
4. Wait for DNS propagation
5. Netlify auto-provisions SSL

### Connect Domain to Cloudflare Pages

1. Transfer domain to Cloudflare (recommended) OR
2. Change nameservers to Cloudflare
3. In Cloudflare Pages → Custom domains → Add domain
4. DNS is automatically configured if domain is on Cloudflare

---

## Post-Deployment Checklist

### Verify Production Site

- [ ] **Homepage loads correctly**
  - [ ] All sections visible
  - [ ] Navigation works
  - [ ] Theme toggle works
  - [ ] Fonts load properly (no flash of unstyled text)

- [ ] **Blog page (/blog)**
  - [ ] All posts display
  - [ ] Search works
  - [ ] Tag filtering works
  - [ ] Pagination works
  - [ ] URL state persists (e.g., `/blog?tag=tech&page=2`)

- [ ] **Individual blog posts**
  - [ ] Posts load correctly
  - [ ] Back button works
  - [ ] Code syntax highlighting works
  - [ ] Images load (if any)

- [ ] **Dark mode**
  - [ ] Toggle works on all pages
  - [ ] Preference persists across navigation
  - [ ] No flashing between modes on page load

- [ ] **RSS Feed**
  - [ ] Accessible at `/rss.xml`
  - [ ] Valid RSS format
  - [ ] Contains your posts

### Performance Testing

Test your site performance:

1. **Lighthouse (Chrome DevTools)**
   ```
   - Right-click → Inspect → Lighthouse tab
   - Run audit (Mobile & Desktop)
   - Target scores: 90+ on all metrics
   ```

2. **PageSpeed Insights**
   - Go to [pagespeed.web.dev](https://pagespeed.web.dev)
   - Enter your domain
   - Check both Mobile and Desktop scores

3. **WebPageTest**
   - Go to [webpagetest.org](https://www.webpagetest.org)
   - Test from multiple locations
   - Check Time to First Byte (TTFB)

**Expected Performance:**
- First Contentful Paint (FCP): < 1.2s
- Largest Contentful Paint (LCP): < 2.5s
- Cumulative Layout Shift (CLS): < 0.1
- Time to Interactive (TTI): < 3.0s

### SEO Setup

- [ ] **Submit sitemap to Google Search Console**
  1. Go to [search.google.com/search-console](https://search.google.com/search-console)
  2. Add your domain
  3. Verify ownership (DNS TXT record)
  4. Submit sitemap at `/sitemap.xml` (if you add one)

- [ ] **Add robots.txt** (if needed)
  ```txt
  # /public/robots.txt
  User-agent: *
  Allow: /
  Sitemap: https://yourdomain.com/sitemap.xml
  ```

- [ ] **Verify social media cards**
  - Test with [Twitter Card Validator](https://cards-dev.twitter.com/validator)
  - Test with [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)

---

## Performance Optimization

### Already Implemented ✅

- ✅ Self-hosted fonts (no external DNS lookups)
- ✅ Optimized font loading (only weights you use)
- ✅ View transitions for smooth navigation
- ✅ Dark mode without flash
- ✅ Lazy loading images (Astro default)
- ✅ CSS/JS minification (production build)

### Optional Improvements

#### Add Sitemap

```bash
pnpm add @astrojs/sitemap
```

Update `astro.config.mjs`:
```js
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  site: 'https://davidmainoo.com', // Your domain
  integrations: [sitemap()],
});
```

#### Add Image Optimization

```bash
pnpm add @astrojs/image
```

Use optimized images in posts:
```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/my-image.jpg';
---

<Image src={myImage} alt="Description" />
```

#### Enable Compression

Most hosting platforms (Vercel, Netlify, Cloudflare) automatically enable Brotli/Gzip compression. Verify with:

```bash
curl -H "Accept-Encoding: br,gzip" -I https://yourdomain.com
```

Look for: `Content-Encoding: br` or `Content-Encoding: gzip`

---

## Monitoring & Analytics

### Privacy-Friendly Analytics

**Option 1: Vercel Analytics** (if using Vercel)
- Built-in, privacy-friendly
- No configuration needed
- Shows real Core Web Vitals

**Option 2: Plausible Analytics**
- Privacy-focused (no cookies)
- GDPR compliant
- €9/month

Add to `BaseLayout.astro`:
```html
<script defer data-domain="yourdomain.com" src="https://plausible.io/js/script.js"></script>
```

**Option 3: Cloudflare Web Analytics**
- Completely free
- Privacy-friendly
- Add snippet to `BaseLayout.astro`

### Error Monitoring

**Option: Sentry** (optional)
- Free tier available
- Tracks JavaScript errors
- Get notified when things break

```bash
pnpm add @sentry/astro
```

---

## Troubleshooting

### Build Fails on Platform

**Check:**
1. Build works locally: `pnpm build`
2. Correct Node version (v18+ recommended)
3. Package manager matches (use pnpm or npm consistently)

**Common fixes:**
- Set Node version in `package.json`:
  ```json
  "engines": {
    "node": ">=18.0.0"
  }
  ```

### Fonts Not Loading

**Verify:**
- [ ] Build includes `.woff2` files in `dist/_astro/`
- [ ] No 404s in browser Network tab
- [ ] CSS imports are correct in `global.css`

### Dark Mode Flashing

**Fixed** ✅ - Theme persistence is implemented in `BaseLayout.astro`

If issues persist:
- Check localStorage in browser DevTools
- Verify `is:inline` script in BaseLayout

### 404 Errors on Navigation

**If using Netlify/Cloudflare:**

Add `public/_redirects`:
```
/*    /index.html   200
```

**If using Vercel:**
Add `vercel.json`:
```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

### Slow Build Times

- Clear build cache: `rm -rf node_modules/.astro dist`
- Rebuild: `pnpm install && pnpm build`

---

## Going Live Checklist

Final steps before announcing:

- [ ] All placeholder content replaced with real content
- [ ] Social media links point to your profiles
- [ ] Resume PDF is uploaded and accessible
- [ ] "Let's talk" links point to correct contact method
- [ ] Custom domain configured and SSL active
- [ ] Test on mobile devices (iOS Safari, Chrome)
- [ ] Test on different browsers (Chrome, Firefox, Safari, Edge)
- [ ] Analytics tracking works
- [ ] RSS feed contains your posts
- [ ] Site indexed by Google (check Search Console)
- [ ] Performance scores are good (Lighthouse 90+)
- [ ] Dark mode works correctly everywhere
- [ ] All blog posts are published (not drafts)

---

## Continuous Deployment (Automatic Updates)

All recommended platforms support automatic deployment:

**How it works:**
1. You push changes to GitHub: `git push`
2. Platform detects the push
3. Automatically builds and deploys
4. New version live in ~2 minutes

**No manual redeployment needed!**

To deploy updates:
```bash
# Make your changes
git add -A
git commit -m "Update blog post"
git push origin main

# Platform automatically deploys
# Check deployment status in platform dashboard
```

---

## Support & Resources

### Astro Documentation
- [Deployment Guide](https://docs.astro.build/en/guides/deploy/)
- [Astro Discord](https://astro.build/chat) - Very active community

### Hosting Support
- [Vercel Docs](https://vercel.com/docs)
- [Netlify Docs](https://docs.netlify.com)
- [Cloudflare Pages Docs](https://developers.cloudflare.com/pages)

### Performance Tools
- [Lighthouse CI](https://github.com/GoogleChrome/lighthouse-ci) - Automated testing
- [WebPageTest](https://webpagetest.org) - Detailed performance analysis

---

## Quick Deploy Commands

```bash
# 1. Final check
pnpm build
pnpm preview  # Test at http://localhost:4321

# 2. Commit and push
git add -A
git commit -m "Prepare for production deployment"
git push origin main

# 3. Deploy on Vercel/Netlify/Cloudflare (one-time setup)
# Then automatic deploys on every git push!
```

---

**Ready to deploy?** Start with Vercel for the easiest experience. Your site will be live in under 10 minutes! 🚀
