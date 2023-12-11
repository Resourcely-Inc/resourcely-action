
This is the repo is to generate the resourcely github action in the marketplace.

To use this github action, there are two alternative options.

**Terraform Cloud**
If you wish for Resourcely to access Terraform Cloud to retrieve the Terraform plan and evaluate your plans and policies with every pull request, configure the `resourcely-action` with the provided settings. For additional information, such as instructions on creating an access token, please refer to the provided [link](https://docs.resourcely.com/getting-started/onboarding/ci-cd-setup/github-actions/terraform-cloud)
```   
# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# Permissions for the action
permissions:
  contents: read  # Read repository content
  packages: read  # Read packages from the repository

# Define jobs to be run
jobs:
  # Name of the job
  resourcely-ci:
    runs-on: ubuntu-latest  # The type of machine to run the job on
    steps:
      - uses: Resourcely-Inc/resourcely-action@v1 # import the action
        with:
          # grab the GitHub access token stored in the repo secrets
          gh_access_token: ${{ secrets.GH_ACCESS_TOKEN }}
          # grab the terraform api token stored in the repo secrets
          tf_api_token: ${{ secrets.TF_API_TOKEN }}
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          # set the resourcely api host
          resourcely_api_host: "https://api.resourcely.io"
```


**Local Plan**

This option requires that the Terraform plan file be available to GitHub Actions and visible to the Resourcely action. By default, Resourcely expected all of your terraform files located in the following directory`tf-plan-files`. For more information, please refer to this [link](https://docs.resourcely.com/getting-started/onboarding/ci-cd-setup/github-actions/local-plan). To use this option, you can specify the configuration with the following

```
# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# Permissions for the action
permissions:
  contents: read  # Read repository content
  packages: read  # Read packages from the repository

# Define jobs to be run
jobs:
  # Name of the job
  resourcely-ci:
    runs-on: ubuntu-latest  # The type of machine to run the job on
    steps:
      - uses: Resourcely-Inc/resourcely-action@v1 # import the action
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          # set the resourcely api host
          resourcely_api_host: "https://api.resourcely.io"
```


If you wish to place all your Terraform files within a directory named according to your custom choice, please indicate the desired configuration using the provided example below.
```
# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# Permissions for the action
permissions:
  contents: read  # Read repository content
  packages: read  # Read packages from the repository

# Define jobs to be run
jobs:
  # Name of the job
  resourcely-ci:
    runs-on: ubuntu-latest  # The type of machine to run the job on
    steps:
      - uses: Resourcely-Inc/resourcely-action@v1 # import the action
        with:
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          # set the resourcely api host
          resourcely_api_host: "https://api.resourcely.io"
          # set the tf directory for resourcely-action to read files from
          tf_directory: "my-custom-directory"
```

