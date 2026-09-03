pipeline {
    agent any

    environment {
        APP_NAME = 'jenkins-demo-app'
        APP_VERSION = 'v1'
        DEPLOY_ENV = 'dev'
    }

    stages {

        stage('Test Jenkins') {
            steps {
                echo "Application: ${APP_NAME}"
                echo "Version: ${APP_VERSION}"
                echo "Environment: ${DEPLOY_ENV}"
            }
        }
    }
}