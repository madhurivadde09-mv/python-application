# GitHub Setup — Step-by-Step

## Step 1: Create repo on GitHub

1. Go to https://github.com/new
2. Repository name: `fastapi-app` (or your choice)
3. Visibility: Public (for free GitHub Actions minutes)
4. **Do not** add README, .gitignore, or license (we already have these)
5. Click **Create repository**

---

## Step 2: Add files locally (already done)

Your project now has:

```
fastapi-app/
├── app/
│   ├── __init__.py
│   └── main.py
├── tests/
│   ├── __init__.py
│   └── test_main.py
├── .github/
│   └── workflows/
│       └── deploy.yml
├── .gitignore
├── Dockerfile
├── requirements.txt
└── readme.md
```

---

## Step 3: Init git and push

```bash
cd fastapi-app

# Init repo
git init

# Add remote (replace YOUR_USERNAME and YOUR_REPO with your GitHub repo)
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git

# Stage and commit
git add .
git commit -m "Initial commit: FastAPI app with CI/CD"

# Push (creates main branch if it doesn't exist)
git branch -M main
git push -u origin main
```

---

## Step 4: Verify workflow

1. Go to your repo on GitHub
2. Click **Actions**
3. You should see a run for the push you just made
4. **Lint + test** job should pass
5. **Build-and-push** job will fail until you add AWS secrets (expected — we do that in the AWS phase)

---

## Step 5: GitHub secrets (for later — AWS phase)

When you set up AWS, add these in **Settings → Secrets and variables → Actions**:

| Secret | Description |
|--------|-------------|
| `AWS_ACCESS_KEY_ID` | IAM access key |
| `AWS_SECRET_ACCESS_KEY` | IAM secret key |
| `AWS_REGION` | e.g. `us-east-1` |
| `ECR_REPOSITORY_NAME` | ECR repo name (from Terraform) |
| `ECS_CLUSTER_NAME` | ECS cluster name |
| `ECS_SERVICE_NAME` | ECS service name |

---

## Quick reference

| Action | Command |
|--------|---------|
| Check status | `git status` |
| Add changes | `git add .` |
| Commit | `git commit -m "message"` |
| Push | `git push` |
