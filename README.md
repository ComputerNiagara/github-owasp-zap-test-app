# github-owasp-zap-test-app

Test repository for validating [github-owasp-templates](https://github.com/ComputerNiagara/github-owasp-templates) reusable workflows.

## What This Repo Tests

- ✅ ZAP Baseline scan triggered manually via `workflow_dispatch`
- ✅ Reusable workflow call from `ComputerNiagara/github-owasp-templates`
- ✅ Preflight connectivity check
- ✅ Issue creation from ZAP findings
- ✅ HTML report artifact upload

## How to Run

1. Go to **Actions → OWASP Baseline Scan → Run workflow**
2. Enter a target URL (use `http://testphp.vulnweb.com` if you don't have one ready)
3. Select environment: `dev`
4. Click **Run workflow**

## Where to Find Results

| Result | Location |
|--------|----------|
| Findings | Issues tab |
| HTML report | Actions → workflow run → Artifacts |
| Summary | Actions → workflow run → Summary |

## Safe Test URLs

These are intentionally vulnerable apps designed for scanner testing:

| URL | What it is |
|-----|-----------|
| `http://testphp.vulnweb.com` | OWASP PHP test app — ZAP's default demo target |
| `http://testfire.net` | IBM demo banking app |
