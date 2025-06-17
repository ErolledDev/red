# SEO Redirection System - Next.js SSR Application

A Next.js Server-Side Rendered application for creating SEO-optimized redirections with custom meta tags to boost content visibility in search engines.

## 🚨 IMPORTANT: AWS Amplify SSR Setup

This application requires **Server-Side Rendering (SSR)** to work properly. The API routes and dynamic file operations will NOT work with static deployment.

### Step 1: Enable SSR in AWS Amplify Console

1. Go to your AWS Amplify app dashboard
2. Click on "App settings" → "Build settings"
3. **CRITICAL**: Make sure your app is configured for **SSR (Server-Side Rendering)**
4. If you see "Static Web Hosting", you need to change it to "SSR"

### Step 2: Verify SSR Configuration

In your Amplify console, check:
- **Platform**: Should show "Web" with SSR support
- **Framework**: Should detect "Next.js SSR"
- **Build command**: Should be `npm run build` (not `npm run export`)
- **Start command**: Should be `npm run start`

### Step 3: Deploy with SSR

The `amplify.yml` file is configured for SSR. Make sure:
- No `output: 'export'` in next.config.js ✅
- API routes are enabled ✅
- Server-side file operations are supported ✅

### Step 4: Test Dynamic Functionality

After deployment, test these features:
1. **Admin Panel**: `/admin` - Should allow creating/editing redirects
2. **API Routes**: `/api/get-redirects` - Should return JSON data
3. **Dynamic Pages**: `/your-slug` - Should load with server-rendered meta tags
4. **File Operations**: Creating redirects should update `redirects.json`

## 🔧 Troubleshooting SSR Issues

### Issue: "API routes not working"
**Solution**: Your app is deployed as static. Redeploy with SSR configuration.

### Issue: "Cannot read/write redirects.json"
**Solution**: File operations require SSR. Check Amplify console for SSR support.

### Issue: "Build fails with SSR"
**Solution**: Make sure `next.config.js` doesn't have `output: 'export'`.

### Issue: "Pages are static, not dynamic"
**Solution**: Verify Amplify is using `npm run start`, not serving static files.

## ✅ SSR Deployment Checklist

Before deploying, ensure:
- [ ] `next.config.js` has NO `output: 'export'`
- [ ] `package.json` has `"start": "next start"`
- [ ] `amplify.yml` is configured for SSR
- [ ] AWS Amplify console shows "SSR" not "Static"
- [ ] Node.js version is 18+ in Amplify settings

## 🚀 How SSR Works in This App

### Server-Side Features:
- **Dynamic API Routes**: `/api/*` routes run as serverless functions
- **File Operations**: `redirects.json` is read/written server-side
- **Meta Tag Generation**: SEO tags generated on each request
- **Real-time Data**: Changes appear immediately without rebuild

### File Structure (SSR):
```
├── app/
│   ├── api/                 # Serverless API routes (SSR)
│   │   ├── create-redirect/ # Creates/updates redirects.json
│   │   ├── get-redirects/   # Reads redirects.json
│   │   └── sitemap/         # Generates dynamic sitemap
│   ├── [slug]/             # Dynamic SSR pages
│   └── admin/              # Admin interface (SSR)
├── redirects.json          # Dynamic data file (SSR writable)
└── amplify.yml             # SSR build configuration
```

## 🔍 Verifying SSR Deployment

After deployment, check:

1. **API Test**: Visit `https://your-app.amplifyapp.com/api/get-redirects`
   - Should return JSON data, not 404

2. **Admin Test**: Visit `https://your-app.amplifyapp.com/admin`
   - Should load admin interface and allow creating redirects

3. **Dynamic Test**: Create a redirect, then visit the generated URL
   - Should show custom meta tags in page source

4. **File Test**: Check if new redirects persist after creation
   - Should save to `redirects.json` permanently

## 📊 SSR vs Static Comparison

| Feature | SSR (Required) | Static (Won't Work) |
|---------|----------------|---------------------|
| API Routes | ✅ Works | ❌ 404 Error |
| File Operations | ✅ Dynamic | ❌ Read-only |
| Admin Panel | ✅ Functional | ❌ Cannot save |
| Meta Tags | ✅ Server-rendered | ❌ Client-only |
| Real-time Updates | ✅ Immediate | ❌ Requires rebuild |

## 🛠️ Local Development

1. **Clone and install**:
   ```bash
   git clone <your-repo>
   cd nextjs-seo-redirection-system
   npm install
   ```

2. **Environment setup**:
   ```bash
   cp .env.example .env.local
   # Edit NEXT_PUBLIC_BASE_URL for local development
   ```

3. **Run in SSR mode**:
   ```bash
   npm run dev    # Development server
   npm run build  # Production build
   npm run start  # Production SSR server
   ```

## 🔐 Security (SSR)

- **Server-side validation**: All API routes validate input server-side
- **File system security**: Restricted to `redirects.json` operations only
- **XSS protection**: Server-side HTML escaping
- **CSRF protection**: Built-in Next.js protection

## 📈 Performance (SSR)

- **Server-side rendering**: Faster initial page loads
- **API route caching**: Efficient serverless function execution
- **Dynamic imports**: Code splitting for optimal performance
- **Image optimization**: Next.js Image component with SSR

---

**⚠️ REMEMBER**: This app MUST be deployed with SSR enabled in AWS Amplify. Static deployment will break all dynamic functionality!

Built with ❤️ using Next.js SSR, TypeScript, and Tailwind CSS