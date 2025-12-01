# GitHub Pages Setup Guide for datathread.ca

## Step 1: Enable GitHub Pages

1. Go to your GitHub repository: `https://github.com/ali-meesam23/datathread`
2. Click on **Settings** (top menu)
3. Scroll down to **Pages** in the left sidebar
4. Under **Source**, select:
   - **Branch**: `master` (or `main` if that's your default branch)
   - **Folder**: `/docs`
5. Click **Save**

Your site will now be live at: `https://ali-meesam23.github.io/datathread/`

## Step 2: Configure Custom Domain (datathread.ca)

### In GitHub:

1. Still in **Settings > Pages**
2. Under **Custom domain**, enter: `datathread.ca`
3. Check **Enforce HTTPS** (this will be available after DNS is configured)
4. Click **Save**

GitHub will create a commit that adds/updates the CNAME file (which we already have in the docs folder).

### In AWS Route53:

1. Log into AWS Console and go to Route53
2. Select your hosted zone for `datathread.ca`
3. Create/Edit DNS records:

#### Option A: Using Apex Domain (datathread.ca)
Create an **A record**:
- **Name**: Leave blank or enter `@` (for apex domain)
- **Type**: A
- **Value**: 
  - `185.199.108.153`
  - `185.199.109.153`
  - `185.199.110.153`
  - `185.199.111.153`
- **TTL**: 300 (or your preference)

#### Option B: Using CNAME (www.datathread.ca)
If you want www subdomain, create a **CNAME record**:
- **Name**: `www`
- **Type**: CNAME
- **Value**: `ali-meesam23.github.io`
- **TTL**: 300

**Note**: GitHub Pages IP addresses can change. For the most current IPs, check:
- https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain

### Verify DNS Configuration:

Wait 5-10 minutes for DNS propagation, then verify:

```bash
# Check A records
dig datathread.ca +short

# Check CNAME (if using www)
dig www.datathread.ca +short
```

You should see the GitHub Pages IP addresses.

## Step 3: SSL Certificate (HTTPS)

1. After DNS propagates (can take up to 24 hours, usually much faster)
2. Go back to **GitHub Settings > Pages**
3. The **Enforce HTTPS** checkbox should become available
4. Check it to enable HTTPS

GitHub automatically provisions SSL certificates via Let's Encrypt.

## Step 4: Verify Everything Works

1. Visit `https://datathread.ca` (wait for DNS/SSL to propagate)
2. Check that all images load correctly
3. Test the contact form
4. Verify all internal links work

## Troubleshooting

### Site not loading:
- Wait for DNS propagation (can take up to 24 hours)
- Clear your browser cache
- Check DNS with: `dig datathread.ca` or use online tools like `dnschecker.org`

### HTTPS not working:
- Wait for SSL certificate provisioning (usually automatic within hours)
- Make sure DNS is fully propagated first
- Check that "Enforce HTTPS" is enabled in GitHub Pages settings

### Images not loading:
- Verify image paths are relative (they are: `logo.png`, `meesam.jpg`, `tingting.jpg`)
- Check that images exist in the `docs/` folder
- Clear browser cache

## Current Repository Structure

```
website/
├── docs/              # GitHub Pages source folder
│   ├── index.html    # Main page
│   ├── style.css     # Styles
│   ├── logo.png      # Company logo
│   ├── meesam.jpg    # Meesam's photo
│   ├── tingting.jpg  # TingTing's photo
│   └── CNAME         # Custom domain config
├── apps/             # Old template (not used)
└── .gitignore        # Git ignore rules
```

## Notes

- The `CNAME` file in `docs/` tells GitHub Pages to use `datathread.ca` as the custom domain
- All assets (images, CSS) use relative paths, so they work correctly
- The contact form uses a mailto link (opens default email client)
- For production, you might want to consider a form service like Formspree or Netlify Forms

