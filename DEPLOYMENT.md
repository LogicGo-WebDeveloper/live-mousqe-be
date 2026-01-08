# cPanel Deployment Guide

## Problem
The error `Cannot find module 'express'` occurs because:
1. Dependencies are not installed (`node_modules` missing)
2. TypeScript code is not compiled (`dist` folder missing)
3. Environment variables are not configured

## Solution Steps

### Step 1: Upload and Extract Files
1. Upload your zip file to cPanel File Manager
2. Extract the zip file in your application directory (e.g., `/home/username/livemosque-node/`)

### Step 2: Install Dependencies
In cPanel Terminal or SSH, navigate to your application directory and run:
```bash
cd /home/ya0ltyc05vhu/livemosque-node
npm install
```

This will install all dependencies including `express` and other required packages.

### Step 3: Build TypeScript Code
Compile your TypeScript code to JavaScript:
```bash
npm run build
```

This creates the `dist` folder with compiled JavaScript files.

### Step 4: Create Environment File
Create a `.env` file in your application root directory with the following variables:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
ADMIN_EMAIL=your_admin_email@example.com
ADMIN_PASSWORD=your_secure_password
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
```

**Important:** Replace the placeholder values with your actual credentials.

### Step 5: Configure Node.js App in cPanel
1. Go to cPanel → **Setup Node.js App**
2. Select your application
3. Set the following:
   - **Node.js version**: Select the latest stable version (v18+ recommended)
   - **Application mode**: Production
   - **Application root**: `/home/ya0ltyc05vhu/livemosque-node`
   - **Application URL**: Your domain/subdomain
   - **Application startup file**: `dist/index.js` OR use `npm start`
   - **Passenger startup file**: Leave empty or set to `dist/index.js`

### Step 6: Set Environment Variables in cPanel
In the Node.js App setup, add environment variables:
- `PORT=5000`
- `MONGO_URI=your_mongodb_connection_string`
- `ADMIN_EMAIL=your_admin_email@example.com`
- `ADMIN_PASSWORD=your_secure_password`
- `CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name`
- `CLOUDINARY_API_KEY=your_cloudinary_api_key`
- `CLOUDINARY_API_SECRET=your_cloudinary_api_secret`

### Step 7: Restart Application
Click **"Restart App"** in cPanel Node.js App setup.

## Alternative: Using Terminal/SSH

If you have SSH access, you can run these commands:

```bash
# Navigate to your app directory
cd /home/ya0ltyc05vhu/livemosque-node

# Install dependencies
npm install

# Build the application
npm run build

# Start the application (for testing)
npm start
```

## Troubleshooting

### If you still get "Cannot find module" errors:
1. Make sure `node_modules` folder exists: `ls -la node_modules`
2. Reinstall dependencies: `rm -rf node_modules package-lock.json && npm install`
3. Verify Node.js version matches: `node --version` (should be v18+)

### If the app doesn't start:
1. Check if `dist` folder exists: `ls -la dist`
2. Rebuild: `npm run build`
3. Check logs in cPanel → Node.js App → View Logs

### If MongoDB connection fails:
1. Verify `MONGO_URI` is set correctly in environment variables
2. Check if MongoDB allows connections from your server IP
3. Test connection string format: `mongodb://username:password@host:port/database`

## Required Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `MONGO_URI` | Yes | MongoDB connection string |
| `ADMIN_EMAIL` | Yes | Admin user email |
| `ADMIN_PASSWORD` | Yes | Admin user password |
| `PORT` | No | Server port (default: 5000) |
| `CLOUDINARY_CLOUD_NAME` | No | Cloudinary cloud name (has default) |
| `CLOUDINARY_API_KEY` | No | Cloudinary API key (has default) |
| `CLOUDINARY_API_SECRET` | No | Cloudinary API secret (has default) |

## Notes
- Never commit `.env` file to version control
- The `dist` folder is generated during build and should not be in git
- Always run `npm install` and `npm run build` after uploading code
- Make sure Node.js version in cPanel matches your development environment





