***There are two options available:***

**Terraform Cloud**

For Resourcely to access Terraform Cloud and retrieve the Terraform plan, enabling evaluation of your plans and policies with each pull request, configure the resourcely-action using the settings provided. For additional guidance, including instructions on creating an access token, please refer to the [Resourcely documentation](https://docs.resourcely.com/getting-started/onboarding/ci-cd-setup/github-actions/terraform-cloud)
```
# Trigger conditions for running this action
on:
  pull_request:
    branches: [ main ]

permissions:
  statuses: read

# Define jobs to be run
jobs:
  resourcely-ci:
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@v1 # import the action
        with:
          # use the generated GITHUB_TOKEN
          gh_access_token: ${{ github.token }}
          # grab the terraform api token stored in the repo secrets
          tf_api_token: ${{ secrets.TF_API_TOKEN }}
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
```


**Local Plan**

This option requires the Terraform plan file to be accessible and visible to the Resourcely GitHub Actions. By default, Resourcely expects all your Terraform files to be located in the `tf-plan-files` directory. For detailed instructions, please refer to the [Resourcely documentation](https://docs.resourcely.com/getting-started/onboarding/ci-cd-setup/github-actions/local-plan). To utilize this option, specify the configuration as follows:

```
# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

permissions:
  contents: read
  packages: read

# Define jobs to be run
jobs:
  resourcely-ci:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@v1 # import the action
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
```

You can set Pattern for Terraform plan files (e.g., plan*). Default Value: plan*
```
# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

permissions:
  contents: read
  packages: read

# Define jobs to be run
jobs:
  resourcely-ci:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@v1 # import the action
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          # set terraform plan file name
          tf_plan_pattern: "plan"
```


If you want to store all your Terraform plan files in a custom-named directory, please specify your preferred configuration using the example provided below:
```
# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

permissions:
  contents: read
  packages: read

# Define jobs to be run
jobs:
  resourcely-ci:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@v1 # import the action
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          # set the tf directory for resourcely-action to read plan files from
          tf_plan_directory: "my-custom-directory"
```
