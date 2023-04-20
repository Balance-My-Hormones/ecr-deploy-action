# ecr-deploy-action

Simple composite action for deploying an image to AWS ECR

## Usage

For all possible inputs and description of inputs, see `action.yml`

The most common case will be to tag and deploy an image by pushing a git tag (or creating a release
that creates a git tag)

```yaml
on:
  push:
    tags: # This is just an example tag pattern. You can use whatever pattern you want.
      - v[0-9]+.[0-9]+.[0-9]+

name: Deploy to Amazon ECR in the dev environment

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2
    
    - name: build
      run: |
        # You implement this as necessary. This is a simple example
        docker build -t "${{ github.ref_name }}"  .
    - name: push ecr
      uses: Deep-Consulting-Solutions/ecr-deploy-action@main
      with:
        service-key: 'n8n' # or whatever the reusable service key is
        account: ${{ secrets.DCS_AWS_ACCOUNT }}
        source-image: ${{ github.ref_name }}
        tag: ${{ github.ref_name }}
    

```