pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
        AWS_REGION = 'ap-south-1'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/onkarlonkar9/serverless-terraform-jenkins.git', branch: "main"
            }
        }

        stage('Install Terraform') {
            steps {
                sh 'terraform -v || sudo apt-get install -y terraform'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Package Lambda') {
            steps {
                dir('lambda') {
                    sh 'zip ../lambda_function.zip lambda_function.py'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}

