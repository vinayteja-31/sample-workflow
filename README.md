# sample-workflow

Minimal Docker image (nginx + static HTML) used to test the **Deployment Workflow API** from [support-service-api-mgmt](https://github.com/tower-cloud/support-service-api-mgmt) (`POST /api/deployments/execute`).

## Push this folder to your own GitHub repo

This repo is intended to live as **its own** GitHub repository (e.g. [vinayteja-31/sample-workflow](https://github.com/vinayteja-31/sample-workflow.git)), not only as a subfolder of the gateway.

From the monorepo, copy only this folder (or push it as the root of the remote):

```bash
cd sample-workflow
git init
git add .
git commit -m "Initial sample-workflow for deploy API testing"
git branch -M main
git remote add origin https://github.com/vinayteja-31/sample-workflow.git
git push -u origin main
```

If you already have an empty remote, the above works after `git init` inside `sample-workflow`.

## GitHub Actions (optional)

Workflow: `.github/workflows/deploy-via-api-mgmt.yaml`

Configure in **this** repo (Settings → Secrets and variables → Actions):

| Type | Name | Purpose |
|------|------|--------|
| Variable | `DEPLOY_GATEWAY_URL` | Base URL of support-service-api-mgmt (no trailing slash) |
| Secret | `DEPLOY_ORG_ID` | IAM |
| Secret | `DEPLOY_IAM_USERNAME` | IAM |
| Secret | `DEPLOY_IAM_PASSWORD` | IAM |
| Secret | `DEPLOY_SERVICE_NAME` | Logical service name in deploy JSON |
| Secret | `DEPLOY_CONTAINER_NAME` | Container name passed to container mgmt (`PUT /api/containers/{name}`) |

Run **Actions → Deploy via API Gateway (manual)** after secrets are set.

The workflow builds and pushes to **GHCR** (`ghcr.io/<owner>/sample-workflow:<sha>`), then calls your gateway to deploy that image.

## Local test (Docker only)

```bash
docker build --platform=linux/amd64 -t sample-workflow:local .
docker run --rm -p 8080:80 sample-workflow:local
# Open http://localhost:8080
```

## Related

- Curl reference in gateway repo: `curl-refs/deployment-workflow-curl-reference.txt`
