pipeline {
  agent any
  
  environment {
    TF_VAR_access_key       = credentials('d47f9240-03ab-410f-b9ba-836ee04049b2')
    TF_VAR_secret_key       = credentials('5933702e-dd43-4b00-a19b-f025894716a2')
    TF_VAR_public_key       = credentials('557a1ecb-7f51-420e-b7b9-393ed0135028')
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