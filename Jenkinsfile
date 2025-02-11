pipeline {
    agent {
        docker {
            image 'ubuntu:latest'
            args '-u root'
        }
    }

    parameters {
        string(name: 'AWS_REGION', defaultValue: 'us-east-1', description: 'AWS Region (e.g., us-east-1)')
        string(name: 'PROJECT_NAME', defaultValue: 'flotorch', description: 'Project Name (e.g., flotorch)')
        string(name: 'TABLE_SUFFIX', defaultValue: '', description: 'Unique Table Suffix (6 lowercase letters)')
        string(name: 'CLIENT_NAME', defaultValue: 'flotorch', description: 'Client Name (e.g., flotorch)')
        string(name: 'CREATED_BY', defaultValue: 'DevOpsTeam', description: 'Created by (e.g., DevOpsTeam)')
        string(name: 'OPENSEARCH_ADMIN_USER', defaultValue: 'admin', description: 'OpenSearch Admin Username')
        password(name: 'OPENSEARCH_ADMIN_PASSWORD', description: 'OpenSearch Admin Password')
        password(name: 'NGINX_AUTH_PASSWORD', description: 'Nginx Auth Password')
        string(name: 'TEMPLATE_VERSION', defaultValue: '', description: 'FloTorch Template Version')
    }

    environment {
        AWS_REGION = "${params.AWS_REGION}"
        PROJECT_NAME = "${params.PROJECT_NAME}"
        TABLE_SUFFIX = "${params.TABLE_SUFFIX}"
        CLIENT_NAME = "${params.CLIENT_NAME}"
        CREATED_BY = "${params.CREATED_BY}"
        OPENSEARCH_ADMIN_USER = "${params.OPENSEARCH_ADMIN_USER}"
        OPENSEARCH_ADMIN_PASSWORD = "${params.OPENSEARCH_ADMIN_PASSWORD}"
        NGINX_AUTH_PASSWORD = "${params.NGINX_AUTH_PASSWORD}"
        TEMPLATE_VERSION = "${params.TEMPLATE_VERSION}"
    }

    stages {
        stage('Install dependencies') {
            steps {
                sh '''
                echo "Installing AWS CLI..."
                yum install -y unzip curl
                curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                unzip awscliv2.zip
                ./aws/install
                aws --version
                '''
            }
        }

        stage('Checkout Repository') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/FissionAI/FloTorch.git'
                }
            }
        }

        stage('Deploy FloTorch Master Stack') {
            steps {
                sh '''
                echo "Starting FloTorch Stack Deployment..."

                aws cloudformation create-stack \
                    --region ${AWS_REGION} \
                    --stack-name flotorch-stack \
                    --template-url https://flotorch-public.s3.amazonaws.com/${TEMPLATE_VERSION}/templates/master-template.yaml \
                    --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
                    --parameters \
                        ParameterKey=ProjectName,ParameterValue=${PROJECT_NAME} \
                        ParameterKey=TableSuffix,ParameterValue=${TABLE_SUFFIX} \
                        ParameterKey=ClientName,ParameterValue=${CLIENT_NAME} \
                        ParameterKey=CreatedBy,ParameterValue=${CREATED_BY} \
                        ParameterKey=OpenSearchAdminUser,ParameterValue=${OPENSEARCH_ADMIN_USER} \
                        ParameterKey=OpenSearchAdminPassword,ParameterValue=${OPENSEARCH_ADMIN_PASSWORD} \
                        ParameterKey=NginxAuthPassword,ParameterValue=${NGINX_AUTH_PASSWORD}

                echo "CloudFormation Stack Deployment Successful!"
                '''
            }
        }
    }

    post {
        success {
            echo "FloTorch Stack deployed successfully!"
        }
        failure {
            echo "Deployment failed!."
        }
    }
}
