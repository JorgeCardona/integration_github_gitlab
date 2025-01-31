```git
git remote add gitlab https://gitlab.com/jorgecardona/integration_github_gitlab.git
git push gitlab --all
12345
```

```yaml
name: Sync with GitLab

on:
  push:
    branches:
      - main
      - dev
      - qa
      - uat
      - prod

jobs:
  sync:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout GitHub repo
        uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Sync with GitLab
        run: |
          # Añadir GitLab como remoto
          git remote add gitlab https://gitlab-ci-token:${{ secrets.GITLAB_TOKEN }}@gitlab.com/jorgecardona/integration_gitlab.git

          # Obtener los últimos cambios de GitLab
          git fetch gitlab

          # Sincronizar ramas
          if [ "${GITHUB_REF##*/}" == "main" ]; then
            git push gitlab main:main
          if [ "${GITHUB_REF##*/}" == "dev" ]; then
            git push gitlab dev:dev
          elif [ "${GITHUB_REF##*/}" == "qa" ]; then
            git push gitlab qa:qa
          elif [ "${GITHUB_REF##*/}" == "uat" ]; then
            git push gitlab uat:uat
          elif [ "${GITHUB_REF##*/}" == "prod" ]; then
            git push gitlab prod:prod
          fi
```
