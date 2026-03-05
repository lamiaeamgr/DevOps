pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = 'docker compose'
        SLACK_CHANNEL_BACKEND = env.SLACK_CHANNEL_BACKEND ?: '#devops-backend'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Backend - Lint & Test') {
            steps {
                sh 'npm ci'
                sh 'npm run lint'
                sh 'npm run test'
            }
            post {
                always {
                    publishHTML(target: [allowMissing: true, reportDir: 'coverage/lcov-report', reportFiles: 'index.html', reportName: 'Backend Coverage'])
                }
            }
        }

        stage('SonarQube Analysis') {
            when {
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        npm ci
                        npx sonar-scanner \
                            -Dsonar.projectKey=miniprojet-devops-backend \
                            -Dsonar.sources=src \
                            -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                    '''
                }
            }
        }

        stage('Quality Gate') {
            when {
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh "${DOCKER_COMPOSE} build --no-cache"
            }
        }

        stage('Deploy (Docker Compose)') {
            when {
                anyOf {
                    branch 'main'
                }
            }
            steps {
                sh "${DOCKER_COMPOSE} down --remove-orphans || true"
                sh "${DOCKER_COMPOSE} up -d --build"
            }
        }
    }

    post {
        success {
            slackSend(
                color: 'good',
                message: "[Backend] Pipeline réussi: ${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BRANCH_NAME})",
                channel: "${SLACK_CHANNEL_BACKEND}"
            )
        }
        failure {
            slackSend(
                color: 'danger',
                message: "[Backend] Pipeline en échec: ${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BRANCH_NAME})",
                channel: "${SLACK_CHANNEL_BACKEND}"
            )
        }
        unstable {
            slackSend(
                color: 'warning',
                message: "[Backend] Pipeline instable: ${env.JOB_NAME} #${env.BUILD_NUMBER} (${env.BRANCH_NAME})",
                channel: "${SLACK_CHANNEL_BACKEND}"
            )
        }
    }
}
