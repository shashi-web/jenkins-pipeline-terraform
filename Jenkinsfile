pipeline {
  agent any
  
  environment {
    TF_VAR_access_key       = credentials('a94556e9-1863-4541-ac08-f480140ec2c9')
    TF_VAR_secret_key       = credentials('ac31bc4d-a4ef-4796-ae92-fa66eff86a12')
    TF_VAR_public_key       = credentials('ad04135c-d078-440e-839a-c71b4df66df2')
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