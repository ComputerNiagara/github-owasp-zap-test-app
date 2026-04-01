# github-owasp-zap-test-app

Test repository for validating [ComputerNiagara/github-owasp-templates](https://github.com/ComputerNiagara/github-owasp-templates) reusable workflows and composite actions end-to-end.

---

## What This Repo Tests

- ✅ ZAP Baseline scan triggered manually via `workflow_dispatch`
- ✅ ZAP Baseline scan triggered automatically via fake deployment pipeline
- ✅ Reusable workflow call from `ComputerNiagara/github-owasp-templates`
- ✅ Composite action calls (`create-deployment`, `update-deployment`)
- ✅ Preflight connectivity check
- ✅ Issue creation from ZAP findings
- ✅ HTML report artifact (`zap_scan`)

---

## Workflows

### `owasp-baseline-scan.yml`

The ZAP baseline scan workflow. Has three trigger paths:

| Trigger | When it fires | Notes |
|---------|--------------|-------|
| `workflow_dispatch` | Manual — Actions tab | Always works |
| `deployment_status` | After successful deployment | ⚠️ Does not fire reliably on personal GitHub.com — see note below |
| `repository_dispatch` | Explicitly triggered by `fake-deploy.yml` | TEST ONLY — workaround for personal GitHub limitation |

### `fake-deploy.yml`

Simulates a deployment to test the automatic scan trigger. Does not deploy anything real — just creates a GitHub Deployment record, waits 5 seconds, reports success, then fires a `repository_dispatch` to trigger the ZAP scan.

---

## How to Run

### Manual scan
1. **Actions → OWASP Baseline Scan → Run workflow**
2. Enter a target URL or leave the default (`http://testphp.vulnweb.com`)
3. Select environment: `dev`
4. Click **Run workflow**

### Automatic scan (via fake deploy)
1. **Actions → Fake Deploy to DEV → Run workflow**
2. Click **Run workflow** — no inputs needed
3. Watch `Fake Deploy to DEV` complete
4. `OWASP Baseline Scan` triggers automatically via `repository_dispatch`

---

## Where to Find Results

| Result | Location |
|--------|----------|
| Findings | **Issues** tab |
| HTML report | Actions → workflow run → **Artifacts** → `zap_scan` |
| Scan summary | Actions → workflow run → **Summary** |

---

## Known Limitation — Personal GitHub.com

On personal GitHub.com accounts, the `deployment_status` event does not fire
reliably when deployments are created by `github-actions[bot]`. This is a
personal account limitation and **does not affect GitHub Enterprise** where
`deployment_status` fires correctly.

The `repository_dispatch` trigger in this repo is a workaround for testing
purposes only. In enterprise, `deployment_status` is the correct and
sufficient approach — no `repository_dispatch` needed.

---

## Safe Test URLs

These are intentionally vulnerable apps designed for security scanner testing:

| URL | What it is |
|-----|-----------|
| `http://testphp.vulnweb.com` | OWASP PHP test app — ZAP's default demo target |
| `http://testfire.net` | IBM demo banking app |
| `https://juice-shop.herokuapp.com` | OWASP Juice Shop |
