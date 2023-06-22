pipeline {
  agent any
  
  environment {
    TF_VAR_access_key       = credentials('89fbb214-acb8-42d2-966d-c563eaaee8aa')
    TF_VAR_secret_key       = credentials('82e0686d-102d-4e22-8472-f62b61029cd4')
    TF_VAR_public_key       = credentials('93786fd0-1b22-428e-bbc6-0006150a03c5')
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
        sh 'terraform plan'
        sh 'terraform apply -auto-approve'
      }
    }
  }
}