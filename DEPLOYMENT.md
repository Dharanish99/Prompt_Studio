# 🚀 Full Stack Deployment Guide (MERN)

This guide walks you through deploying the **Prompt Enhancer** project "in one piece" using **Render** for the Backend and **Vercel** for the Frontend.

## 🏗️ Architecture Overview
- **Database**: MongoDB Atlas
- **Backend**: Render (Web Service)
- **Frontend**: Vercel (Static Site)

---

## 📋 Prerequisites
1.  **GitHub Repository**: Ensure your code is pushed (Done).
2.  **Accounts**:
    - [Render.com](https://render.com)
    - [Vercel.com](https://vercel.com)
    - [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
    - Google Cloud Console (for OAuth)
    - GitHub Developer Settings (for OAuth)
    - Groq API Key

---

## 🛠️ Step 1: Database Setup (MongoDB Atlas)
1.  Ensure your Cluster is running.
2.  **Network Access**: Allow access from running services.
    - Go to **Network Access** > **Add IP Address**.
    - Select **Allow Access From Anywhere** (`0.0.0.0/0`) (Required for Render).
3.  **Database Access**: Ensure you have a database user (e.g., `superadmin`) with read/write privileges.
4.  **Connection String**: Get your URI: `mongodb+srv://<username>:<password>@cluster0...`

---

## ⚙️ Step 2: Deploy Backend (Render)
*We deploy the backend first to generate the API URL.*

1.  **Create Service**:
    - Log in to Render.
    - Click **New +** > **Web Service**.
    - Connect your `Prompt_Studio` repository.
2.  **Configuration**:
    - **Name**: `prompt-enhancer-api` (or similar)
    - **Root Directory**: `server` (Important!)
    - **Runtime**: `Node`
    - **Build Command**: `npm install`
    - **Start Command**: `npm start`
3.  **Environment Variables**:
    Add the following keys (copy values from your local `.env` or see `RENDER_DEPLOYMENT_GUIDE.md`):

    | Key | Value Note |
    | :--- | :--- |
    | `NODE_ENV` | `production` |
    | `PORT` | `5000` |
    | `MONGO_URI` | Your MongoDB connection string |
    | `SESSION_SECRET` | A long random string |
    | `CLIENT_URL` | `https://temp-placeholder.com` (Update this later after Frontend deploy) |
    | `GOOGLE_CLIENT_ID` | From Google Cloud Console |
    | `GOOGLE_CLIENT_SECRET` | From Google Cloud Console |
    | `GITHUB_CLIENT_ID` | From GitHub settings |
    | `GITHUB_CLIENT_SECRET` | From GitHub settings |
    | `GROQ_API_KEY` | Your Groq API Key |
    | *(Add other GROQ keys)* | Add all specific `GROQ_API_KEY_*` variants |

4.  **Deploy**: Click **Create Web Service**.
5.  **Copy URL**: Once live, copy your backend URL (e.g., `https://prompt-enhancer-api.onrender.com`).

---

## 🎨 Step 3: Deploy Frontend (Vercel)
1.  **Import Project**:
    - Log in to Vercel.
    - Click **Add New...** > **Project**.
    - Import `Prompt_Studio`.
2.  **Configuration**:
    - **Framework Preset**: Create React App
    - **Root Directory**: Click "Edit" and select `client`.
    - **Build Command**: `npm run build` (Default)
3.  **Environment Variables**:
    - Add the following variable so the frontend knows where the backend is:

    | Key | Value |
    | :--- | :--- |
    | `REACT_APP_API_URL` | `https://prompt-enhancer-api.onrender.com` (Your Render URL) |

4.  **Deploy**: Click **Deploy**.
5.  **Copy URL**: Once live, copy your frontend URL (e.g., `https://prompt-studio.vercel.app`).

---

## 🔗 Step 4: Final Connection (CORS & OAuth)
*Now that we have the Frontend URL, we must update the Backend to allow it.*

1.  **Update Backend CORS**:
    - Go back to **Render** > Your Service > **Environment**.
    - Update `CLIENT_URL` to your new Vercel URL (e.g., `https://prompt-studio.vercel.app`).
    - **Save Changes** (Render will automatically redeploy).

2.  **Update OAuth Callbacks**:
    - **Google Console**:
        - Add Authorized Redirect URI: `https://<YOUR-RENDER-BACKEND-URL>/auth/google/callback`
    - **GitHub Developer Settings**:
        - Update Authorization Callback URL: `https://<YOUR-RENDER-BACKEND-URL>/auth/github/callback`

---

## ✅ Verification
1.  Open your Vercel URL.
2.  Try to **Login** (tests OAuth + Database).
3.  Try to **Enhance a Prompt** (tests API + Groq + Session).

---

## 🆘 Troubleshooting
- **CORS Errors**: Double-check `CLIENT_URL` in Render matches your Vercel URL exactly (no trailing slash).
- **Build Fails**: Check logs. If `client` build fails on Vercel, ensure you selected `client` as Root Directory.
- **API Errors**: Check Render logs for "MongoDB Connected" and ensure IP Whitelist in Atlas includes `0.0.0.0/0`.
