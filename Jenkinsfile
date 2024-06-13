pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS = credentials('313cfdad-ceba-4616-93f8-bc9b1dae9a3f')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    git 'https://github.com/Fozia1910/tfpipeline.git'
                }
            }
        }
        stage('Terraform Init') {
            steps {
                script {
                    dir('tfpipeline') {
                        withAzureServicePrincipal(credentialsId: '313cfdad-ceba-4616-93f8-bc9b1dae9a3f', 
                                                  subscriptionId: '005ccd00-654a-48a2-a3ef-ee990b504e9b',
                                                  tenantId: 'bf4a0561-a67c-4f99-99d6-4126cc81f773',
                                                  clientId: '97e3f051-e3db-4bb4-87b0-207f965750c6',
                                                  clientSecret: 'XGe8Q~MOId-hIomQ5GtzebHlu_WqvYjRcW4FWaOT') {
                            sh 'terraform init'
                        }
                    }
                }
            }
        }
        stage('Terraform Plan & Apply') {
            steps {
                script {
                    dir('tfpipeline') {
                        withAzureServicePrincipal(credentialsId: '313cfdad-ceba-4616-93f8-bc9b1dae9a3f', 
                                                  subscriptionId: '005ccd00-654a-48a2-a3ef-ee990b504e9b',
                                                  tenantId: 'bf4a0561-a67c-4f99-99d6-4126cc81f773',
                                                  clientId: '97e3f051-e3db-4bb4-87b0-207f965750c6',
                                                  clientSecret: 'XGe8Q~MOId-hIomQ5GtzebHlu_WqvYjRcW4FWaOT') {
                            sh 'terraform plan -out=tfplan'
                            sh 'terraform apply tfplan'
                        }
                    }
                }
            }
        }
    }
}
