***There are two options available:***

**Terraform Cloud**

For Resourcely to access Terraform Cloud and retrieve the Terraform plan, enabling evaluation of your plans and policies with each pull request, configure the resourcely-action using the settings provided. For additional guidance, including instructions on creating an access token, please refer to the [Resourcely documentation](https://docs.resourcely.com/getting-started/onboarding/ci-cd-setup/github-actions/terraform-cloud)
```
# Trigger conditions for running this action
on:
  pull_request:
    branches: [ main ]

# Define jobs to be run
jobs:
  resourcely-ci:
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@main
        with:
          # grab the GitHub access token stored in the repo secrets
          gh_access_token: ${{ secrets.GH_ACCESS_TOKEN }}
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

# Define jobs to be run
jobs:
  resourcely-ci:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@main
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
```
If you are submitting multiple plans locally you'll need to supply a manifest as well with the appropriate configuration for your plan submissions setup. e.g.:

```
 ...
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          manifest: |
            {
              "plans": [{
                "plan_file": "plan-dev.json",
                "config_root_path": ".",
                "environment": "dev"
              },{
                "plan_file": "plan-prod.json",
                "config_root_path": ".",
                "environment": "prod"
              }]
            }
```


You can set Pattern for Terraform plan files (e.g., plan*). Default Value: plan*
```
# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# Define jobs to be run
jobs:
  resourcely-ci:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@main
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          # set terraform plan pattern
          tf_plan_pattern: "plan*"
```


If you want to store all your Terraform plan files in a custom-named directory, please specify your preferred configuration using the example provided below:
```
# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# Define jobs to be run
jobs:
  resourcely-ci:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@main
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          # set the tf directory for resourcely-action to read plan files from
          tf_plan_directory: "my-custom-directory"
```

**Configuration**
This action pulls two docker images from Resourcely. The first is [wait-for-terraform](https://github.com/Resourcely-Inc/resourcely-container-registry/pkgs/container/wait-for-terraform-plan/319601944?tag=latest) and the second is [resourcely-cli](https://github.com/Resourcely-Inc/resourcely-container-registry/pkgs/container/resourcely-cli). By default we pull the latest version, but if you would like to pin to a specific version you can do so by using `docker_tag_wait_for_terraform` and `docker_tag_resourcely_cli`
```
jobs:
  resourcely-ci:
    runs-on: ubuntu-latest
    steps:
      - uses: Resourcely-Inc/resourcely-action@main
        with:
          ...
          # pin versions of resourcely containers
          docker_tag_wait_for_terraform: v0.1.6
          docker_tag_resourcely_cli: v1.0.45
```
