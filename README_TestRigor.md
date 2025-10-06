# 🧪 TestRigor Integration with GitHub Actions

This repository demonstrates how to integrate **TestRigor** automated tests with **GitHub Actions CI/CD**.

---

## 📁 Project Structure
```
.
├── .github/
│   └── workflows/
│       └── testrigor.yml     # GitHub Actions workflow file
├── README.md                 # This file
```

---

## 🚀 How It Works

Each time you push code (or open a pull request), the GitHub Actions workflow triggers a **TestRigor test run** automatically using your **TestRigor project**.

---

## ⚙️ Setup Instructions

### 1. Get Your TestRigor API Token
1. Log in to your [TestRigor account](https://app.testrigor.com/).
2. Go to **User Settings → API Access Token**.
3. Copy your token.

### 2. Add It to GitHub Secrets
1. Go to your GitHub repository → **Settings → Secrets → Actions**.
2. Click **New Repository Secret**.
3. Name: `TESTRIGOR_AUTH_TOKEN`
4. Value: paste your copied token.

### 3. Example GitHub Workflow (`.github/workflows/testrigor.yml`)

```yaml
name: Run TestRigor Tests

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  run-tests:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run TestRigor tests
        env:
          TESTRIGOR_AUTH_TOKEN: ${{ secrets.TESTRIGOR_AUTH_TOKEN }}
        run: |
          curl -X POST "https://api.testrigor.com/api/v1/projects/<YOUR_PROJECT_ID>/tests/run"           -H "Authorization: Bearer $TESTRIGOR_AUTH_TOKEN"           -H "Content-Type: application/json"           -d '{"test_suite_name": "Main Regression Suite"}'
```

> ⚠️ Replace `<YOUR_PROJECT_ID>` with your actual TestRigor project ID from your TestRigor dashboard.

---

## ✅ Verify Success

Once configured:
- Go to your GitHub repo → **Actions** tab.
- Run the workflow manually or push a new commit.
- You’ll see logs showing whether the TestRigor test run passed or failed.

---

## 🧩 Troubleshooting

| Issue | Cause | Fix |
|-------|--------|-----|
| `Unauthorized access` | Invalid or missing `TESTRIGOR_AUTH_TOKEN` | Recheck your API token and GitHub Secret name |
| `401 error` | Wrong project ID or token | Verify both values |
| No test run triggered | API call incorrect | Double-check your curl URL and payload |

---

## 💡 Helpful Links
- [TestRigor Docs](https://help.testrigor.com/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
