# 🦖 Deploy Docusaurus to AWS S3 (Amazon Web Services S3)

A GitHub Action that builds your Docusaurus site and syncs it to an Amazon S3 bucket. This action handles the complete deployment process: installing dependencies, building the site, and uploading to S3.

## Features

- ✅ Installs dependencies with Yarn
- ✅ Builds your Docusaurus site
- ✅ Syncs build output to S3 with `--exact-timestamps` and `--delete`
- ✅ Works with both Docusaurus v2 and v3
- ✅ Supports custom build directories

## Usage

### `workflow.yml` Example

Place this in `.github/workflows/deploy.yml` in your repository. [Refer to the documentation on workflow YAML syntax here.](https://help.github.com/en/articles/workflow-syntax-for-github-actions)

```yaml
name: 🦖 Deploy Docusaurus to AWS S3
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: yarn

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Deploy to S3
        uses: docuactions/aws-s3@main
        with:
          aws-region: us-east-1
          aws-s3-bucket: ${{ secrets.AWS_S3_BUCKET }}
```

## Configuration

### Inputs

| Input           | Description                                      | Required | Default   |
|-----------------|--------------------------------------------------|----------|-----------|
| `aws-region`    | AWS region where your S3 bucket is located       | Yes      | -         |
| `aws-s3-bucket` | Name of the S3 bucket to deploy to               | Yes      | -         |
| `build-dir`     | Build output directory (relative to source-dir)  | No       | `build`   |
| `source-dir`    | Source directory containing your Docusaurus site | No       | `.`       |

### Required Secrets

You must configure these as [encrypted secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) in your repository:

| Secret                    | Description                                                                                                  |
|---------------------------|--------------------------------------------------------------------------------------------------------------|
| `AWS_ACCESS_KEY_ID`       | Your AWS Access Key. [More info here.](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) |
| `AWS_SECRET_ACCESS_KEY`   | Your AWS Secret Access Key. [More info here.](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) |
| `AWS_S3_BUCKET`           | Your S3 bucket name (can also be passed directly as input if you prefer) |

### Prerequisites

1. **Node.js**: Use `actions/setup-node@v4` to set up Node.js (v18+ for Docusaurus v3)
2. **AWS Credentials**: Use `aws-actions/configure-aws-credentials` to configure AWS access
3. **S3 Bucket**: Your bucket should be configured for static website hosting
4. **IAM Permissions**: Your AWS user needs `s3:PutObject`, `s3:DeleteObject`, and `s3:ListBucket` permissions

## Credits
* [Brock Davis](https://github.com/brockneedscoffee)