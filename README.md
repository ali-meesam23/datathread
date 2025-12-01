# DataThread Consulting Website

## About

DataThread helps financial, crypto, and industrial businesses upgrade their decision-making, trading infrastructure, and operations through rigorous modelling, digital transformation, and custom tools.

**Tagline:** Designing systems that make your business measurably better.

### What We Do

We specialize in four core areas:

1. **Digital Transformation & Data Systems** - Workflow digitization, KPI dashboards, and data pipelines
2. **Financial Modelling & Scenario Analysis** - Pricing models, forecasting, and cashflow stress testing
3. **Trading System Design** - Architecture for automated trading systems (TradFi, derivatives, crypto)
4. **Industrial & Mining Process Optimization** - Throughput improvements, sensor projects, and KPI frameworks

### Industries

- Traditional finance
- Crypto and digital assets
- Mining (soft rock)
- Chemical operations (nitrogen and phosphate)

**Website:** [datathread.ca](https://datathread.ca)

---

## Frontend Development & Deployment Guide

### Project Structure

```
website/
├── docs/              # Production website (deployed to Netlify)
│   ├── index.html    # Main page
│   ├── style.css     # Styles
│   ├── logo.png      # Company logo
│   ├── meesam.jpg    # Team photo
│   └── tingting.jpg  # Team photo
├── apps/             # Old template (not used in production)
├── netlify.toml      # Netlify configuration
└── README.md         # This file
```

### Local Development

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd website
   ```

2. **Open the website locally:**
   - Simply open `docs/index.html` in your browser
   - Or use a local server:
     ```bash
     # Using Python
     cd docs
     python -m http.server 8000
     
     # Using Node.js (if you have http-server installed)
     npx http-server docs -p 8000
     ```
   - Visit `http://localhost:8000`

3. **Make changes:**
   - Edit `docs/index.html` for content/structure
   - Edit `docs/style.css` for styling
   - All assets (images, CSS) use relative paths

### Deployment

The website is deployed on **Netlify** and automatically deploys from the `master` branch.

#### Quick Deploy Steps

1. **Connect to Netlify:**
   - Go to [Netlify Dashboard](https://app.netlify.com)
   - Click "Add new site" → "Import an existing project"
   - Connect your GitHub repository
   - Configure:
     - **Publish directory:** `docs`
     - **Build command:** (leave empty - static site)

2. **Custom Domain Setup:**
   - In Netlify: **Site settings** → **Domain management** → **Add custom domain**
   - Enter: `datathread.ca`
   - Follow Netlify's DNS instructions to update Route53 records

3. **Form Configuration:**
   - **Site settings** → **Forms** → **Form notifications**
   - Add email notification to: `meesam.ali@datathread.ca`

4. **Auto-deployment:**
   - Push to `master` branch → Netlify automatically deploys
   ```bash
   git add .
   git commit -m "Your changes"
   git push origin master
   ```

#### Netlify Configuration

The `netlify.toml` file configures:
- Publish directory: `docs`
- Forms enabled: Yes
- No build command needed (static site)

### Technologies Used

- **HTML5** - Structure
- **CSS3** - Styling
- **Vanilla JavaScript** - Form handling and smooth scrolling
- **Netlify Forms** - Contact form backend
- **Netlify** - Hosting and CDN

---

## Backend Overview

This is a **static website** with minimal backend dependencies:

### Backend Services

1. **Netlify Forms** (Primary Backend Service)
   - **Purpose:** Handles contact form submissions
   - **How it works:**
     - Form has `data-netlify="true"` attribute
     - Submissions are processed by Netlify's backend
     - Form data is stored in Netlify dashboard
     - Email notifications sent via Netlify
   - **Configuration:** Set up in Netlify dashboard → Forms → Notifications
   - **No API keys needed** - works automatically when hosted on Netlify

2. **Netlify Hosting**
   - **Purpose:** Serves static files via CDN
   - **Features:**
     - Automatic HTTPS/SSL certificates
     - Global CDN distribution
     - Automatic deployments from GitHub

### Data Flow

```
User fills contact form
    ↓
Form submitted via AJAX (prevents page reload)
    ↓
POST request to Netlify Forms endpoint
    ↓
Netlify processes and stores submission
    ↓
Email notification sent to configured address
    ↓
Form data visible in Netlify dashboard
```

### No Additional Backend Required

- ✅ No database needed (form submissions stored in Netlify)
- ✅ No server-side code (pure static HTML/CSS/JS)
- ✅ No API integrations (form handled by Netlify)
- ✅ No authentication/authorization needed

### Form Submission Details

- **Form name:** `contact`
- **Fields:** name, company, email, message, budget, timeline
- **Spam protection:** Honeypot field (`bot-field`)
- **Submission method:** AJAX (prevents 404 redirect)
- **Success/Error handling:** Client-side JavaScript

---

## Contact

- **Email:** meesam.ali@datathread.ca
- **Phone:** +1 (306) 880-1987
- **Website:** https://datathread.ca

---

## Notes

- **Production files:** All production files are in the `docs/` directory
- **Unused files:**
  - `apps/` folder - Old HTML template, not used in production (kept for reference)
  - `landingpageinfo.md` - Content reference document (not used in code)
- The website is a static site with no build process required
- Form submissions are free up to 100/month on Netlify's free tier

