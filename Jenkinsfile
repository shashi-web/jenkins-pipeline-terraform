pipeline {
  agent any

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
              sh 'sudo apt-get update'
              sh 'sudo apt-get install -y curl'
            }

            if (!jqInstalled) {
              echo "Installing jq..."
              sh 'sudo apt-get update'
              sh 'sudo apt-get install -y jq'
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
