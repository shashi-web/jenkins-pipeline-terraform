pipeline {
  agent any


  parameters {
    string(name: 'access_key', description: 'Access Key')
    string(name: 'secret_key', description: 'Secret Key')
    string(name: 'public_key', description: 'Public Key')
    choice(name: 'cidr_block', choices: ['10.0.0.0/24'], description: 'CIDR Block')
    choice(name: 'subnet_cidr_block', choices: ['10.0.0.0/25'], description: 'Subnet CIDR Block')
    choice(name: 'image_name', choices: ['ami-022e1a32d3f742bd8'], description: 'Image Name')
    choice(name: 'script_file', choices: ['nginx-entry-script.sh'], description: 'Script File')
    choice(name: 'server_name', choices: ['jenkins'], description: 'Server Name')
  }

  stages {
    stage('Source Code Retrieval') {
      steps {
        // Checkout source code from version control system (e.g., Git)
        git(
          url: 'https://github.com/shashi-web/terraform-27-06-2023.git',
          credentialsId: '3ee1c4bc-b18e-4c20-9500-b1bac41e92d7',
          branch: 'master'
        )
      }
    }

    stage('Who Am I') {
      steps {
        sh 'whoami'
      }
    }

    stage('Check Dependencies') {
      steps {
        script {
          sshagent(['03007e71-c7b5-4c86-8466-962176e6d5bb']) {
            def curlInstalled = sh(script: 'command -v curl', returnStatus: true) == 0
            def jqInstalled = sh(script: 'command -v jq', returnStatus: true) == 0

            echo "curl is installed: ${curlInstalled}"
            echo "jq is installed: ${jqInstalled}"

            if (!curlInstalled) {
              echo "Installing curl..."
              sh 'yum update'
              sh 'yum install -y curl'
            }

            if (!jqInstalled) {
              echo "Installing jq..."
              sh 'yum update'
              sh 'yum install -y jq'
            }
          }
        }
      }
    }
    
    stage('Get Latest Terraform Version') {
      steps {
        script {
          sshagent(['03007e71-c7b5-4c86-8466-962176e6d5bb']) {
            def latestVersion = sh(returnStdout: true, script: "curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r '.current_version'")
            echo "Latest Terraform version: ${latestVersion}"
          }
          
        }
      }
    }

    stage('Infrastructure Provisioning & Deployment') {
      steps {
        sshagent(['03007e71-c7b5-4c86-8466-962176e6d5bb']) {
            // Initialize Terraform
            sh 'terraform init'

            // Plan infrastructure changes
            sh 'terraform plan -out=tfplan'

            // Apply infrastructure changes (if approved)
            input message: 'Deploy infrastructure?', ok: 'Deploy'
            sh 'terraform apply tfplan'
        }
      }
    }
  }
}
