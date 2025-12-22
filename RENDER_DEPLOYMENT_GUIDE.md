# 🚀 Render Production Deployment Guide

## ✅ BACKEND PRODUCTION READINESS CHECKLIST

### 1. Authentication & Sessions
- [x] Passport.js OAuth (Google + GitHub) configured
- [x] Session-based auth with MongoDB store
- [x] Production-ready cookie settings (`secure: true`, `sameSite: "none"`)
- [x] Trust proxy enabled for Render
- [x] No Clerk remnants found

### 2. Security & CORS
- [x] Helmet security headers enabled
- [x] CORS configured for cross-origin frontend
- [x] Environment-based CORS origins
- [x] HTTP-only cookies for session security

### 3. Database & Models
- [x] MongoDB Atlas connection ready
- [x] User model with OAuth providers
- [x] History model for prompt storage
- [x] Template model for remix functionality

### 4. API Routes & Controllers
- [x] `/auth/*` - OAuth login/logout
- [x] `/api/prompts/*` - Image prompt enhancement
- [x] `/api/text/*` - Text prompt enhancement (3 variations)
- [x] `/api/templates/*` - Template remix functionality
- [x] `/api/history/*` - User history retrieval
- [x] All routes protected with Passport session auth

### 5. AI Services
- [x] Groq API integration for all engines
- [x] Visual prompt enhancement (single optimized)
- [x] Text prompt enhancement (logical, creative, optimized)
- [x] Template remix with protected tokens
- [x] JSON validation and error handling

## 🔧 RENDER DEPLOYMENT STEPS

### Step 1: Create Render Web Service
1. Connect your GitHub repository
2. Choose "Web Service"
3. Set build command: `cd server && npm install`
4. Set start command: `cd server && npm start`
5. Set environment: `Node`

### Step 2: Environment Variables
Copy all variables from `.env.production` to Render's Environment Variables:

```
NODE_ENV=production
PORT=5000
CLIENT_URL=https://your-frontend-app.onrender.com
MONGO_URI=YOUR_MONGO_URI
SESSION_SECRET=YOUR_SESSION_SECRET
GOOGLE_CLIENT_ID=YOUR_GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET=YOUR_GOOGLE_CLIENT_SECRET
GITHUB_CLIENT_ID=YOUR_GITHUB_CLIENT_ID
GITHUB_CLIENT_SECRET=YOUR_GITHUB_CLIENT_SECRET
SMTP_EMAIL=dharanishrp@gmail.com
SMTP_APP_PASSWORD=YOUR_SMTP_APP_PASSWORD
GROQ_API_KEY=YOUR_GROQ_API_KEY
GROQ_API_KEY_VISUALS=YOUR_GROQ_API_KEY_VISUALS
GROQ_API_KEY_REMIX=YOUR_GROQ_API_KEY_REMIX
GROQ_API_KEY_TEXT_AUDIT=YOUR_GROQ_API_KEY_TEXT_AUDIT
GROQ_API_KEY_TEXT_ENHANCE=YOUR_GROQ_API_KEY_TEXT_ENHANCE
GROQ_API_KEY_TEXT_SIMULATE=YOUR_GROQ_API_KEY_TEXT_SIMULATE
```

### Step 3: Update OAuth Callback URLs

#### Google OAuth Console:
- Add: `https://your-backend-app.onrender.com/auth/google/callback`

#### GitHub OAuth App:
- Add: `https://your-backend-app.onrender.com/auth/github/callback`

### Step 4: Update CLIENT_URL
After frontend deployment, update `CLIENT_URL` in Render environment variables.

## 🧪 TESTING ENDPOINTS

### Health Check
```bash
curl https://your-backend-app.onrender.com/
# Expected: "Prompt Studio API is healthy and running..."
```

### Authentication Test
```bash
# Visit in browser:
https://your-backend-app.onrender.com/auth/google
# Should redirect to Google OAuth
```

### API Test (after login)
```bash
curl -X POST https://your-backend-app.onrender.com/api/text/enhance \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Write a story about AI"}' \
  --cookie-jar cookies.txt
```

## 🔍 VERIFICATION CHECKLIST

After deployment, verify:

- [ ] Health endpoint returns 200
- [ ] Google OAuth login works
- [ ] GitHub OAuth login works
- [ ] Session cookies are set correctly
- [ ] `/api/history` returns user history
- [ ] `/api/text/enhance` returns 3 prompt variations
- [ ] `/api/prompts/image-enhance` returns single enhanced prompt
- [ ] `/api/templates` returns template list
- [ ] CORS allows frontend requests

## 🚨 TROUBLESHOOTING

### Common Issues:

1. **CORS Errors**: Update `CLIENT_URL` environment variable
2. **OAuth Fails**: Check callback URLs in OAuth providers
3. **Session Issues**: Verify `trust proxy` and cookie settings
4. **Database Errors**: Check MongoDB Atlas IP whitelist (allow all: 0.0.0.0/0)
5. **API Errors**: Check Groq API key limits and quotas

### Debug Logs:
Check Render logs for:
- `✅ MongoDB Connected`
- `✅ All routes mounted successfully`
- `🚀 Server running on port 5000`

## 📋 FINAL PRODUCTION VERIFICATION

The backend is production-ready when:
1. All OAuth flows work end-to-end
2. Sessions persist across requests
3. All API endpoints return expected data
4. History saves correctly for authenticated users
5. Prompt enhancement returns proper JSON structure
6. No Clerk references exist in codebase
7. All environment variables are properly set