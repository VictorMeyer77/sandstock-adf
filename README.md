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

1. When you run `terraform apply`, all Data Factory components are created, except for the pipelines.
2. Via the Azure Data Factory interface, you can import ADF resources into the main branch.
3. Create a new branch based on the main branch and create or update pipelines in your working branch.
4. Before merging with the main branch, replace all your secrets and variables with GitHub Environment variable names.
5. Deploy your pipeline in a validation environment via GitHub Actions.
6. Once your development is validated, you can merge it into the main branch.
7. With GitHub Actions, publish the new pipeline.
8. To destroy the infrastructure using the `terraform destroy` command, you should remove all pipelines. This can also be achieved using GitHub Actions.

## Actions

You should configure some environment secrets and variables.

### create_pipeline

This action publishes one or all pipelines to the Data Factory. First, the pipeline replaces all your secrets and variables, then it publishes directly to the Data Factory. You must provide the pipeline name, or use "all" to publish all pipelines. If a pipeline already exists, it will be overwritten.

### delete_pipeline

This action deletes one or all pipelines from the Data Factory. You must provide the pipeline name, or use "all" to delete all pipelines. You should delete all pipelines before running `terraform destroy`.
