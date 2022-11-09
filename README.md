# github-reusable-actions

Inspired by [favedom-dev/github-reusable-workflow](https://github.com/favedom-dev/github-reusable-workflow)

---

## Service Principal

Create the Service Principal with script [`sp-create.sh`](./scripts/setup/sp-create.sh)

Get the `++OBJECT_ID_SERVICE_PRINCIPAL++`

```bash
az ad sp list --display-name "sp-acr-shipmagic"
```

Note: the resource group must be the same that the Container Registry is in

```bash
az role assignment create --assignee "++OBJECT_ID_SERVICE_PRINCIPAL++" \
--role "AcrPush" \
--resource-group "dev-staging-sm-rg-registry"
```

---

Each repo has that uses these worklows has the secrets set

- `GH_USERNAME`
  - only used for maven `settings.xml`
  - the username that matches `GH_TOKEN`
- `GH_TOKEN`
- `REGISTRY_USERNAME`
  - Service Principal account ID (not name)
- `REGISTRY_PASSWORD`
  - Service Principal account password

Docker registry login server: `jxshipmagic.azurecr.io`

---
