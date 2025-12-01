# Route53 + Netlify Setup (Keep Route53 DNS)

This guide is for keeping Route53 DNS management while hosting your website on Netlify.

## Why This Setup?

- ✅ Keep Route53 for DNS (email, other services stay configured)
- ✅ Host website on Netlify (for working contact forms)
- ✅ No need to transfer nameservers
- ✅ All existing DNS records remain unchanged

## Step-by-Step Setup

### 1. Deploy to Netlify

1. Go to [https://app.netlify.com](https://app.netlify.com)
2. **Add new site** → **Import from GitHub**
3. Select repository: `ali-meesam23/datathread`
4. Build settings:
   - **Publish directory**: `docs`
   - **Build command**: (leave empty)
5. Click **Deploy site**

### 2. Add Custom Domain in Netlify

1. After deployment: **Site settings** → **Domain management**
2. Click **Add custom domain**
3. Enter: `datathread.ca`
4. Click **Verify**
5. Netlify will show you DNS configuration options

### 3. Configure Route53 DNS Records

**Important**: Keep your existing Route53 hosted zone. Only update the website-related records.

#### Option A: Use www Subdomain (Easiest) ⭐ Recommended

1. In Route53, for `www` subdomain:
   - **Name**: `www`
   - **Type**: CNAME
   - **Value**: `your-site-name.netlify.app` (from Netlify)
   - **TTL**: 300
   - Click **Create records**

2. In Netlify: **Domain management** → **HTTPS** → Enable redirect:
   - Redirect `datathread.ca` → `www.datathread.ca`
   - This makes both URLs work

3. For apex domain (`datathread.ca`):
   - Keep existing A records (for email, etc.)
   - OR create a redirect record if needed
   - The redirect in Netlify will handle visitors

#### Option B: Point Apex Domain Directly to Netlify

1. In Netlify dashboard, when you add `datathread.ca`, it will show you DNS records needed
2. Look for the A record IP addresses Netlify provides
3. In Route53, for apex domain:
   - Delete old A records (if they were for GitHub Pages)
   - Create new A records with Netlify's IP addresses:
     - **Name**: (blank or `@`)
     - **Type**: A
     - **Value**: (IP addresses from Netlify - usually 4 IPs)
     - **TTL**: 300
   - Create 4 separate A records, one for each IP

**Note**: Netlify's IP addresses may change. Check Netlify's documentation for current IPs, or use the www subdomain approach (Option A) which is more stable.

### 4. Verify DNS

1. Wait 5-10 minutes for DNS propagation
2. Test: `dig datathread.ca` or `dig www.datathread.ca`
3. Visit your site to confirm it loads

### 5. Set Up Form Notifications

1. Netlify dashboard: **Site settings** → **Forms** → **Form notifications**
2. Click **Add notification**
3. Select **Email notification**
4. Configure:
   - **Email to notify**: `contact@datathread.ca`
   - **Subject**: `New Contact: DataThread Website`
5. Click **Save**

### 6. Enable HTTPS

- Netlify automatically provisions SSL certificates
- Wait 5-10 minutes after DNS propagates
- HTTPS will be enabled automatically
- Enforce HTTPS: **Site settings** → **Domain management** → **HTTPS** → **Enforce HTTPS**

## What Stays in Route53

✅ All your existing DNS records (email, MX records, etc.)
✅ Your hosted zone configuration
✅ Nameservers remain the same
✅ Only website A/CNAME records point to Netlify

## What Goes to Netlify

✅ Website hosting
✅ SSL certificates
✅ Form processing
✅ CDN delivery

## Testing

1. Visit `https://www.datathread.ca` (or `https://datathread.ca` if using redirect)
2. Test the contact form
3. Check email for form submissions
4. Verify all images load

## Troubleshooting

### Site not loading:
- Check DNS propagation: `dig www.datathread.ca` or use [dnschecker.org](https://dnschecker.org)
- Verify CNAME record in Route53 points to correct Netlify domain
- Wait 10-15 minutes for DNS to propagate

### HTTPS not working:
- Wait for SSL certificate provisioning (automatic, usually 5-10 minutes)
- Check Netlify dashboard → Domain management → HTTPS status

### Form not working:
- Verify form has `data-netlify="true"` attribute (already done ✅)
- Check Netlify dashboard → Forms → Active forms
- Ensure you're visiting the Netlify-hosted version (not cached GitHub Pages version)

## Summary

- **DNS**: Managed in Route53 (unchanged)
- **Hosting**: Netlify (for website only)
- **Forms**: Netlify Forms (100 free submissions/month)
- **Email**: Still uses Route53 MX records (unchanged)
- **Other services**: Still use Route53 DNS (unchanged)

This setup gives you working forms while keeping all your existing DNS configuration!

