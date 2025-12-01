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

## Step 4: Configure DNS (Choose One)

### Option A: Use Netlify DNS (Easiest) ⭐ Recommended

1. In Netlify: **Domain management** → **DNS**
2. Click **"Add DNS zone"** or **"Use Netlify DNS"**
3. Netlify will provide 4 nameservers (e.g., `dns1.p01.nsone.net`)
4. Copy these nameservers
5. In AWS Route53:
   - Go to your hosted zone for `datathread.ca`
   - Click **Edit** on the nameservers
   - Replace with Netlify's nameservers
   - Save
6. Wait 5-10 minutes for DNS propagation

### Option B: Keep Route53 DNS

1. In Netlify, note your site's Netlify domain (e.g., `your-site.netlify.app`)
2. In Route53, for the apex domain (`datathread.ca`):
   - Delete existing A records
   - Create new record:
     - **Type**: A
     - **Alias**: Yes
     - **Route traffic to**: You'll need to use Netlify's load balancer IPs
     - (This is more complex - Option A is easier)
3. For `www` subdomain:
   - **Type**: CNAME
   - **Value**: `your-site.netlify.app`

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

