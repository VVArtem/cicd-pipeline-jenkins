pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        
    }

    stages {
        stage('Init'){
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        echo "Main Branch"
                        env.IMAGE_NAME = 'nodemain:v1.0'
                        env.CONTAINER_NAME = 'node-app-main'
                        env.HOST_PORT = "3000"
                    }
                    else if (env.BRANCH_NAME == 'dev') {
                        echo "Dev Branch"
                        env.IMAGE_NAME = "nodedev:v1.0"
                        env.CONTAINER_NAME = "node-app-dev"
                        env.HOST_PORT = "3001"
                    }
                }
            }
        }

        stage('Install dependencies') {
            steps {
                echo 'Installing pependencies'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                sh 'npm test'
            }
        }

        stage('Docker build'){
            steps {
                script {
                    echo "Building Docker image: ${env.IMAGE_NAME}"
                    sh "docker build -t ${env.IMAGE_NAME} ."
                }
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    echo "Deploying container ${env.CONTAINER_NAME} on port ${env.HOST_PORT}..."
                    
                    // Remove old container before creation
                    sh "docker rm -f ${env.CONTAINER_NAME} || true"
                    
                    sh """
                        docker run -d \
                        --name ${env.CONTAINER_NAME} \
                        --expose ${env.HOST_PORT} \
                        -p ${env.HOST_PORT}:${env.INTERNAL_PORT} \
                        ${env.IMAGE_NAME}
                    """
                }
            }
        }
    }
}