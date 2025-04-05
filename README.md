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

Only pipelines and triggers can be updated via the Azure Data Factory interface.

This repository have two principal branches: `main` and `dev`. Both are the same, except the main branch is tokenized with environment variables like {{VARIABLE_NAME}}.

1. When you run `terraform apply`, all Data Factory components are created, except for pipelines and triggers.
2. Via the Azure Data Factory interface, you can import ADF resources into the dev branch.
3. Create a new branch based on the dev branch and create or update pipelines/triggers in your working branch.
4. Create a pull request between your working branch and the dev branch and merge it.
5. Create a release branch based on the dev branch and replace all your environment variable with {{VARIABLE_NAME}}
6. Create a pull request between the release branch and the main branch and merge it.
7. With GitHub Actions, publish pipelines or triggers.
8. By default, triggers are disabled. Use actions to start them.
9. To destroy the infrastructure using the `terraform destroy` command, you should remove all pipelines and triggers. This can also be achieved using GitHub Actions.

## Actions

You should configure some environment secrets and variables.

### pipeline_create

This action publishes one or all pipelines to the Data Factory. First, the action replaces all your secrets and variables, then it publishes directly to the Data Factory. You must provide the pipeline name, or use "all" to publish all pipelines. If a pipeline already exists, it will be overwritten.

### pipeline_delete

This action deletes one or all pipelines from the Data Factory. You must provide the pipeline name, or use "all" to delete all pipelines. You should delete all pipelines before running `terraform destroy`.

### trigger_create

This action publishes one or all triggers to the Data Factory. First, the action replaces all your secrets and variables, then it publishes directly to the Data Factory. You must provide the trigger name, or use "all" to publish all triggers. You should release associated pipelines first.

### pipeline_delete

This action deletes one or all triggers from the Data Factory. You must provide the trigger name, or use "all" to delete all triggers. You should delete all triggers before running `terraform destroy`.

### trigger_start_stop

This action start or stop one or all triggers. You must provide the trigger name, or use "all" to start or stop all triggers.