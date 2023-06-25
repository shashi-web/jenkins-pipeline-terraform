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
                    // Install Terraform code
                    sh 'terraform --version'
                }
            }
        }

        stage('Execute Terraform Commands') {
            steps {
                sh 'terraform init'

                script {
                    def action = input(
                        message: 'Select the action to perform:',
                        parameters: [
                            choice(name: 'ACTION', choices: ['Apply', 'Destroy'], description: 'Choose the action')
                        ]
                    )

                    action = action.toLowerCase()

                    if (action == 'apply') {
                        sh 'terraform plan'
                        // Prompt for confirmation before applying the Terraform changes
                        def confirmApply = input(
                            message: 'Do you want to proceed with Terraform apply?',
                            parameters: [
                                booleanParam(defaultValue: false, description: 'Confirm apply', name: 'confirmApply')
                            ]
                        )

                        if (confirmApply == true) {
                            sh 'terraform apply -auto-approve'
                        } else {
                            echo 'Apply operation aborted by user.'
                        }
                    } else if (action == 'destroy') {
                        sh 'terraform plan -destroy'
                        // Prompt for confirmation before destroying resources
                        def confirmDestroy = input(
                            message: 'Are you sure you want to destroy the resources?',
                            parameters: [
                                booleanParam(defaultValue: false, description: 'Confirm destroy', name: 'confirmDestroy')
                            ]
                        )

                        if (confirmDestroy == true) {
                            sh 'terraform destroy -auto-approve'
                        } else {
                            echo 'Destroy operation aborted by user.'
                        }
                    } else {
                        error "Invalid action selected: ${action}"
                    }
                }
            }
        }
    }
}
