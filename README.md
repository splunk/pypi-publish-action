# pypi-publish-action
Repository for action publishing package to pypi

## Description
This action is based on poetry cli and serves only to build source and wheel distributions and publish them to pypi.
It requires poetry installed in environment.

## Inputs
| Input Parameter  | Required |
|:----------------:|:--------:|
|     pypi_username      |  true   |
|   pypi_token  |  true   |
                                                                           
## Example usage:

```yaml
# jobs section in GH actions  workflow file 
jobs:
 release:
    name: Release new version
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: false
          persist-credentials: false
      - name: Semantic release
        id: semantic
        uses: splunk/semantic-release-action@v1.2
        with:
          extra_plugins: |
            @google/semantic-release-replace-plugin
        env:
          GITHUB_TOKEN: ${{ secrets.GH_TOKEN_ADMIN }}
      - name: Install poetry
        run: |
          curl -sSL https://install.python-poetry.org | python3 -
      - name: Publish to pypi
        if: ${{ steps.semantic.outputs.new_release_published == 'true' }}
        uses: splunk/pypi-publish-action@v1.0
        with:
          pypi_username: ${{ secrets.PYPI_USERNAME }}
          pypi_token: ${{ secrets.PYPI_TOKEN }}
```

