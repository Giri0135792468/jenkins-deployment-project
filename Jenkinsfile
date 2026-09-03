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
    options {
        timeout(time: 5, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {
         stage('Stage Environment') {
            environment {
               STAGE_MESSAGE = 'This exists only in this stage'
            }
           steps {
                echo "${STAGE_MESSAGE}"
            }
        }
        stage('Show Parameters') {
            steps {
                echo "Application: ${APP_NAME}"
                echo "Version: ${params.APP_VERSION}"
                echo "Environment: ${params.DEPLOY_ENV}"
            }
        }
    }
    post {
       always {
        echo 'Pipeline completed'
    }

    success {
        echo 'Deployment pipeline succeeded'
    }

    failure {
        echo 'Deployment pipeline failed'
    }
    }
}