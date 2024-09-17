pipeline {
    agent any
    environment {
        AWS_REGION = 'eu-west-1'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/muckalo/agrcic-tf-and-jenkins-1.git'
            }
        }
        stage('Prepare Lambda Deployment Package') {
            steps {
                sh '''
                cd lambda
                zip -r ../lambda_function.zip .
                cd ..
                '''
            }
        }
        stage('Deploy with Terraform') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                    terraform init
                    terraform apply -auto-approve
                    '''
                }
            }
        }
    }
}
