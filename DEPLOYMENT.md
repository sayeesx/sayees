# 🚀 Deployment Guide

Complete guide to deploying your portfolio to various platforms.

## Pre-Deployment Checklist

Before deploying, ensure you've updated:

- [ ] Domain name in `index.html` (all canonical and OG URLs)
- [ ] Domain in `sitemap.xml`
- [ ] Domain in `security.txt`
- [ ] Profile image path
- [ ] Personal information (name, bio, skills)
- [ ] Social links (GitHub, LinkedIn, email)
- [ ] Project descriptions

## Option 1: Vercel (Recommended)

### Why Vercel?
- Free SSL/HTTPS
- Global CDN
- Zero configuration
- Custom domain support
- Automatic deployments

### Steps

1. **Install Vercel CLI** (optional)
   ```bash
   npm i -g vercel
   ```

2. **Deploy via CLI**
   ```bash
   cd portfolio
   vercel
   ```

3. **Or Deploy via GitHub**
   - Push code to GitHub
   - Visit [vercel.com](https://vercel.com)
   - Import your repository
   - Deploy

4. **Add Custom Domain**
   - Go to Project Settings
   - Domains → Add Domain
   - Enter: `sayees.narrs.in`
   - Update DNS records as instructed

### DNS Configuration
```
Type: CNAME
Name: sayees
Value: cname.vercel-dns.com
```

## Option 2: Netlify

### Steps

1. **Drag & Drop**
   - Visit [netlify.com](https://netlify.com)
   - Drag portfolio folder to deploy zone
   - Done!

2. **Via Git**
   - Connect GitHub repository
   - Auto-deploy on push

3. **Custom Domain**
   - Site Settings → Domain Management
   - Add custom domain
   - Update DNS

### DNS Configuration
```
Type: CNAME
Name: sayees
Value: [your-site].netlify.app
```

## Option 3: GitHub Pages

### Steps

1. **Create Repository**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/yourusername/portfolio.git
   git push -u origin main
   ```

2. **Enable GitHub Pages**
   - Repository Settings
   - Pages section
   - Source: main branch
   - Save

3. **Custom Domain**
   - Add `CNAME` file with domain:
     ```
     sayees.narrs.in
     ```
   - Update DNS to point to GitHub

### DNS Configuration
```
Type: A
Name: @
Value: 185.199.108.153
       185.199.109.153
       185.199.110.153
       185.199.111.153

Type: CNAME
Name: sayees
Value: yourusername.github.io
```

## Option 4: Custom Server (VPS/Cloud)

### Requirements
- Web server (Nginx/Apache)
- SSL certificate (Let's Encrypt)
- Domain access

### Nginx Configuration

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name sayees.narrs.in www.sayees.narrs.in;
    
    # Redirect to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name sayees.narrs.in www.sayees.narrs.in;
    
    # SSL Configuration
    ssl_certificate /etc/letsencrypt/live/sayees.narrs.in/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/sayees.narrs.in/privkey.pem;
    
    # Root directory
    root /var/www/portfolio;
    index index.html;
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    
    # Cache static assets
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|webmanifest)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # Main location
    location / {
        try_files $uri $uri/ =404;
    }
    
    # Error pages
    error_page 404 /index.html;
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName sayees.narrs.in
    ServerAlias www.sayees.narrs.in
    
    # Redirect to HTTPS
    Redirect permanent / https://sayees.narrs.in/
</VirtualHost>

<VirtualHost *:443>
    ServerName sayees.narrs.in
    ServerAlias www.sayees.narrs.in
    
    DocumentRoot /var/www/portfolio
    
    # SSL Configuration
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/sayees.narrs.in/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/sayees.narrs.in/privkey.pem
    
    # Security headers
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-XSS-Protection "1; mode=block"
    
    <Directory /var/www/portfolio>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    # Cache static assets
    <FilesMatch "\.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|webmanifest)$">
        Header set Cache-Control "max-age=31536000, public, immutable"
    </FilesMatch>
</VirtualHost>
```

## Post-Deployment

### 1. Test Your Site

- [ ] Homepage loads correctly
- [ ] All images display
- [ ] Favicons appear in browser tab
- [ ] Mobile responsive design works
- [ ] Links work (social, email)
- [ ] Tabs switch properly

### 2. Test SEO Files

- [ ] Visit: `yoursite.com/sitemap.xml`
- [ ] Visit: `yoursite.com/robots.txt`
- [ ] Visit: `yoursite.com/llms.txt`
- [ ] Visit: `yoursite.com/.well-known/security.txt`

### 3. Verify Search Engine Indexing

#### Google Search Console
1. Visit [search.google.com/search-console](https://search.google.com/search-console)
2. Add property (your domain)
3. Verify ownership
4. Submit sitemap
5. Request indexing

#### Bing Webmaster Tools
1. Visit [bing.com/webmasters](https://www.bing.com/webmasters)
2. Add site
3. Verify ownership
4. Submit sitemap

### 4. Test Performance

- **PageSpeed Insights**: [pagespeed.web.dev](https://pagespeed.web.dev)
- **GTmetrix**: [gtmetrix.com](https://gtmetrix.com)
- **WebPageTest**: [webpagetest.org](https://www.webpagetest.org)

### 5. Test Social Sharing

- **Facebook**: [developers.facebook.com/tools/debug](https://developers.facebook.com/tools/debug/)
- **Twitter**: [cards-dev.twitter.com/validator](https://cards-dev.twitter.com/validator)
- **LinkedIn**: Share and check preview

### 6. Monitor Analytics (Optional)

Add Google Analytics or similar:

```html
<!-- Add before </head> in index.html -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

## SSL Certificate (Let's Encrypt)

If using custom server:

```bash
# Install certbot
sudo apt-get update
sudo apt-get install certbot python3-certbot-nginx

# Get certificate
sudo certbot --nginx -d sayees.narrs.in -d www.sayees.narrs.in

# Auto-renewal (test)
sudo certbot renew --dry-run
```

## Troubleshooting

### Images Not Loading
- Check file paths are correct
- Verify images exist in `/assets/` folder
- Check file permissions (644 for files, 755 for directories)

### Favicon Not Showing
- Clear browser cache (Ctrl+Shift+Delete)
- Wait 5-10 minutes for propagation
- Test in incognito mode

### Domain Not Working
- Check DNS propagation: [whatsmydns.net](https://www.whatsmydns.net)
- DNS can take 24-48 hours to propagate
- Verify DNS records are correct

### SSL Issues
- Ensure certificate is valid
- Check certificate paths in server config
- Verify port 443 is open

## Need Help?

- Check hosting provider documentation
- Search Stack Overflow
- Contact: sayeesck@yahoo.com

---

Good luck with your deployment! 🚀
