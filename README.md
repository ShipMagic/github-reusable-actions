# github-reusable-actions

Inspired by [favedom-dev/github-reusable-workflow](https://github.com/favedom-dev/github-reusable-workflow)

---

## Information

- [`shipmagic-commons`](https://github.com/ShipMagic/shipmagic-commons) packages are stored in [GitHub Packages](https://github.com/orgs/ShipMagic/packages)
- Docker registry login server: [`jxshipmagic.azurecr.io`](https://portal.azure.com/#@cblackburnlive.onmicrosoft.com/resource/subscriptions/b7965add-088b-48e5-863b-924167a26e54/resourceGroups/dev-staging-sm-rg-registry/providers/Microsoft.ContainerRegistry/registries/jxshipmagic/overview)

---

## Service Principal

Create the Service Principal with script [`sp-create.sh`](./scripts/setup/sp-create.sh) and save the `Service principal ID` and `Service principal password`.  These are used later for each repo.

Get the `++OBJECT_ID_SERVICE_PRINCIPAL++`

```bash
az ad sp list --display-name "sp-acr-shipmagic"
```

**Note:** The resource group must match the Container Registry's resource group

```bash
az role assignment create --assignee "++OBJECT_ID_SERVICE_PRINCIPAL++" \
--role "AcrPush" \
--resource-group "dev-staging-sm-rg-registry"
```

---

## GitHub repo secrets adn Azure Key Vault

Azure Key Vault [`jxshipmagic`](https://portal.azure.com/#@cblackburnlive.onmicrosoft.com/resource/subscriptions/b7965add-088b-48e5-863b-924167a26e54/resourceGroups/dev-staging-sm-rg-cluster/providers/Microsoft.KeyVault/vaults/jxshipmagic/secrets)

Each repo has that uses these worklows has the secrets set

| GitHub SECRET | Azure key Vault SECRET | INFO | NOTES |
| --- | --- | --- | --- |
| `GH_USERNAME` | `GH-USERNAME` | - only used for maven `settings.xml` <br /> - the username that matches `GH_TOKEN` | |
| `GH_TOKEN` | `GH-TOKEN` | Generated token for `GH_USERNAME` | needs to be able to write packages for `shipmagic-commons` |
| `REGISTRY_USERNAME` | `REGISTRY-USERNAME` | Service Principal account ID (not name) | `az ad sp list --display-name sp-acr-shipmagic --query "[].appId" --output tsv` |
| `REGISTRY_PASSWORD` | `REGISTRY-PASSWORD` | Service Principal account password | refer to the saved `Service principal password`. |

