pipeline {
  agent any
  
  environment {
    TF_VAR_access_key       = credentials('bd44ff92-7e19-4b0a-be0b-ce2eb8462d2f')
    TF_VAR_secret_key       = credentials('59f3fee6-69ce-47c6-bf36-d9dc565a4c77')
    TF_VAR_public_key       = credentials('baee9f41-8582-48bc-9493-573e2e9f5855')
    TF_VAR_cidr_block       = '10.0.0.0/24'
    TF_VAR_subnet_cidr_block = '10.0.0.0/25'
    TF_VAR_image_name       = 'ami-022e1a32d3f742bd8'
    TF_VAR_script_file      = 'nginx-entry-script.sh'
    TF_VAR_server_name      = 'terraform'
  }
  
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/shashi-web/terraform-27-06-2023.git'
      }
    }
    
    stage('Terraform Setup') {
      steps {
        sh 'curl -LO https://releases.hashicorp.com/terraform/latest/terraform_latest_linux_amd64.zip'
        sh 'dnf install -y java-1.8.0-openjdk' // Install Java Development Kit (JDK)
        sh 'jar xvf terraform_latest_linux_amd64.zip' // Extract the contents of the ZIP file
        sh 'chmod +x terraform'
        sh 'mv terraform /usr/local/bin/'
        sh 'rm terraform_latest_linux_amd64.zip'
        sh 'terraform --version'
      }
    }
    
    stage('Terraform Apply') {
      steps {
        sh 'terraform init'
        sh 'terraform apply -auto-approve=false'
      }
    }
  }
}