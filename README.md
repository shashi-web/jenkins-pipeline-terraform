# Jenkins Terraform Pipeline

This repository contains a Jenkins pipeline script for automating Terraform infrastructure provisioning and management. The pipeline fetches Terraform files from a Git repository, installs Terraform, and executes Terraform commands based on user input.

## Prerequisites

Before using this pipeline, ensure the following prerequisites are met:

* Jenkins is installed and configured.
* Terraform is installed on the Jenkins server.
* Access and secret keys for an AWS IAM user with appropriate permissions are available as Jenkins credentials.

## Usage

* Configure the Jenkins pipeline with the appropriate credentials and environment variables.
* Create a Jenkins job and link it to this pipeline script.
* When triggered, the pipeline will fetch the Terraform files from the specified Git repository and install Terraform.
* The pipeline will prompt you to select an action: "Apply" or "Destroy".
* If "Apply" is selected, the pipeline will execute terraform plan, prompt for confirmation, and then execute terraform apply to apply the Terraform changes.
* If "Destroy" is selected, the pipeline will execute terraform plan -destroy, prompt for confirmation, and then execute terraform destroy to destroy the provisioned resources.
* The pipeline will display status messages and inform you of the outcome of the selected action.

## Notes

* This pipeline assumes the Terraform files are stored in a Git repository. Adjust the Git repository URL and branch as needed in the pipeline script.
* Make sure to properly configure the AWS IAM user credentials and permissions for Terraform to interact with your AWS infrastructure.
* Customize the environment variables in the pipeline script according to your specific Terraform configuration.