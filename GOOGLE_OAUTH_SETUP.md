# Google OAuth Setup Guide

This guide walks you through setting up Google OAuth for user authentication in the DHMAD platform.

## Overview

Google OAuth is used in three places:
1. **Frontend** (main website) - User login/registration
2. **Developer Portal** - Developer login/registration
3. **Backend** - Verifying Google OAuth tokens

Each environment (development, production, sandbox) may need separate OAuth credentials.

---

## Step 1: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click **"Select a project"** → **"New Project"**
3. Enter project name (e.g., "DHMAD Platform")
4. Click **"Create"**
5. Wait for project creation, then select it

---

## Step 2: Enable Google+ API

1. In Google Cloud Console, navigate to **APIs & Services** → **Library**
2. Search for **"Google+ API"** or **"People API"**
3. Click on it and click **"Enable"**
4. Wait for the API to be enabled

**Note:** Google+ API is being deprecated. You may need to use **Google Identity Services** instead, but the People API should still work for basic profile information.

---

## Step 3: Create OAuth 2.0 Credentials

1. Navigate to **APIs & Services** → **Credentials**
2. Click **"+ CREATE CREDENTIALS"** → **"OAuth client ID"**
3. If prompted, configure the OAuth consent screen first:
   - Choose **"External"** (unless you have a Google Workspace)
   - Fill in required fields:
     - **App name**: DHMAD
     - **User support email**: Your email
     - **Developer contact information**: Your email
   - Click **"Save and Continue"**
   - Add scopes: `email`, `profile`, `openid`
   - Add test users (if in testing mode)
   - Click **"Save and Continue"** → **"Back to Dashboard"**

4. Now create OAuth Client ID:
   - **Application type**: Web application
   - **Name**: Choose a descriptive name (e.g., "DHMAD Frontend Development")

---

## Step 4: Configure Authorized JavaScript Origins

In the OAuth client configuration, under **"Authorized JavaScript origins"**, add:

### For Development:
```
http://localhost:3000
http://localhost:3001
http://127.0.0.1:3000
http://127.0.0.1:3001
```

### For Production:
```
https://dhmad.tn
https://www.dhmad.tn
https://developer.dhmad.tn
https://www.developer.dhmad.tn
```

### For Sandbox:
```
https://sandbox.dhmad.tn
https://www.sandbox.dhmad.tn
https://developer.sandbox.dhmad.tn
https://www.developer.sandbox.dhmad.tn
```

**Important:**
- Include protocol (`http://` or `https://`)
- Include port number for localhost
- **No trailing slashes**
- **No paths** (just origin)

---

## Step 5: Configure Authorized Redirect URIs

Under **"Authorized redirect URIs"**, add:

### For Development:
```
http://localhost:3000
http://localhost:3001
http://127.0.0.1:3000
http://127.0.0.1:3001
```

### For Production:
```
https://dhmad.tn
https://www.dhmad.tn
https://developer.dhmad.tn
https://www.developer.dhmad.tn
```

### For Sandbox:
```
https://sandbox.dhmad.tn
https://www.sandbox.dhmad.tn
https://developer.sandbox.dhmad.tn
https://www.developer.sandbox.dhmad.tn
```

**Important Notes:**
- The redirect URI must be the **origin only** (protocol + domain + port)
- **Do NOT** include any path (like `/login` or `/callback`)
- The `@react-oauth/google` library automatically uses the origin as the redirect URI
- Make sure there's **no trailing slash**

---

## Step 6: Save and Copy Credentials

1. Click **"Create"**
2. You'll see a popup with:
   - **Client ID** (copy this)
   - **Client Secret** (copy this - you won't see it again!)
3. Save both securely

---

## Step 7: Configure Environment Variables

### Frontend (Main Website)

#### Development (`frontend/.env.development`):
```env
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_google_client_id_here
```

#### Production (`frontend/.env.production`):
```env
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_production_google_client_id_here
```

#### Sandbox Development (`frontend/.env.sandbox.development`):
```env
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_sandbox_dev_google_client_id_here
```

#### Sandbox Production (`frontend/.env.sandbox.production`):
```env
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_sandbox_prod_google_client_id_here
```

### Developer Portal

#### Development (`developer-portal/.env.development`):
```env
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_google_client_id_here
```

#### Production (`developer-portal/.env.production`):
```env
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_production_google_client_id_here
```

**Note:** Developer portal typically uses the same Client ID as the frontend for the same environment.

### Backend

#### All Environments (`backend/.env.*`):
```env
GOOGLE_CLIENT_ID=your_google_client_id_here
```

**Note:** Backend uses the same Client ID across environments, or you can use separate ones per environment.

---

## Step 8: Multiple OAuth Clients (Recommended)

For better security and separation, create separate OAuth clients for each environment:

### Option A: One Client Per Environment
- **Development Client**: `http://localhost:3000`, `http://localhost:3001`
- **Production Client**: `https://dhmad.tn`, `https://developer.dhmad.tn`
- **Sandbox Client**: `https://sandbox.dhmad.tn`, `https://developer.sandbox.dhmad.tn`

### Option B: Shared Client (Simpler)
- One client with all origins/redirects listed above

---

## Step 9: Wait for Propagation

After saving changes in Google Cloud Console:
- **Wait 5-10 minutes** for changes to propagate
- Changes are usually instant, but can take up to 10 minutes

---

## Step 10: Test the Setup

### Frontend:
1. Start the frontend: `npm run dev` (or appropriate command)
2. Navigate to login page
3. Click "Sign in with Google"
4. Should redirect to Google login
5. After login, should redirect back to your app

### Developer Portal:
1. Start the developer portal: `npm run dev`
2. Navigate to login/register page
3. Click "Sign in with Google"
4. Should work the same way

---

## Troubleshooting

### Error: "redirect_uri_mismatch"

**Cause:** The redirect URI in your request doesn't match what's configured in Google Cloud Console.

**Fix:**
1. Check browser console (F12) for the exact redirect URI being used
2. Verify it matches exactly in Google Cloud Console (case-sensitive, no trailing slash)
3. Wait a few minutes after making changes
4. Clear browser cache

### Error: "invalid_client"

**Cause:** Client ID is incorrect or doesn't exist.

**Fix:**
1. Verify `NEXT_PUBLIC_GOOGLE_CLIENT_ID` matches the Client ID in Google Cloud Console
2. Check for typos or extra spaces
3. Restart your development server after changing env variables

### Error: "access_denied"

**Cause:** User denied permission or OAuth consent screen not configured.

**Fix:**
1. Configure OAuth consent screen in Google Cloud Console
2. Add required scopes: `email`, `profile`, `openid`
3. If in testing mode, add test users

### Changes Not Taking Effect

1. **Wait 5-10 minutes** - Google's changes can take time to propagate
2. **Clear browser cache** - Cached OAuth settings can cause issues
3. **Restart development server** - Environment variables are loaded at startup
4. **Check environment file** - Make sure you're editing the correct `.env` file
5. **Verify file location** - `.env` files should be in the project root

---

## Complete Configuration Example

### Google Cloud Console Configuration:

**Authorized JavaScript origins:**
```
http://localhost:3000
http://localhost:3001
https://dhmad.tn
https://www.dhmad.tn
https://developer.dhmad.tn
https://www.developer.dhmad.tn
https://sandbox.dhmad.tn
https://www.sandbox.dhmad.tn
https://developer.sandbox.dhmad.tn
https://www.developer.sandbox.dhmad.tn
```

**Authorized redirect URIs:**
```
http://localhost:3000
http://localhost:3001
https://dhmad.tn
https://www.dhmad.tn
https://developer.dhmad.tn
https://www.developer.dhmad.tn
https://sandbox.dhmad.tn
https://www.sandbox.dhmad.tn
https://developer.sandbox.dhmad.tn
https://www.developer.sandbox.dhmad.tn
```

---

## Security Best Practices

1. **Never commit credentials** - Use `.env` files and add them to `.gitignore`
2. **Use separate clients per environment** - Better security isolation
3. **Rotate credentials regularly** - Especially if compromised
4. **Use HTTPS in production** - Never use HTTP for production OAuth
5. **Restrict origins** - Only add origins you actually use
6. **Monitor usage** - Check Google Cloud Console for suspicious activity
7. **Keep Client Secret secure** - Only use server-side, never expose in frontend code

---

## Quick Reference Checklist

- [ ] Google Cloud project created
- [ ] Google+ API or People API enabled
- [ ] OAuth consent screen configured
- [ ] OAuth 2.0 Client ID created
- [ ] Authorized JavaScript origins added
- [ ] Authorized redirect URIs added
- [ ] Client ID and Secret copied
- [ ] Environment variables set in frontend
- [ ] Environment variables set in developer-portal
- [ ] Environment variables set in backend
- [ ] Waited 5-10 minutes for propagation
- [ ] Tested login on frontend
- [ ] Tested login on developer portal
- [ ] Verified redirect URIs match exactly

---

## Additional Resources

- [Google OAuth 2.0 Documentation](https://developers.google.com/identity/protocols/oauth2)
- [Google Cloud Console](https://console.cloud.google.com/)
- [React OAuth Google Library](https://www.npmjs.com/package/@react-oauth/google)
