# Quick Start: Deploy to Netlify

Follow these steps to get your site live on Netlify:

## Step 1: Connect GitHub to Netlify (5 minutes)

1. Go to [https://app.netlify.com](https://app.netlify.com)
2. Sign in (or create a free account)
3. Click **"Add new site"** → **"Import an existing project"**
4. Click **"Deploy with GitHub"**
5. Authorize Netlify to access your GitHub account
6. Select your repository: **`ali-meesam23/datathread`**

## Step 2: Configure Build Settings (2 minutes)

Netlify should auto-detect your settings, but verify:

- **Branch to deploy**: `master`
- **Base directory**: (leave empty)
- **Publish directory**: `docs` ⚠️ **IMPORTANT: Set this to `docs`**
- **Build command**: (leave empty - no build needed)

Click **"Deploy site"**

## Step 3: Add Custom Domain (3 minutes)

1. After deployment, go to **Site settings** → **Domain management**
2. Click **"Add custom domain"**
3. Enter: `datathread.ca`
4. Click **"Verify"**
5. Netlify will show you DNS configuration options

## Step 4: Configure DNS in Route53 (Keep Your Existing DNS)

**Important**: You're keeping Route53 for DNS management (for email and other services). Just point the website to Netlify.

1. In Netlify dashboard, after adding `datathread.ca`:
   - Go to **Domain management**
   - You'll see your site's Netlify domain (e.g., `your-site-name.netlify.app`)
   - Note this domain name

2. In AWS Route53, update DNS records:

   **For the apex domain (`datathread.ca`):**
   - Delete any existing A records pointing to GitHub Pages IPs
   - Create a new record:
     - **Name**: Leave blank (or `@`)
     - **Type**: A
     - **Alias**: Yes (toggle ON)
     - **Route traffic to**: 
       - Select: **Alias to CloudFront distribution**
       - **CloudFront distribution**: You'll need to find Netlify's CloudFront distribution
       - OR use this workaround: Create a CNAME for `www` and use ALIAS to point to that
   
   **Easier approach for apex domain:**
   - Since Route53 doesn't allow CNAME at apex, use this:
   - Keep your existing A records (or create new ones) pointing to Netlify's IPs
   - Netlify will provide IP addresses when you add the domain
   - OR: Use `www` subdomain and redirect apex to www

   **For `www` subdomain (Recommended):**
   - **Name**: `www`
   - **Type**: CNAME
   - **Value**: `your-site-name.netlify.app` (your actual Netlify domain)
   - **TTL**: 300

3. **Alternative: Use www and redirect apex**
   - Set up `www.datathread.ca` with CNAME to Netlify
   - In Netlify: **Domain management** → Add redirect from `datathread.ca` to `www.datathread.ca`
   - This way both work, and you keep Route53 DNS

**Note**: Netlify will show you the exact DNS records needed when you add the custom domain. Follow those instructions, but keep your Route53 hosted zone (don't transfer nameservers).

## Step 5: Set Up Form Notifications (2 minutes)

1. In Netlify dashboard: **Site settings** → **Forms** → **Form notifications**
2. Click **"Add notification"**
3. Select **Email notification**
4. Configure:
   - **Email to notify**: `contact@datathread.ca`
   - **Subject**: `New Contact: DataThread Website`
5. Click **"Save"**

## Step 6: Enable HTTPS (Automatic)

- Netlify automatically provisions SSL certificates
- Wait 5-10 minutes after DNS propagates
- HTTPS will be enabled automatically
- You can enforce HTTPS in **Site settings** → **Domain management** → **HTTPS**

## Step 7: Test Everything

1. Visit `https://datathread.ca` (wait for DNS to propagate)
2. Test the contact form
3. Check your email for form submissions
4. Verify all images load correctly

## That's It! 🎉

Your site is now:
- ✅ Hosted for free on Netlify
- ✅ Using HTTPS automatically
- ✅ Forms working and sending emails
- ✅ Fast CDN globally
- ✅ Auto-deploying from GitHub

## Future Updates

Just push to GitHub and Netlify will automatically deploy:
```bash
git add .
git commit -m "Your changes"
git push origin master
```

Netlify will rebuild and deploy automatically!

## Need Help?

- Check `NETLIFY_SETUP.md` for detailed instructions
- Netlify docs: https://docs.netlify.com
- Form submissions: Netlify dashboard → Forms

