# SURL.CF - Simple, Fast URL Shortener

[中文](README_ZH.md) | [English](README.md)

SURL.CF is a URL shortening service built with Next.js and Cloudflare Pages. It allows users to convert long URLs into short links for easier sharing and use. It supports custom short links, clipboard copying, responsive design, Cloudflare KV for link storage, optional human verification with Cloudflare Turnstile, a configurable announcement system, an admin console, and GitHub integration.

## Features

- Quickly convert long URLs into short links
- Support for custom short links (e.g., surl.cf/my-product)
- Copy to clipboard functionality
- Responsive design for all devices
- Cloudflare KV for link data storage
- Optional human verification with Cloudflare Turnstile
- Configurable announcement system with title, Markdown content, and different notification types
- Admin console for managing and analyzing short links
- GitHub repository integration
- Deployed on Cloudflare Pages for global fast access
- Multilingual support (English and Chinese)

## Local Development

1. Clone the repository:

```bash
git clone https://github.com/siiway/surl.cf.git
cd surl.cf
```

2. Make sure you have Node.js version 18.18.0 or higher installed. This project requires Node.js ^18.18.0 || ^19.8.0 || >= 20.0.0 for Next.js 15.3.2 to work properly.

   The project includes `.node-version` and `.nvmrc` files that specify Node.js 20.11.1. If you use nvm, you can run:

   ```bash
   nvm use
   ```

3. Install dependencies:

```bash
npm install
```

4. Run the development server:

```bash
npm run dev
```

5. Open [http://localhost:3000](http://localhost:3000) in your browser to see the result.

## Deployment to Cloudflare Pages

1. Create a KV namespace in your Cloudflare dashboard, named `LINKS_KV`.

2. Set KV namespace IDs as environment variables in your Cloudflare Pages project settings:

```toml
name = "your-project-name"
compatibility_date = "2025-05-13"
pages_build_output_dir = ".next"

# Global variables and bindings (for local development)
[vars]
API_KEY = "your-api-key"

# Global KV namespace configuration
[[kv_namespaces]]
binding = "LINKS_KV"
id = "${KV_ID}"

# Production environment configuration
[env.production.vars]
API_KEY = "your-production-api-key"

# Production KV namespace configuration
[[env.production.kv_namespaces]]
binding = "LINKS_KV"
id = "${KV_ID}"

# Preview environment configuration
[env.preview.vars]
API_KEY = "your-preview-api-key"

# Preview KV namespace configuration
[[env.preview.kv_namespaces]]
binding = "LINKS_KV"
id = "${KV_PREVIEW_ID}"
```

**Note:** Configure the Node.js version (20) in your Cloudflare Pages project settings, not in wrangler.toml.

To get the KV ID and PREVIEW ID:

**Using Cloudflare Dashboard:**

- Go to Cloudflare Dashboard > Workers & Pages > KV
- Create two namespaces: one for production (e.g., `LINKS_KV`) and one for preview (e.g., `LINKS_KV_PREVIEW`)
- Copy the IDs of both namespaces

**Using Wrangler CLI:**

- Install Wrangler: `npm install -g wrangler`
- Login: `wrangler login`
- Create namespaces:

```bash
wrangler kv:namespace create "LINKS_KV"
wrangler kv:namespace create "LINKS_KV_PREVIEW" --preview
```

- The command output will show the IDs you need

Then set them as environment variables in your Cloudflare Pages project settings:

- `KV_ID`: Your production KV namespace ID
- `KV_PREVIEW_ID`: Your preview KV namespace ID (used for development and preview deployments)

3. Create a site in [Cloudflare Turnstile](https://www.cloudflare.com/products/turnstile/) to get a Site Key and Secret Key.

4. Set Turnstile keys as environment variables in your Cloudflare Pages project settings:

```toml
[env.production]
NODE_ENV = "production"
NEXT_PUBLIC_TURNSTILE_SITE_KEY = "${TURNSTILE_SITE_KEY}"
TURNSTILE_SECRET_KEY = "${TURNSTILE_SECRET_KEY}"
```

5. Deploy using Cloudflare Pages:
   - Connect your GitHub repository
   - Set the build command to `npm run build`
   - Set the output directory to `.next`
   - For Framework preset, select **Next.js**
   - Set Node.js version to **20.11.1** (required for Next.js 15+, matches the `.node-version` file)
   - Add the environment variable `NODE_ENV=production`

6. After deployment, your URL shortening service will be available at the domain provided by Cloudflare Pages.

## Tech Stack

- [Next.js](https://nextjs.org) - React framework
- [Tailwind CSS](https://tailwindcss.com) - CSS framework
- [Cloudflare Pages](https://pages.cloudflare.com) - Hosting service
- [Cloudflare KV](https://developers.cloudflare.com/workers/runtime-apis/kv) - Key-value storage
- [Cloudflare Turnstile](https://www.cloudflare.com/products/turnstile/) - Human verification service
- [Jose](https://github.com/panva/jose) - JWT implementation library

## Admin Console

SURL.CF includes an admin console that can be accessed by visiting the `/admin` path. The default admin credentials are:

- Username: `admin`
- Password: `admin123`

**Important Note:** In a production environment, be sure to change the default credentials and use a strong password and secure JWT key.

Admin console features:

1. **Dashboard** - View link statistics
2. **Link Management** - View, search, and delete short links
3. **Settings** - Manage account settings and system configuration, including enabling/disabling human verification

To configure admin credentials and other settings, set the following environment variables in your Cloudflare Pages project settings:

```env
JWT_SECRET                      # Key for JWT signing
ADMIN_USERNAME                  # Admin username
ADMIN_PASSWORD                  # Admin password
NEXT_PUBLIC_ENABLE_TURNSTILE    # Whether to enable Turnstile (true/false)
NEXT_PUBLIC_ANNOUNCEMENT_TITLE  # Announcement title
NEXT_PUBLIC_ANNOUNCEMENT_CONTENT # Announcement content (supports Markdown)
NEXT_PUBLIC_ANNOUNCEMENT_TYPE   # Announcement type (info/warning/success/error)
NEXT_PUBLIC_GITHUB_URL          # GitHub repository link
```

Example values:

```env
JWT_SECRET = "your-super-secret-jwt-key"
ADMIN_USERNAME = "your-username"
ADMIN_PASSWORD = "your-password"
NEXT_PUBLIC_ENABLE_TURNSTILE = "true"
NEXT_PUBLIC_ANNOUNCEMENT_TITLE = "Welcome to SURL.CF"
NEXT_PUBLIC_ANNOUNCEMENT_CONTENT = "This is a **simple, fast** URL shortening service!"
NEXT_PUBLIC_ANNOUNCEMENT_TYPE = "info"
NEXT_PUBLIC_GITHUB_URL = "https://github.com/siiway/surl.cf"
```

## Open Source License

This project is licensed under the GNU General Public License v3.0.

For more information, see the [LICENSE](LICENSE) file.

## Language Support

This project supports both English and Chinese. For the Chinese version of this README, please see [README_ZH.md](README_ZH.md).
