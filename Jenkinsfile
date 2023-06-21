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
          def sshCommand(command) {
            withCredentials([sshUserPrivateKey(credentialsId: '03007e71-c7b5-4c86-8466-962176e6d5bb', keyFileVariable: 'KEY_FILE')]) {
              return sh(script: "ssh -i \$KEY_FILE ec2-user@54.158.216.25 ${command}", returnStdout: true).trim()
            }
          }

          def curlInstalled = sshCommand('command -v curl') == 0
          def jqInstalled = sshCommand('command -v jq') == 0

          echo "curl is installed: ${curlInstalled}"
          echo "jq is installed: ${jqInstalled}"

          if (!curlInstalled) {
            echo "Installing curl..."
            sshCommand('apt-get update')
            sshCommand('apt-get install -y curl')
          }

          if (!jqInstalled) {
            echo "Installing jq..."
            sshCommand('apt-get update')
            sshCommand('apt-get install -y jq')
          }
        }
      }
    }

    stage('Get Latest Terraform Version') {
      steps {
        script {
          def sshCommand(command) {
            withCredentials([sshUserPrivateKey(credentialsId: '03007e71-c7b5-4c86-8466-962176e6d5bb', keyFileVariable: 'KEY_FILE')]) {
              return sh(script: "ssh -i \$KEY_FILE ec2-user@54.158.216.25 ${command}", returnStdout: true).trim()
            }
          }

          def latestVersion = sshCommand("curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r '.current_version'")
          echo "Latest Terraform version: ${latestVersion}"
        }
      }
    }

    stage('Infrastructure Provisioning & Deployment') {
      steps {
        script {
          def sshCommand(command) {
            withCredentials([sshUserPrivateKey(credentialsId: '03007e71-c7b5-4c86-8466-962176e6d5bb', keyFileVariable: 'KEY_FILE')]) {
              return sh(script: "ssh -i \$KEY_FILE ec2-user@54.158.216.25 ${command}", returnStdout: true).trim()
            }
          }

          sshCommand('terraform init')
          sshCommand('terraform plan -out=tfplan')

          input message: 'Deploy infrastructure?', ok: 'Deploy'
          sshCommand('terraform apply tfplan')
        }
      }
    }
  }
}
