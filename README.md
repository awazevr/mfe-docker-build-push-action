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

**Required** AWS Access Key Id

### `aws-secret-access-key`

**Required** AWS Secret Access Key

```tsv
10011	IGNORE	(Cookie Without Secure Flag)
10015	IGNORE	(Incomplete or No Cache-control and Pragma HTTP Header Set)
```

### `aws-region`

**Required** The AWS Region to deploy to


### `bit-token`

**Required** The Bit Dev token 

### `pat-token`

**Required** Github Personal Access Token

### `ecr-repository`

**Required** Specifies ECR to push docker image

### `build-only`

**Required** Build flag which indicates if only the build stage is required and not the push docker image action



## Usage
You can use this composite Action in your own workflow by adding:

```yml
name: search-mfe-composite

on:
  push:
    branches: [ feature/MFE-3 ]

  workflow_dispatch:

jobs:
  build_and_push_image:
    environment: feature
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
          ecr-repository: dev-age-search-mfe
          builfd-only: true

```

