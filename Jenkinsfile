pipeline {
    agent any

    parameters {
        choice(
            name: 'DEPLOY_ENV',
            choices: ['dev', 'staging', 'prod'],
            description: 'Select deployment environment'
        )

        string(
            name: 'APP_VERSION',
            defaultValue: 'v1',
            description: 'Docker image version'
        )
    }

    environment {
        APP_NAME = 'jenkins-demo-app'
    }

    stages {

        stage('Show Parameters') {
            steps {
                echo "Application: ${APP_NAME}"
                echo "Version: ${params.APP_VERSION}"
                echo "Environment: ${params.DEPLOY_ENV}"
            }
        }
    }
}