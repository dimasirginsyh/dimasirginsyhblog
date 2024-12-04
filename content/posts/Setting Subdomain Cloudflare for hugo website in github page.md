---
title: Setting Subdomain Cloudflare for hugo website in github page
date: 2024-12-04
draft: false
tags:
  - go
  - mitoyus
  - blog
  - cloudflare
  - github
  - chatgpt
---
If you're using the **Hugo** framework to deploy your site on GitHub Pages and you're experiencing issues with missing styles or incorrect layout after setting up a subdomain through Cloudflare, there are a few things to check and configure. Here's a detailed guide to ensure that everything works smoothly.

### 1. **Check the `baseURL` in Hugo's Configuration**

The `baseURL` setting in Hugo's `config.toml` (or `config.yaml` or `config.json`, depending on your setup) is critical for making sure your site works correctly with a custom subdomain or domain.

For a GitHub Pages deployment, you'll need to set the `baseURL` in your Hugo configuration file to the full URL of your site (including `https://`).

#### Example configuration for `config.toml`:

```toml
baseURL = "https://your-subdomain.example.com/"
```

This ensures that Hugo generates links (e.g., for stylesheets, JavaScript files, and images) relative to the root of your subdomain.

### 2. **Set Up the DNS for Your Subdomain in Cloudflare**

Ensure your Cloudflare DNS settings are correct to point your subdomain to GitHub Pages:

1. **Go to Cloudflare dashboard** and select your domain.
2. **Go to the DNS settings**.
3. Add a `CNAME` record for your subdomain:
    - **Type**: CNAME
    - **Name**: `blog` (or whatever your subdomain is, such as `blog` or `shop`)
    - **Target**: `your-username.github.io`
    - **TTL**: Auto or 5 minutes (default)
4. Save the DNS record.

This points your subdomain to the GitHub Pages site, where Hugo is hosted.

### 3. **Ensure Proper `CNAME` File in Your GitHub Repository**

GitHub Pages requires a `CNAME` file in your repository to recognize your custom domain or subdomain.

1. In your GitHub repository, create a file named `CNAME` (no extension) in the **root** of the repository.
2. Inside the `CNAME` file, add your subdomain (e.g., `blog.example.com`).

Example content of the `CNAME` file:

```
blog.example.com
```

This will ensure that GitHub Pages serves your site under that subdomain.

### 4. **Check Your Hugo `theme` and `static` Folder Paths**

If styles are missing, it might be due to how Hugo generates paths to assets such as CSS or JavaScript. When deploying to a subdomain, it's important to ensure that all assets are being served correctly with the right paths.

- **Check your theme’s `static` folder**: Ensure that assets like CSS and JS are correctly placed in the `static` directory of your Hugo site. Hugo will copy anything inside the `static` folder to the root of your deployed site.
    
- **Verify the correct `href` and `src` paths**: In Hugo, you should be using Hugo’s built-in functions to generate URLs for assets, which ensures they work regardless of your domain or subdomain.
    

For example, for your CSS link:

```html
<link rel="stylesheet" href="{{ "/css/style.css" | relURL }}">
```

This uses `relURL`, which will correctly generate the path to the `style.css` file relative to your root domain or subdomain.

### 5. **Ensure Correct GitHub Pages Deployment Settings**

If you're deploying your Hugo site to GitHub Pages, ensure the repository settings are correctly configured:

1. **Go to your GitHub repository settings**.
2. Scroll down to the **GitHub Pages** section.
3. Ensure the **Source** is set to the correct branch (typically `main` or `gh-pages`).
4. GitHub should serve your site from the `public` directory after Hugo builds the site.

### 6. **Clear Cache and Check HTTPS Settings**

Sometimes, issues with missing styles can arise from cached assets or incorrect HTTPS handling.

- **Cloudflare Cache**: Go to **Cloudflare > Caching > Purge Everything** to ensure that the latest version of your site is being served.
    
- **SSL/TLS Settings**: Cloudflare forces HTTPS by default, which is good for security. Ensure that the **SSL/TLS** settings in Cloudflare are set to **Full** or **Full (Strict)** to avoid mixed content issues between HTTPS (Cloudflare) and HTTP (GitHub Pages).
    

### 7. **Check for Mixed Content Issues**

If you're using HTTPS (which you should), you may run into "mixed content" issues where some resources (like stylesheets or scripts) are being loaded over HTTP rather than HTTPS.

- **Open your site in the browser** and check the developer console (right-click > Inspect > Console tab) for any warnings or errors related to mixed content.
- If there are any HTTP resources, ensure all links in your Hugo site use HTTPS.

For example:

```html
<link rel="stylesheet" href="https://your-subdomain.example.com/css/style.css">
```

Or use Hugo’s `relURL` to generate proper links.

### 8. **Wait for DNS Propagation**

If you just made DNS changes, it may take some time (up to 24 hours) for DNS propagation to take full effect. Be patient, as changes may not appear immediately.

### 9. **Clear Browser Cache**

Browsers cache CSS and JavaScript, so if your styles aren’t loading properly, try clearing your browser cache or use an incognito window to load the site.

### In Summary:

- **Set `baseURL` correctly** in `config.toml` for Hugo.
- **Configure Cloudflare DNS** to point to GitHub Pages.
- **Create a `CNAME` file** in your GitHub repo with your subdomain.
- Ensure all **asset paths** in your Hugo templates are correct using `relURL`.
- **Check HTTPS settings** and ensure **no mixed content** (HTTP vs HTTPS).
- **Clear cache** in Cloudflare and your browser.

Once you make these changes, your Hugo site hosted on GitHub Pages should display correctly under your subdomain with all styles and assets loading as expected.