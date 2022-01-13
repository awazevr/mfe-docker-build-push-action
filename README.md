# mfe-docker-build-push-action
This is a GitHub Action meant to be used as a [composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) within an existing workflow. This action encapsulates setting up a checkout, build, test and push a docker image to an Elastic Container Register (ECR) in one step. 

The action encapsulates the following other actions:

- [actions/checkout](https://github.com/actions/checkout)
- [benjlevesque/short-sha](https://github.com/benjlevesque/short-sha)
- [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)
- [aws-actions/amazon-ecr-login](https://github.com/aws-actions/amazon-ecr-login)
- [docker/build-push-action](https://github.com/docker/build-push-action)



## Inputs

### `aws-access-key-id`

**Required** target API definition, OpenAPI or SOAP, local file or URL, e.g. https://www.example.com/openapi.json
or target endpoint URL, GraphQL, e.g. https://www.example.com/graphql

### `aws-secret-access-key`

**Required** You can also specify a relative path to the rules file to ignore any alerts from the ZAP scan. Make sure to create
the rules file inside the relevant repository. The following shows a sample rules file configuration.
Make sure to checkout the repository (actions/checkout@v2) to provide the ZAP rules to the scan action.

```tsv
10011	IGNORE	(Cookie Without Secure Flag)
10015	IGNORE	(Incomplete or No Cache-control and Pragma HTTP Header Set)
```

### `aws-region`

**Required** The title for the GitHub issue to be created.


### `bit-token`

**Required** By default ZAP Docker container will fail with an [exit code](https://github.com/zaproxy/zaproxy/blob/7abbd57f6894c2abf4f1ed00fb95e99c34ef2e28/docker/zap-api-scan.py#L35),
if it identifies any alerts. Set this option to `true` if you want to fail the status of the GitHub Scan if ZAP identifies any alerts during the scan.

### `pat-token`

**Required** By default ZAP Docker container will fail with an [exit code](https://github.com/zaproxy/zaproxy/blob/7abbd57f6894c2abf4f1ed00fb95e99c34ef2e28/docker/zap-api-scan.py#L35),
if it identifies any alerts. Set this option to `true` if you want to fail the status of the GitHub Scan if ZAP identifies any alerts during the scan.




## Usage
You can use this composite Action in your own workflow by adding:

```yml
name: search-mfe-composite

on:
  push:
    branches: [ feature/MFE-3 ]
  # pull_request:
  #   branches: [ master ]

  workflow_dispatch:

jobs:
  build_and_push_image:
    environment: feature
    env:
      ECR_REPOSITORY: dev-age-search-mfe
    runs-on: ubuntu-latest

    steps:
      - name: Checkout, Build and Push Docker Image
        uses: awazevr/docker-build-push-action@v1.0.20
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-west-2
          bit-token: ${{ secrets.BIT_TOKEN }}
          pat-token: ${{ secrets.PAT_TOKEN}}

```

