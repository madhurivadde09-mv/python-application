# AWS Setup — Free Account & ECR/ECS

## Step 1: Create AWS Account

1. Go to **https://aws.amazon.com**
2. Click **Create an AWS Account**
3. Enter **email**, choose **root user**, set **password**
4. Contact info: choose **Personal** or **Business**, fill details
5. Payment: add **credit/debit card** (required; you won't be charged unless you exceed free tier)
6. Identity verification: **phone** or **SMS**
7. Choose **Free** support plan
8. Confirm and sign in

> **Note:** New accounts get 12 months free tier. ECR: 500MB storage free. Fargate/ALB are pay-as-you-go. Use `terraform destroy` after testing to avoid ongoing charges.

---

## Step 2: Create IAM User (Don't use root for daily use)

1. Sign in to **AWS Console**
2. Search **IAM** → **Users** → **Create user**
3. User name: `github-actions` (or any name)
4. **Do not** check "Provide user access to the console" (we need programmatic access only)
5. Click **Next**
6. **Attach policies directly** → **Create policy** (opens new tab)
7. Choose **JSON** tab, paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecr:GetAuthorizationToken"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "ecr:PutImage",
        "ecr:InitiateLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:CompleteLayerUpload"
      ],
      "Resource": "arn:aws:ecr:*:*:repository/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ecs:UpdateService",
        "ecs:DescribeServices",
        "ecs:DescribeTaskDefinition",
        "ecs:RegisterTaskDefinition"
      ],
      "Resource": "*"
    }
  ]
}
```

8. **Next** → Policy name: `GitHubActionsECRECS` → **Create policy**
9. Go back to the user creation tab, **Refresh** policies, select `GitHubActionsECRECS`
10. **Next** → **Create user**

---

## Step 3: Create Access Keys for IAM User

1. **IAM** → **Users** → click `github-actions`
2. **Security credentials** tab → **Access keys** → **Create access key**
3. Use case: **Application running outside AWS**
4. **Next** → **Create access key**
5. **Copy** `Access key ID` and `Secret access key` — you won't see the secret again

---

## Step 4: Run Terraform (Creates ECR, ECS, etc.)

You need Terraform to create the ECR repo, ECS cluster, service, and related infra. Once that's done, you'll have:

- ECR repository name
- ECS cluster name
- ECS service name

Add these to GitHub secrets (Step 5).

> **Terraform scripts:** Create these in `infra/` (see DevOps assignment plan). We'll set this up in the next phase.

---

## Step 5: Add GitHub Secrets

1. Repo → **Settings** → **Secrets and variables** → **Actions**
2. **New repository secret** for each:

| Name | Value |
|------|-------|
| `AWS_ACCESS_KEY_ID` | From Step 3 |
| `AWS_SECRET_ACCESS_KEY` | From Step 3 |
| `AWS_REGION` | e.g. `us-east-1` |
| `ECR_REPOSITORY_NAME` | From Terraform output (e.g. `fastapi-app`) |
| `ECS_CLUSTER_NAME` | From Terraform output |
| `ECS_SERVICE_NAME` | From Terraform output |

---

## Step 6: Enable Deploy in Workflow

In `.github/workflows/deploy.yml`, change:

```yaml
if: false  # Set to true when AWS is ready
```

to:

```yaml
if: true
```

Then push. The `build-and-push` job will run and deploy to ECS.

---

## Order Summary

1. Create AWS account
2. Create IAM user + access keys
3. Run Terraform (creates ECR, ECS, VPC, ALB, etc.)
4. Add 6 secrets to GitHub
5. Set `if: true` in deploy.yml and push
