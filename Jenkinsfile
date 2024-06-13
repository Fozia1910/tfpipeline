pipeline {
    agent any

    environment {
        ARM_SUBSCRIPTION_ID = '005ccd00-654a-48a2-a3ef-ee990b504e9b'
        ARM_CLIENT_ID       = credentials('97e3f051-e3db-4bb4-87b0-207f965750c6')
        ARM_CLIENT_SECRET   = credentials('XGe8Q~MOId-hIomQ5GtzebHlu_WqvYjRcW4FWaOT')
        ARM_TENANT_ID       = 'bf4a0561-a67c-4f99-99d6-4126cc81f773'
        TF_VAR_location     = 'East US'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Azure Login') {
            steps {
                script {
                    def creds = credentials('azurelogin')
                    withCredentials([azureServicePrincipal(credentialsId: 'azurelogin', displayName: 'Azure Credentials')]) {
                        sh "az login --service-principal -u $ARM_CLIENT_ID -p $ARM_CLIENT_SECRET -t $ARM_TENANT_ID"
                    }
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    sh 'terraform init -backend-config="storage_account_name=<your_storage_account_name> \
                         -backend-config="container_name=<your_container_name>" \
                         -backend-config="key=<your_backend_key>"'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    sh 'terraform plan -var="client_id=$ARM_CLIENT_ID" \
                        -var="client_secret=$ARM_CLIENT_SECRET" \
                        -var="subscription_id=$ARM_SUBSCRIPTION_ID" \
                        -var="tenant_id=$ARM_TENANT_ID"'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    sh 'terraform apply -auto-approve -var="client_id=$ARM_CLIENT_ID" \
                        -var="client_secret=$ARM_CLIENT_SECRET" \
                        -var="subscription_id=$ARM_SUBSCRIPTION_ID" \
                        -var="tenant_id=$ARM_TENANT_ID"'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
            echo 'Jenkins Build Completed'
        }
    }
}
