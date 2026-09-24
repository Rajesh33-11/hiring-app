pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'MySonarQubeServer'
        SLACK_CHANNEL = '#jenkins-alerts'
        // 'SonarScanner' must match the SonarQube Scanner installation
        // name configured in Part 1, Step 1.2
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {

        stage('Git Clone') {
            steps {
                echo 'Cloning source code from GitHub...'
                git branch: 'main',
                    url: 'https://github.com/betawins/hiring-app.git'
                sh 'ls -la'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarCloud static code analysis...'
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh "${SCANNER_HOME}/bin/sonar-scanner"
                }
            }
        }

        stage('Slack Notification') {
            steps {
                echo 'Sending build result to Slack...'
                slackSend(
                    channel: "${SLACK_CHANNEL}",
                    color: 'good',
                    message: "✅ Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}\n${env.BUILD_URL}"
                )
            }
        }
    }

    post {
        failure {
            slackSend(
                channel: "${SLACK_CHANNEL}",
                color: 'danger',
                message: "❌ Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}\n${env.BUILD_URL}"
            )
        }
    }
}

