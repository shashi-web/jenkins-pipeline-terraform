pipeline {
    agent {
        node {
            label ''
        }
    }
    
    environment {
        TF_VAR_access_key       = credentials('Access_key')
        TF_VAR_secret_key       = credentials('Secret_key')
        TF_VAR_public_key       = credentials('public_key')
        TF_VAR_cidr_block       = '10.0.0.0/24'
        TF_VAR_subnet_cidr_block = '10.0.0.0/25'
        TF_VAR_image_name       = 'ami-022e1a32d3f742bd8'
        TF_VAR_script_file      = 'nginx-entry-script.sh'
        TF_VAR_server_name      = 'terraform'
    }
    
    stages {
        stage('Fetch Terraform Files') {
            steps {
                git branch: 'master', url: 'https://github.com/shashi-web/terraform-27-06-2023.git'
            }
        }
        
        stage('Install Terraform') {
            steps {
                script {
                    def installedVersion = sh(returnStdout: true, script: 'terraform --version | awk \'{print $2}\'').trim()
                    
                    if (installedVersion.startsWith('Terraform')) {
                        echo "Terraform is already installed in version: ${installedVersion}"
                    } else {
                        sh 'sudo apt-get update'
                        sh 'sudo apt-get install unzip curl -y'
                        sh 'sudo curl -LO https://releases.hashicorp.com/terraform/0.14.10/terraform_0.14.10_linux_amd64.zip'
                        sh 'sudo unzip terraform_0.14.10_linux_amd64.zip'
                        sh 'sudo mv terraform /usr/local/bin/'
                        sh 'terraform --version'
                    }
                }
            }
        }
        
        stage('Execute Terraform Commands') {
            steps {
                sh 'terraform init'
                sh 'terraform plan'
                
                input(message: 'Do you want to proceed with Terraform apply?', ok: 'Proceed') 
                
                sh 'terraform apply -auto-approve'
            }
        }
    }
}