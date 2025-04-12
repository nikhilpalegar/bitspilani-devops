pipeline {
    agent any
    parameters {
        booleanParam(name: 'DEPLOY_TO_STAGING', defaultValue: true, description: 'Should we deploy to staging?')
        booleanParam(name: 'DEPLOY_TO_PRODUCTION', defaultValue: true, description: 'Should we deploy to production?')
    }

    environment {
        SCRIPTS_DIR = 'Scripts'
        DEPLOY_DIR = 'SITES'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out brnach: ${env.BRANCH_NAME}"
                Checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Running Build Script"
                sh "chmod +x ./${SCRIPTS_DIR}/build.sh"
                sh "./${SCRIPTS_DIR}/build.sh"
            }
        }

        stage('Test') {
            steps {
                echo "Running Test Script"
                sh "chmod +x ./${SCRIPTS_DIR}/test.sh"
                sh "./${SCRIPTS_DIR}/test.sh"
            }
        }

        stage('Deploy to Staging') {
            when {
                expression { return params.DEPLOY_TO_STAGING }
            }
            steps {
                echo "Deploying to STAGING"
                sh "chmod +x ./${DEPLOY_DIR}/deploy-staging.sh"
                sh "./${DEPLOY_DIR}/deploy-staging.sh"
            }
        }

        stage('Deploy to Production') {
            when {
                expression { return params.DEPLOY_TO_PRODUCTION }
            }
            steps {
                echo "Deploying to PRODUCTION"
                sh "chmod +x ./${DEPLOY_DIR}/deploy-production.sh"
                sh "./${DEPLOY_DIR}/deploy-production.sh"
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "pipeline failed!"
        }
    }
}