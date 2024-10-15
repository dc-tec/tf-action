# Terraform/OpenTofu/Terragrunt Runner Action

This GitHub Action allows you to run Terraform, OpenTofu, or Terragrunt commands with flexible options in your workflows. It uses a custom Docker image with `tenv` for version management, providing a consistent environment for your Infrastructure as Code (IaC) operations.

## Description

This action provides a flexible way to execute Terraform, OpenTofu, or Terragrunt commands in your GitHub Actions workflows. It supports:

- Running different versions of Terraform, OpenTofu, or Terragrunt
- Executing various commands (init, plan, apply, etc.)
- Specifying working directories
- Passing variable files and individual variables
- Adding extra command-line arguments

The action uses a custom Docker image that includes `tenv` for easy version management of the IaC tools.

## Inputs

### Required Inputs

- `program`: The program to run. Options are `tf` (Terraform), `tofu` (OpenTofu), or `tg` (Terragrunt).
- `command`: The command to run (e.g., init, plan, apply).

### Optional Inputs

- `version`: The version of the program to use. If not specified, the default version in the Docker image will be used.
- `working_directory`: The directory where the command should be executed. Default is the root of the repository (`.`).
- `var_file`: A comma-separated list of `.tfvars` files to use.
- `vars`: A JSON string of variables to pass to the command.
- `extra_args`: Additional arguments to pass to the command.

## Outputs

This action does not have any outputs. The result of the command execution will be visible in the workflow logs.

## Environment Variables

This action does not require any specific environment variables to be set.

## Secrets

This action does not require any secrets to be set. However, if your Terraform/OpenTofu/Terragrunt configuration requires authentication (e.g., cloud provider credentials), you should set these as secrets in your GitHub repository and pass them as environment variables in your workflow.

## Example Usage

Here's an example of how to use this action in your workflow:

```yaml
name: Terraform Plan

on: [push]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Terraform Plan
        uses: dc-tec/tf-action@v1
        with:
          program: tf
          version: 1.9.7
          command: plan
          working_directory: ./terraform
          var_file: prod.tfvars,common.tfvars
          vars: '{"region": "us-west-2", "instance_type": "t3.micro"}'
          extra_args: "-no-color -input=false"
          env:
            AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
            AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

This example runs a Terraform plan command using version 1.0.0, in the `./terraform` directory, with specified var files, additional variables, and extra arguments. It also passes AWS credentials as environment variables.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
