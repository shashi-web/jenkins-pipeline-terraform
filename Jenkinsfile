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
        
        stage('Check and Install Terraform') {
            steps {
                script {
                    def installedVersion = sh(returnStdout: true, script: 'terraform --version | awk \'{print $2}\'').trim()
                    def latestVersion = sh(returnStdout: true, script: 'curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r \'.current_version\'').trim()
                    
                    if (installedVersion.startsWith('Terraform')) {
                        if (installedVersion.contains(latestVersion)) {
                            echo "Terraform is already installed in the latest version: ${installedVersion}"
                        } else {
                            echo "Installed version of Terraform (${installedVersion}) is not the latest. Installing the latest version (${latestVersion})..."
                            sh 'curl -LO "https://releases.hashicorp.com/terraform/${latestVersion}/terraform_${latestVersion}_linux_amd64.zip"'
                            sh 'unzip "terraform_${latestVersion}_linux_amd64.zip"'
                            sh 'sudo mv terraform /usr/local/bin/'
                            sh 'rm "terraform_${latestVersion}_linux_amd64.zip"'
                            echo "Latest version of Terraform (${latestVersion}) has been installed"
                        }
                    } else {
                        echo "Terraform is not installed. Installing the latest version (${latestVersion})..."
                        sh 'curl -LO "https://releases.hashicorp.com/terraform/${latestVersion}/terraform_${latestVersion}_linux_amd64.zip"'
                        sh 'unzip "terraform_${latestVersion}_linux_amd64.zip"'
                        sh 'sudo mv terraform /usr/local/bin/'
                        sh 'rm "terraform_${latestVersion}_linux_amd64.zip"'
                        echo "Latest version of Terraform (${latestVersion}) has been installed"
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