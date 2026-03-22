# Terraform Setup Action

Composite GitHub Action for Terraform operations — setup, init, and common CI tasks.

## Features

- Automatically detects and installs the correct Terraform version using [tfswitch](https://tfswitch.warrensbox.com/) (reads `required_version` from `.tf` files)
- Configures Terraform Cloud credentials
- Initializes Terraform workspace

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_actions_checkout"></a> [actions/checkout](https://github.com/actions/checkout) | 4 |
| <a name="requirement_tfswitch"></a> [tfswitch](https://tfswitch.warrensbox.com/) | latest |

## Inputs

| Name | Description | Required | Default |
| --- | --- | --- | --- |
| <a name="input_working_directory"></a> [working-directory](#input_working_directory) | The working directory containing Terraform files | true | `null` |
| <a name="input_environment"></a> [environment](#input_environment) | The Terraform Cloud workspace name | true | `null` |

## Environment Variables

| Name | Description |
| --- | --- |
| `TF_TOKEN_app_terraform_io` | Terraform Cloud API token. Set via GitHub secret: `${{ secrets.TF_API_TOKEN }}` |

## Outputs

`No Outputs`

## Usage

Here's an example of how to use this action in a GitHub Actions workflow:

```yaml
name: Terraform CI

on:
  push:
    branches: [ main, dev ]
  pull_request:
    branches: [ main ]
  workflow_dispatch:

jobs:
  plan:
    runs-on: ubuntu-latest

    steps:
      - uses: mbasri-actions/terraform-setup@v1
        with:
          working-directory: 'quick-aws-ec2'
          environment: 'dev'
        env:
          TF_TOKEN_app_terraform_io: ${{ secrets.TF_API_TOKEN }}

      - name: Terraform plan
        run: terraform plan -no-color
        working-directory: quick-aws-ec2
        env:
          TF_WORKSPACE: dev
          TF_TOKEN_app_terraform_io: ${{ secrets.TF_API_TOKEN }}
```

## Author

* [**Mohamed BASRI**](https://github.com/mbasri)

## License

This is free and unencumbered software released into the public domain. See the [LICENSE](./LICENSE) file for details.
