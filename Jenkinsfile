pipeline {
  agent any
  
  environment {
    TF_VAR_access_key       = credentials('008174a4-606e-4445-afe9-fe914e0a9d75')
    TF_VAR_secret_key       = credentials('d54234e1-00c9-403a-8f2b-aaae2d0f6d01')
    TF_VAR_public_key       = credentials('97ddf096-c176-4647-8d93-d9b3228cc29b')
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