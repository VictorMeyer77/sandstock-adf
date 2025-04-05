# sandstock-adf

This repository manages Azure Data Factory for sandstock, a sandbox Azure project designed to gather and maintain all best practices in an Azure solution.

- **Terraform repository:** [sandstock-infra](https://github.com/VictorMeyer77/sandstock-infra)
- **Web app repository:** [sandstock](https://github.com/VictorMeyer77/sandstock)

## Branching Model

To enhance reliability, overcome ARM template limitations, and harmonize Data Factory components, the following elements are managed using Terraform:

- Dataset
- Integration Runtime
- Linked Service
- Managed Virtual Network

Only the "pipeline" folder can be updated via the Azure Data Factory interface.

This repository have two principal branches: `main` and `dev`. Both are the same, except the main branch is tokenized with environment variables like {{VARIABLE_NAME}}.

1. When you run `terraform apply`, all Data Factory components are created, except for the pipelines.
2. Via the Azure Data Factory interface, you can import ADF resources into the dev branch.
3. Create a new branch based on the dev branch and create or update pipelines in your working branch.
4. Create a pull request between your working branch and the dev branch and merge it.
5. Create a release branch based on the dev branch and replace all your environment variable with {{VARIABLE_NAME}}
6. Create a pull request between the release branch and the main branch and merge it.
7. With GitHub Actions, publish the new pipeline.
8. To destroy the infrastructure using the `terraform destroy` command, you should remove all pipelines. This can also be achieved using GitHub Actions.

## Actions

You should configure some environment secrets and variables.

### create_pipeline

This action publishes one or all pipelines to the Data Factory. First, the pipeline replaces all your secrets and variables, then it publishes directly to the Data Factory. You must provide the pipeline name, or use "all" to publish all pipelines. If a pipeline already exists, it will be overwritten.

### delete_pipeline

This action deletes one or all pipelines from the Data Factory. You must provide the pipeline name, or use "all" to delete all pipelines. You should delete all pipelines before running `terraform destroy`.
