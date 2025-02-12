pipeline {
    agent {
        docker {
            image 'ubuntu:latest'
            args '-u root'
        }
    }

    parameters {
        // ... keeping existing parameters ...
    }

    environment {
        // ... keeping existing environment variables ...
    }

    stages {
        stage('Install dependencies') {
            steps {
                sh '''
                echo "Installing AWS CLI..."
                apt-get update
                apt-get install -y unzip curl
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
                sh """
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
                """
            }
        }
    }

    post {
        success {
            echo "FloTorch Stack deployed successfully!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}
