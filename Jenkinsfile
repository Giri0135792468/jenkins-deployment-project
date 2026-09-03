pipeline {
    agent any



    environment {
        APP_NAME = 'jenkins-demo-app'
        APP_VERSION = 'v1'
    DEPLOY_ENV = 'dev'
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

        

        stage('Build Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh 'docker --version'
                    sh 'docker build -t ${DOCKER_USERNAME}/jenkins-demo-app:${APP_VERSION} .'
                }
            }
        }
        stage('Stash Application') {
    steps {
        stash name: 'application-files',
              includes: 'app/index.html,Dockerfile,k8s/**'
    }
}
stage('Use Stashed Files') {
    steps {
        deleteDir()

        unstash 'application-files'

        sh 'ls -la'
        sh 'ls -la app'
    }
}
        stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                    sh 'docker push ${DOCKER_USERNAME}/jenkins-demo-app:${APP_VERSION}'
                    sh 'docker logout'
                }
            }
        }
        stage('Approval') {
    steps {
        input message: 'Do you want to continue with deployment?',
              ok: 'Deploy'
    }
}
stage('Deploy to Kubernetes') {
    steps {
        sh '''
            kubectl apply -f k8s/deployment.yaml
            kubectl apply -f k8s/service.yaml
        '''
    }
}
stage('Archive Artifacts') {
    steps {
        archiveArtifacts artifacts: 'k8s/*.yaml', fingerprint: true
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