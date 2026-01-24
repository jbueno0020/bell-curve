# GitHub Pages Deployment Guide

## 📋 Repository Structure ✅

Your repository is correctly configured with:

```
bell-curve/
├── index.html      ← Main application file (at root ✓)
├── .nojekyll       ← Prevents Jekyll processing (at root ✓)
└── bellcurve1      ← Legacy JS file (unused, safe to keep)
```

**Status**: ✅ Repository structure is correct - no nested directories!

---

## 🚀 GitHub Pages Configuration

### Step 1: Configure GitHub Pages Settings

1. Go to your repository: `https://github.com/jbueno0020/bell-curve`
2. Click **Settings** (in the top navigation)
3. Scroll down to **Pages** (in the left sidebar)
4. Under **Source**, configure:
   - **Branch**: `main`
   - **Folder**: `/ (root)`
5. Click **Save**

### Step 2: Wait for Deployment

- GitHub Pages will automatically build and deploy your site
- This usually takes 1-3 minutes
- You'll see a green checkmark when deployment succeeds

### Step 3: Access Your Site

Your site will be available at:
```
https://jbueno0020.github.io/bell-curve/
```

---

## 🔧 What Was Fixed

### Root Cause
The blank white screen was caused by:

1. **Missing Mount Flag**: The `window.__bellCurveAppMounted` flag was never set to `true`
   - This caused false error messages even when the app loaded successfully

2. **Short Timeout**: 1.5 second timeout was too short for:
   - Babel to transpile JSX code
   - React to render on slow connections
   - GitHub Pages CDN delivery

3. **Poor Error Diagnostics**: No way to know if CDN libraries failed to load

### Changes Made

✅ **Enhanced Error Detection**
- Added `checkDependencies()` to verify React, ReactDOM, and Babel loaded
- Shows specific error for each missing CDN library

✅ **Fixed Mount Detection**
- Set `window.__bellCurveAppMounted = true` when React starts rendering
- Prevents false "App failed to load" messages

✅ **Increased Timeout**
- Timeout increased from 1.5s → 3s
- Accommodates slow connections and Babel transpilation

✅ **Better Error Messages**
- Actionable troubleshooting steps
- Clear indication if it's a GitHub Pages issue
- Links to check CDN accessibility

---

## 🧪 Testing

### Test Locally
```bash
# Serve the site locally
python3 -m http.server 8000

# Open in browser
http://localhost:8000/
```

### Test on GitHub Pages
1. Wait for GitHub Actions to finish deploying
2. Visit: `https://jbueno0020.github.io/bell-curve/`
3. The app should load within 3 seconds
4. If you see an error, it will now show which CDN library failed

---

## 📦 File Structure Details

### index.html
- **Location**: Repository root ✅
- **Type**: Static HTML with inline React/Babel
- **Dependencies**: All loaded from CDN (unpkg.com)
  - React 18
  - ReactDOM 18
  - Babel Standalone
- **Assets**: No local assets required (fully self-contained)

### .nojekyll
- **Purpose**: Tells GitHub Pages to skip Jekyll processing
- **Required**: Yes (prevents GitHub from ignoring files starting with `_`)
- **Location**: Repository root ✅

### bellcurve1
- **Type**: JavaScript source file
- **Status**: Not referenced in index.html (legacy file)
- **Action**: Safe to keep or delete

---

## ❌ Common Issues & Solutions

### Issue: Blank white page with no error
**Cause**: CDN libraries (React/ReactDOM/Babel) failed to load
**Solution**:
- Check if unpkg.com is accessible
- Try accessing https://unpkg.com/react@18/umd/react.production.min.js directly
- Check browser console (F12) for network errors

### Issue: "App failed to load" with CDN errors
**Cause**: Network blocking unpkg.com or slow connection
**Solution**:
- Verify internet connection
- Check corporate firewall/VPN settings
- Try different network
- Clear browser cache and refresh

### Issue: Changes not showing on GitHub Pages
**Cause**: GitHub Pages cache or deployment delay
**Solution**:
- Wait 2-3 minutes for deployment to complete
- Check GitHub Actions tab for deployment status
- Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)
- Clear browser cache

---

## 📊 Verification Checklist

Before opening a GitHub issue, verify:

- [ ] Repository has `index.html` at root (not in subdirectory)
- [ ] Repository has `.nojekyll` file at root
- [ ] GitHub Pages is configured: `Settings → Pages → Source: main branch / (root)`
- [ ] GitHub Actions deployment succeeded (check Actions tab)
- [ ] Site URL is correct: `https://jbueno0020.github.io/bell-curve/`
- [ ] Browser console (F12) checked for errors
- [ ] unpkg.com is accessible from your network

---

## 🎯 Expected Behavior

### ✅ Success
- Page loads within 3 seconds
- Bell Curve Score Visualizer interface appears
- No error messages
- Console shows no errors

### ⚠️ Error (with diagnostics)
- After 3 seconds, error message appears
- Error lists which CDN libraries failed
- Troubleshooting steps displayed
- Console shows specific error details

---

## 🔗 Useful Links

- **Live Site**: https://jbueno0020.github.io/bell-curve/
- **Repository**: https://github.com/jbueno0020/bell-curve
- **GitHub Pages Docs**: https://docs.github.com/en/pages
- **CDN Status**: https://unpkg.com/

---

## 📝 Notes

1. **No build process required** - This is a static HTML site with inline React/Babel
2. **No dependencies to install** - All libraries loaded from CDN
3. **No gh-pages branch needed** - GitHub Pages serves directly from `main` branch
4. **No nested paths** - index.html is at repository root (verified ✅)

If you continue experiencing issues after following this guide, the problem is likely:
- Network/firewall blocking unpkg.com CDN
- GitHub Pages service disruption (check https://www.githubstatus.com/)
- Browser extension blocking scripts

---

*Last Updated: 2026-01-24*
*Commit: 04212c4 - Fix GitHub Pages blank screen*
