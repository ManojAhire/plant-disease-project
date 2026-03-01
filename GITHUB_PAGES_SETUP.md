# GitHub Pages Deployment Guide

## Architecture

This project deploys to two separate platforms:

- **Frontend**: GitHub Pages (static files from `/static` folder)
- **Backend API**: Vercel (FastAPI server at `https://plant-disease-project.vercel.app`)

## Setup Instructions

### 1. Enable GitHub Pages in Repository Settings

1. Go to your GitHub repository: `https://github.com/ManojAhire/plant-disease-project`
2. Click **Settings** → **Pages**
3. Under "Build and deployment":
   - **Source**: Select "GitHub Actions"
   - This enables automatic deployment on every commit to `main`

### 2. Configure Backend URL

The frontend automatically detects and uses the correct API URL:
- **Local development** (`localhost`): Uses `http://localhost:8000`
- **GitHub Pages production**: Uses `https://plant-disease-project.vercel.app`

To change the backend URL, edit [static/index.html](static/index.html) line 19:

```javascript
apiURL: 'https://your-backend-url.vercel.app'  // Update this
```

### 3. Automatic Deployment

The repository includes a GitHub Actions workflow (`.github/workflows/deploy.yml`) that:
- Triggers on every push to `main` branch
- Deploys the `/static` folder to GitHub Pages
- Takes ~1-2 minutes to complete

View deployment status:
- Go to **Actions** tab in GitHub
- Look for the "Deploy to GitHub Pages" workflow

### 4. Access Your App

Once enabled, your app will be available at:

```
https://ManojAhire.github.io/plant-disease-project/
```

### 5. Features

✅ **Offline Mode**: Works without internet using on-device TensorFlow.js model
✅ **Online Mode**: Sends images to Vercel backend for faster, more accurate predictions  
✅ **PWA Support**: Install as mobile app via manifest.json
✅ **Service Worker**: Offline functionality and caching

## Troubleshooting

### Deployment Fails
- Check the **Actions** tab for error messages
- Ensure all files in `/static` are correctly formatted
- Make sure your Vercel backend is running

### API Requests Fail
- Check browser console (F12 → Console tab)
- Verify Vercel backend is online: `https://plant-disease-project.vercel.app/predict`
- Ensure CORS is enabled on the backend (should be configured in `app.py`)

### Models Not Loading
- Check that model files exist in `/static/models/`
- Verify model.json and .bin files are committed to git
- Check browser's Network tab for failed requests

## Optional: Custom Domain

To use a custom domain instead of `github.io`:

1. In **Settings** → **Pages**, add your domain under "Custom domain"
2. Update your DNS records according to GitHub's instructions
3. The workflow will automatically handle HTTPS

## File Structure

```
plant-disease-project/
├── .github/workflows/
│   └── deploy.yml              # GitHub Pages workflow
├── static/                      # What gets deployed to GitHub Pages
│   ├── index.html              # Main app
│   ├── js/
│   │   ├── app.js              # Main controller
│   │   ├── online.js           # Backend API integration
│   │   └── ...
│   ├── models/                 # TensorFlow.js model files
│   └── css/
├── app.py                       # Vercel backend (FastAPI)
├── requirements.txt             # Python dependencies
└── vercel.json                 # Vercel configuration
```

## Environment Variables

No environment variables needed! The configuration is hardcoded based on hostname detection.

If you need to override this behavior, create a `config.json` in the `/static` folder with:

```json
{
  "apiURL": "https://your-custom-backend-url.vercel.app"
}
```

Then update `static/js/online.js` to load this config file.
