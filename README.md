# CaliforniaMotorcycleAccidents.com

**California Motorcycle Accident Information & Resource Platform**

> Educational information and resources for motorcycle accident victims in California.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Tech Stack](#tech-stack)
3. [Quick Start (Local Development)](#quick-start)
4. [GitHub Pages Deployment](#github-pages-deployment)
5. [Netlify Deployment](#netlify-deployment)
6. [Vercel Deployment](#vercel-deployment)
7. [SEO Customization Guide](#seo-customization-guide)
8. [Content Editing Guide](#content-editing-guide)
9. [Adding Blog Articles](#adding-blog-articles)
10. [Performance Optimization](#performance-optimization)
11. [Lead Generation Setup](#lead-generation-setup)
12. [Dark Mode](#dark-mode)
13. [Spanish Language Support](#spanish-language-support)
14. [Disclaimer Requirements](#disclaimer-requirements)

---

## Project Overview

**Domain:** CaliforniaMotorcycleAccidents.com  
**Purpose:** Educational legal information platform for motorcycle accident victims in California  
**Contact:** salatrir@gmail.com

### Pages

| Route | Description |
|---|---|
| `/` | Home – stats, resources overview, lead capture |
| `/accident-guide` | Step-by-step post-accident guide |
| `/injury-information` | Injury types, symptoms, recovery |
| `/insurance-guide` | Coverage types, claims process, settlements |
| `/california-laws` | Helmet, lane splitting, insurance, reporting |
| `/compensation` | Compensation categories (educational) |
| `/checklist` | Interactive accident checklist |
| `/blog` | Article listing |
| `/blog/[slug]` | Individual article |
| `/contact` | Contact form |
| `/es` | Spanish-language home |
| `/privacy` | Privacy policy |
| `/terms` | Terms of use |

---

## Tech Stack

- **Framework:** Next.js 14 (App Router, Static Export)
- **Language:** TypeScript
- **Styling:** Tailwind CSS
- **Icons:** Lucide React
- **SEO:** Next.js Metadata API + Schema.org JSON-LD
- **Deployment:** GitHub Pages / Netlify / Vercel

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/yourusername/california-motorcycle-accidents.git
cd california-motorcycle-accidents

# Install dependencies
npm install

# Start local development server
npm run dev
# → http://localhost:3000

# Build for production
npm run build

# Preview production build
npm start
```

---

## GitHub Pages Deployment

### Step 1: Enable GitHub Pages

1. Push code to a GitHub repository
2. Go to **Settings → Pages**
3. Set Source to **GitHub Actions**

### Step 2: Create Workflow File

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./out
      - uses: actions/deploy-pages@v4
        id: deployment
```

### Step 3: Configure Domain

In `next.config.js`, if using a custom domain, no changes needed.  
If deploying to `username.github.io/repo-name`, add:
```js
basePath: '/repo-name',
```

---

## Netlify Deployment

### Option A: Netlify UI (Drag & Drop)

```bash
npm run build
# Drag the /out folder to Netlify drop zone at netlify.com/drop
```

### Option B: Netlify CLI

```bash
npm install -g netlify-cli
npm run build
netlify deploy --prod --dir=out
```

### Option C: Git Integration (Recommended)

1. Push to GitHub
2. Go to [app.netlify.com](https://app.netlify.com) → **Add new site → Import from Git**
3. Configure:
   - **Build command:** `npm run build`
   - **Publish directory:** `out`
4. Add your custom domain in **Domain management**

### netlify.toml (optional)

```toml
[build]
  command = "npm run build"
  publish = "out"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

---

## Vercel Deployment

### Option A: Vercel CLI

```bash
npm install -g vercel
vercel --prod
```

### Option B: Git Integration (Recommended)

1. Push to GitHub
2. Go to [vercel.com](https://vercel.com) → **Add New Project**
3. Import repository
4. Framework preset: **Next.js** (auto-detected)
5. Deploy — Vercel handles everything

> **Note:** For Vercel deployment, remove `output: 'export'` from `next.config.js` to use Vercel's native Next.js runtime with SSR/ISR capabilities.

---

## SEO Customization Guide

### Update Site URL

Edit `src/lib/seo.ts`:
```ts
export const SITE_URL = 'https://californiamotorcycleaccidents.com'
```

### Update Root Metadata

Edit `src/app/layout.tsx` → `metadata` object:
- `metadataBase` – your domain
- `title.default` – site name
- `description` – site-wide description
- `verification.google` – paste your Google Search Console verification code

### Verify in Google Search Console

1. Go to [search.google.com/search-console](https://search.google.com/search-console)
2. Add property → URL prefix → your domain
3. Copy the verification code
4. Paste into `src/app/layout.tsx` at `verification: { google: 'YOUR_CODE' }`

### Submit Sitemap

After deploying, submit:
```
https://californiamotorcycleaccidents.com/sitemap.xml
```
to Google Search Console and Bing Webmaster Tools.

### Page-Level SEO

Each page exports its own `metadata` object. Edit the `title` and `description` in each page file under `src/app/*/page.tsx`.

### Schema.org Structured Data

- Organization schema: `src/app/layout.tsx`
- FAQ schema: `src/app/page.tsx` (home) and `src/app/california-laws/page.tsx`
- Article schema: `src/app/blog/[slug]/page.tsx`
- HowTo schema: `src/app/accident-guide/page.tsx`

---

## Content Editing Guide

### Edit Page Content

All page content is in the page files:
```
src/app/page.tsx                    → Home page
src/app/accident-guide/page.tsx     → Accident guide
src/app/injury-information/page.tsx → Injury center
src/app/insurance-guide/page.tsx    → Insurance guide
src/app/california-laws/page.tsx    → CA laws
src/app/compensation/page.tsx       → Compensation
src/app/checklist/ChecklistClient.tsx → Checklist items
```

### Edit Checklist Items

Open `src/app/checklist/ChecklistClient.tsx` and modify the `sections` array:

```ts
const sections = [
  {
    id: 'scene',
    title: 'At the Scene',
    color: 'border-red-400',
    items: [
      { id: 'call-911', text: 'Your custom text here' },
      // ...
    ],
  },
]
```

### Edit Statistics

In `src/app/page.tsx`, find the `<StatCard>` components and update values:

```tsx
<StatCard value="15,000+" label="Crashes per year" note="Annual CA average" />
```

### Edit Navigation Links

`src/components/layout/Header.tsx` → `navLinks` array  
`src/components/layout/Footer.tsx` → `footerLinks` object

### Edit Contact Email

Search the project for `salatrir@gmail.com` and replace with your preferred email.

---

## Adding Blog Articles

### Option 1: MDX Files (Static)

1. Install MDX support:
```bash
npm install @next/mdx @mdx-js/loader @mdx-js/react
```

2. Create `src/content/blog/your-article-slug.mdx`

3. Update `src/app/blog/[slug]/page.tsx` to read MDX files dynamically

### Option 2: Headless CMS (Recommended for 300 articles)

Popular options:
- **Contentful** – enterprise-grade, great API
- **Sanity** – flexible schemas, great DX
- **Notion** – familiar interface, Notion API
- **Ghost** – built for content sites

### Option 3: Use the Article Seeds

`src/lib/articleSeeds.ts` contains structured data for 50 seed articles across all 6 categories (representing the planned 300). Use these slugs and metadata to generate content programmatically or with an AI writing tool.

### Article SEO Template

Each new article should include:
```tsx
export const metadata: Metadata = {
  title: 'Your Article Title – CaliforniaMotorcycleAccidents.com',
  description: 'Your 150-160 character description with primary keyword.',
  alternates: { canonical: 'https://californiamotorcycleaccidents.com/blog/your-slug' },
}
```

And include the Article schema JSON-LD block.

---

## Performance Optimization

### Image Optimization

- Use WebP format for all images
- Place images in `/public/images/`
- Use Next.js `<Image>` component (note: requires `unoptimized: true` for static export)
- Compress images with [Squoosh](https://squoosh.app) before uploading

### Core Web Vitals

- **LCP:** Keep hero section simple, avoid large images above fold
- **FID/INP:** Minimize client-side JS; most pages are server-rendered
- **CLS:** Set explicit width/height on all images

### Caching Headers (Netlify)

Add to `netlify.toml`:
```toml
[[headers]]
  for = "/static/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/*.html"
  [headers.values]
    Cache-Control = "public, max-age=0, must-revalidate"
```

### Caching Headers (Vercel)

Vercel handles this automatically for Next.js projects.

---

## Lead Generation Setup

### Connect Forms to a Backend

The `LeadForm` and `ContactForm` components currently simulate submission. Connect them to a real backend:

**Option A: Formspree (simplest)**
```ts
const response = await fetch('https://formspree.io/f/YOUR_FORM_ID', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(form),
})
```

**Option B: ConvertKit / Mailchimp (newsletter)**
Replace the simulated submit with your email service API call.

**Option C: Custom API Route (Vercel/Netlify)**
Create `src/app/api/contact/route.ts` with your email logic.

### Email Notification

For immediate email alerts on form submissions:
1. Use [Resend](https://resend.com) or [SendGrid](https://sendgrid.com)
2. Create an API route at `src/app/api/contact/route.ts`
3. Send confirmation to `salatrir@gmail.com`

---

## Dark Mode

Dark mode is enabled automatically based on the user's OS preference and can be toggled via the moon/sun icon in the header. The preference is saved in `localStorage`.

To change the default to dark:
```ts
// src/components/layout/ThemeProvider.tsx
const initial = saved ?? 'dark' // change 'light' to 'dark'
```

---

## Spanish Language Support

The Spanish home page is at `/es`. To expand Spanish support:

1. Create Spanish versions of pages under `src/app/es/`
2. Add `hreflang` tags in each page's metadata:
```ts
alternates: {
  languages: {
    'en': 'https://californiamotorcycleaccidents.com/accident-guide',
    'es': 'https://californiamotorcycleaccidents.com/es/guia-accidente',
  },
},
```
3. Consider a translation library like `next-intl` for full i18n support

---

## Disclaimer Requirements

**This is critical.** Every page must maintain the educational disclaimer. Do not:
- Claim to be a law firm
- Provide legal advice
- Guarantee outcomes or compensation amounts
- Create attorney-client relationships

The disclaimer is embedded in:
- `src/components/ui/DisclaimerBanner.tsx` (page-level)
- `src/components/layout/Footer.tsx` (site-wide)
- `src/app/contact/ContactForm.tsx` (form-level)
- `src/app/terms/page.tsx` (terms page)

---

## Project Structure

```
california-motorcycle-accidents/
├── public/
│   ├── manifest.json          # PWA manifest
│   ├── icons/                 # App icons (add your own)
│   └── images/               # Site images (add your own)
├── src/
│   ├── app/
│   │   ├── layout.tsx         # Root layout + global SEO
│   │   ├── page.tsx           # Home page
│   │   ├── sitemap.ts         # Auto-generated sitemap
│   │   ├── robots.ts          # robots.txt
│   │   ├── not-found.tsx      # 404 page
│   │   ├── accident-guide/
│   │   ├── injury-information/
│   │   ├── insurance-guide/
│   │   ├── california-laws/
│   │   ├── compensation/
│   │   ├── checklist/
│   │   ├── blog/
│   │   ├── contact/
│   │   ├── es/                # Spanish
│   │   ├── privacy/
│   │   └── terms/
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Header.tsx
│   │   │   ├── Footer.tsx
│   │   │   └── ThemeProvider.tsx
│   │   └── ui/
│   │       ├── StatCard.tsx
│   │       ├── DisclaimerBanner.tsx
│   │       ├── LeadForm.tsx
│   │       └── PageHero.tsx
│   ├── lib/
│   │   ├── seo.ts             # SEO helpers
│   │   └── articleSeeds.ts   # 50 article seed definitions
│   └── styles/
│       └── globals.css        # Global Tailwind styles
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── README.md
```

---

## License

Copyright © 2026 CaliforniaMotorcycleAccidents.com. All rights reserved.

Content is for educational purposes only. Nothing constitutes legal advice.
