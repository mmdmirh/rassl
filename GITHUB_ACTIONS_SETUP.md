# GitHub Actions Automatic Deployment Setup

## ✅ What Has Been Created

A GitHub Actions workflow that automatically deploys your website to the server whenever you push to the `main` branch.

## 🔑 Required: Add GitHub Secrets

You need to add 4 secrets to your GitHub repository:

### Step 1: Go to GitHub Repository Settings

1. Go to: https://github.com/mmdmirh/rassl
2. Click on **Settings** (top right)
3. In the left sidebar, click **Secrets and variables** → **Actions**
4. Click **New repository secret**

### Step 2: Add These 4 Secrets

#### Secret 1: SSH_PRIVATE_KEY
- **Name**: `SSH_PRIVATE_KEY`
- **Value**: Copy the entire content from `/Users/mohamad/CascadeProjects/rassl-website/gitlab-ci-key`
  ```bash
  cat /Users/mohamad/CascadeProjects/rassl-website/gitlab-ci-key
  ```
- Copy the entire output including `-----BEGIN` and `-----END` lines

#### Secret 2: SERVER_IP
- **Name**: `SERVER_IP`
- **Value**: `130.113.70.212`

#### Secret 3: SERVER_USER
- **Name**: `SERVER_USER`
- **Value**: `mirhos5`

#### Secret 4: SERVER_PATH
- **Name**: `SERVER_PATH`
- **Value**: `/var/www/rassl-website`

### Step 3: Save Each Secret

Click **Add secret** after entering each one.

---

## 🚀 How It Works

### Automatic Deployment

Once secrets are added:

1. **You make changes** to your website locally
2. **Commit and push** to GitHub:
   ```bash
   git add .
   git commit -m "Update website"
   git push origin main
   ```
3. **GitHub Actions automatically**:
   - Checks out your code
   - Connects to the server via SSH
   - Deploys files using rsync
   - Fixes permissions and SELinux
   - Restarts Nginx
   - ✅ Your website is live!

### Manual Deployment

You can also trigger deployment manually:

1. Go to: https://github.com/mmdmirh/rassl/actions
2. Click on **Deploy RASSL Website** workflow
3. Click **Run workflow** → **Run workflow**

---

## 📊 Monitoring Deployments

### View Deployment Status

1. Go to: https://github.com/mmdmirh/rassl/actions
2. See all deployment runs and their status
3. Click on any run to see detailed logs

### Deployment Notifications

GitHub will show:
- ✅ Green checkmark if deployment succeeds
- ❌ Red X if deployment fails
- 🟡 Yellow dot while deployment is running

---

## 🔄 Workflow

```
┌─────────────────┐
│  Make Changes   │
│  to Website     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  git add .      │
│  git commit     │
│  git push       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ GitHub Actions  │
│ Triggered       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Deploy to       │
│ Server          │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Website Live!   │
│ http://IP       │
└─────────────────┘
```

---

## 🛠️ Testing the Setup

After adding secrets, test the workflow:

1. Make a small change to `index.html`
2. Commit and push:
   ```bash
   git add index.html
   git commit -m "Test automatic deployment"
   git push origin main
   ```
3. Go to https://github.com/mmdmirh/rassl/actions
4. Watch the deployment run
5. Check http://130.113.70.212 to see your changes

---

## 📝 Manual Deployment (Backup)

If GitHub Actions is not working, you can still deploy manually:

```bash
cd /Users/mohamad/CascadeProjects/rassl-github
git pull origin main
rsync -rltvz --no-perms --no-owner --no-group \
  --exclude='.git' --exclude='*.py' \
  ./ mirhos5@130.113.70.212:/var/www/rassl-website/
ssh -t mirhos5@130.113.70.212 "bash fix-selinux.sh"
```

---

## 🐛 Troubleshooting

### Workflow fails with "Permission denied (publickey)"
→ SSH_PRIVATE_KEY secret is not set correctly or is missing

### Workflow fails at "Fix permissions"
→ Server password required for sudo. This is expected - the files are deployed but permissions might not be fixed. Run manually:
```bash
ssh -t mirhos5@130.113.70.212 "bash fix-selinux.sh"
```

### Changes not appearing on website
→ Clear browser cache: `Ctrl+Shift+R` (Windows/Linux) or `Cmd+Shift+R` (Mac)

---

## 🔐 Security Notes

- ✅ SSH private key is stored securely in GitHub Secrets
- ✅ Secrets are encrypted and never exposed in logs
- ✅ Only repository admins can view/edit secrets
- ✅ Workflow only runs on push to `main` branch

---

## 📋 Quick Reference

| Item | Value |
|------|-------|
| **Website URL** | http://130.113.70.212 |
| **GitHub Repo** | https://github.com/mmdmirh/rassl |
| **Actions Page** | https://github.com/mmdmirh/rassl/actions |
| **Workflow File** | `.github/workflows/deploy.yml` |

---

**Setup Date**: October 24, 2025  
**Status**: Ready for automatic deployment after adding secrets
