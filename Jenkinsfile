pipeline {
    agent any

    environment {
        DIRECTORY_PATH = '/path/to/code'
        TESTING_ENVIRONMENT = 'TestEnv'
        PRODUCTION_ENVIRONMENT = 'AdiProduction'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo "Fetch the source code from the directory path specified by the environment variable: ${env.DIRECTORY_PATH}"
                    echo "Compile the code and generate any necessary artifacts."
                    sh 'echo "Build stage complete." > build.log'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo "Running unit tests."
                    echo "Running integration tests."
                    sh 'echo "Test stage complete." > test.log'
                }
            }
        }
        stage('Code Quality Check') {
            steps {
                script {
                    echo "Check the quality of the code."
                    sh 'echo "Code Quality Check complete." > code_quality.log'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploy the application to a testing environment specified by the environment variable: ${env.TESTING_ENVIRONMENT}"
                    sh 'echo "Deploy stage complete." > deploy.log'
                }
            }
        }
        stage('Approval') {
            steps {
                echo "Waiting for approval..."
                sleep(time: 10, unit: 'SECONDS')
            }
            post {
                always {
                    script {
                        def logFiles = ['build.log', 'test.log', 'code_quality.log', 'deploy.log']
                        def attachments = logFiles.collect { it -> readFile(file: it) }
                        emailext subject: "Pipeline Approval Notification",
                                 body: "The pipeline is awaiting approval. Logs are attached.",
                                 to: "adhi2851@gmail.com",
                                 attachmentsPattern: '*.log'
                    }
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                script {
                    echo "Deploy the code to the production environment: ${env.PRODUCTION_ENVIRONMENT}"
                    sh 'echo "Deploy to Production stage complete." > deploy_production.log'
                }
            }
            post {
                always {
                    script {
                        def logFiles = ['build.log', 'test.log', 'code_quality.log', 'deploy.log', 'deploy_production.log']
                        def attachments = logFiles.collect { it -> readFile(file: it) }
                        emailext subject: "Pipeline Deployment Notification",
                                 body: "The pipeline has been deployed to the production environment: ${env.PRODUCTION_ENVIRONMENT}. Logs are attached.",
                                 to: "adhi2851@gmail.com",
                                 attachmentsPattern: '*.log'
                    }
                }
            }
        }
    }
}
