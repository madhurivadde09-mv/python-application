# Rotate AWS Access Keys & Fix Build-and-Push

## Step 1: Deactivate old key (AWS Console)

1. AWS Console → IAM → Users → your user (e.g. github-actions-ecr)
2. Security credentials tab → Access keys
3. Click Actions → Deactivate on the old key
4. Then Actions → Delete (once you confirm you don't need it)

## Step 2: Create new key (same user)

1. Same page: Security credentials → Create access key
2. Use case: Application running outside AWS
3. Next → Create access key
4. Copy Access key ID and Secret access key (save securely, won't be shown again)

## Step 3: Update GitHub secrets

1. Repo → Settings → Secrets and variables → Actions
2. Update these two (click pencil icon):
   - AWS_ACCESS_KEY_ID → paste new key ID
   - AWS_SECRET_ACCESS_KEY → paste new secret key
3. Verify these also exist and are correct:
   - AWS_REGION → ap-southeast-2
   - ECR_REPOSITORY_NAME → github-repo (must match your ECR repo name exactly)

## Step 4: Update local .env (optional, for validate script)

1. Open .env in project root
2. Replace AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY with new values
3. Run: python3 scripts/validate_aws.py (restore from .restore if deleted)

## Step 5: Push and trigger workflow

```
git add .
git commit -m "Fix docker build for ECR push"
git push
```

## Checklist

- [ ] Old key deactivated/deleted in IAM
- [ ] New key created (same user)
- [ ] GitHub secrets updated (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY)
- [ ] AWS_REGION = ap-southeast-2
- [ ] ECR_REPOSITORY_NAME = github-repo (exact match)
- [ ] Local .env updated (if using validate script)
- [ ] Code pushed, workflow triggered
