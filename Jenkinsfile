pipeline {
  agent any
  
  environment {
    TF_VAR_access_key       = credentials('c68b4688-44a5-4f97-91c9-119b25e77c7c')
    TF_VAR_secret_key       = credentials('7e0daa05-e1d3-44f1-b8b6-a9b3efac5178')
    TF_VAR_public_key       = credentials('38b4a35a-ed6d-4e9e-8765-53fd761e16f8')
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