pipeline {
    agent any

    options {
        timestamps() // Add timestamps to console output
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git Branch to build')
        booleanParam(name: 'DEPLOY_TO_STAGING', defaultValue: false, description: 'Should we deploy to staging?')
        booleanParam(name: 'DEPLOY_TO_PRODUCTION', defaultValue: false, description: 'Should we deploy to production?')
    }

    environment {
        SCRIPTS_DIR = 'Scripts'
        DEPLOY_DIR = 'SITES'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out branch: ${params.BRANCH_NAME}"
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/nikhilpalegar/bitspilani-devops'
            }
        }

        stage('Build') {
            steps {
                echo "Running Build Script"
                sh '''
                    chmod +x ./${SCRIPTS_DIR}/build.sh
                    ./${SCRIPTS_DIR}/build.sh
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running Test Script"
                sh '''
                    chmod +x ./${SCRIPTS_DIR}/test.sh
                    ./${SCRIPTS_DIR}/test.sh
                '''
            }
        }

        stage('Deploy to Staging') {
            when {
                expression { return params.DEPLOY_TO_STAGING }
            }
            steps {
                echo "Deploying to STAGING"
                sh '''
                    chmod +x ./${DEPLOY_DIR}/deploy-staging.sh
                    ./${DEPLOY_DIR}/deploy-staging.sh
                '''
            }
        }

        stage('Deploy to Production') {
            when {
                expression { return params.DEPLOY_TO_PRODUCTION }
            }
            steps {
                echo "Deploying to PRODUCTION"
                sh '''
                    chmod +x ./${DEPLOY_DIR}/deploy-production.sh
                    ./${DEPLOY_DIR}/deploy-production.sh
                '''
            }
        }

    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
        always {
            echo "Build Completed"
        }
    }
}
