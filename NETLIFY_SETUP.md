# Netlify Forms Setup Guide

Your contact form is now configured to use Netlify Forms. You have two options:

## Option 1: Host on Netlify (Recommended) ⭐

This is the easiest and best option. Netlify Forms work seamlessly when you host on Netlify.

### Steps:

1. **Push your code to GitHub** (already done ✅)

2. **Connect to Netlify:**
   - Go to [Netlify Dashboard](https://app.netlify.com)
   - Click **"Add new site"** → **"Import an existing project"**
   - Choose **GitHub** and authorize Netlify
   - Select your repository: `ali-meesam23/datathread`
   - Configure build settings:
     - **Base directory**: Leave empty (or `docs` if you want to keep the docs folder structure)
     - **Publish directory**: `docs` (this is where your `index.html` is)
     - **Build command**: Leave empty (static site, no build needed)
   - Click **"Deploy site"**

3. **Configure Custom Domain:**
   - In Netlify dashboard, go to **Site settings** → **Domain management**
   - Click **"Add custom domain"**
   - Enter `datathread.ca`
   - Netlify will show you DNS records to add in Route53

4. **Update Route53 DNS:**

   **Important**: Route53 doesn't allow CNAME records at the apex (root) domain. You have two options:

   ### Option A: Use ALIAS Record (Recommended for Route53)
   
   For the apex domain (`datathread.ca`):
   - **Name**: Leave blank or enter `@` (for apex domain)
   - **Type**: A (but enable "Alias")
   - **Alias**: Yes (toggle this ON)
   - **Alias target**: Select from the dropdown or enter your Netlify domain
     - Look for: `Netlify` in the alias target dropdown
     - Or manually enter: `your-site-name.netlify.app` (replace with your actual Netlify domain)
   - **Evaluate target health**: No (or Yes, your choice)
   - Click **Create records**

   For the `www` subdomain:
   - **Name**: `www`
   - **Type**: CNAME
   - **Value**: `your-site-name.netlify.app` (your actual Netlify domain)
   - Click **Create records**

   ### Option B: Use Netlify's DNS (Easier Alternative)
   
   If you prefer, you can transfer DNS management to Netlify:
   - In Netlify dashboard: **Site settings** → **Domain management** → **DNS**
   - Netlify will provide nameservers
   - In Route53: Go to your hosted zone → **Edit** → Update nameservers to Netlify's nameservers
   - This way, Netlify manages all DNS records automatically

5. **Configure Form Notifications:**
   - Go to **Site settings** → **Forms** → **Form notifications**
   - Click **"Add notification"**
   - Choose **Email notification**
   - Enter: `contact@datathread.ca` (or your preferred email)
   - Subject: `New Contact Form Submission from DataThread`
   - Click **"Save"**

6. **Test the Form:**
   - Submit a test message through the contact form
   - Check your email inbox for the notification
   - View submissions in Netlify dashboard under **Forms** → **Active forms** → **contact**

### Benefits:
- ✅ Forms work automatically
- ✅ Free SSL certificate
- ✅ Fast CDN
- ✅ Easy deployments from GitHub
- ✅ Form submissions visible in Netlify dashboard
- ✅ Email notifications

---

## Option 2: Keep GitHub Pages + Use Netlify Forms (Workaround)

If you want to keep GitHub Pages but use Netlify Forms, you need to:

1. **Create a Netlify site** (can be empty/minimal)
2. **Point your form to Netlify's endpoint**
3. **Use AJAX to submit forms**

This is more complex and not recommended. Option 1 is much better.

---

## Form Configuration

Your form is already set up with:
- ✅ `data-netlify="true"` attribute
- ✅ `name="contact"` for form identification
- ✅ Honeypot spam protection (`bot-field`)
- ✅ Success/error message handling

## Viewing Form Submissions

1. Go to your Netlify dashboard
2. Click on your site
3. Go to **Forms** tab
4. Click on **"contact"** form
5. View all submissions here
6. Export as CSV if needed

## Email Notifications Setup

1. In Netlify dashboard: **Site settings** → **Forms** → **Form notifications**
2. Click **"Add notification"**
3. Select **Email notification**
4. Configure:
   - **Email to notify**: `contact@datathread.ca`
   - **Email from**: `Netlify Forms <forms@netlify.com>` (or customize)
   - **Subject**: `New Contact: DataThread Website`
   - **Email template**: You can customize the email template

## Customizing Email Notifications

You can customize the email template in Netlify:
- Go to **Forms** → **Form notifications** → Edit your notification
- Use variables like:
  - `{{form_name}}` - Form name
  - `{{all_fields}}` - All form fields
  - `{{name}}`, `{{email}}`, `{{message}}` - Specific fields

## Testing

1. Fill out the contact form on your site
2. Submit it
3. You should see a success message
4. Check your email inbox
5. Check Netlify dashboard for the submission

## Troubleshooting

### Form not submitting:
- Make sure you're hosting on Netlify (Option 1)
- Check browser console for errors
- Verify `data-netlify="true"` is in the form tag

### Not receiving emails:
- Check Netlify dashboard → Forms → Notifications
- Verify email address is correct
- Check spam folder
- Ensure notification is enabled

### Form submissions not showing:
- Check Netlify dashboard → Forms → Active forms
- Make sure form has `name="contact"` attribute
- Verify you're on the correct Netlify site

