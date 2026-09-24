pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        SONARQUBE_ENV = 'sonarqube'
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building Hiring App...'
                sh 'ls -la'
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube static code analysis...'

                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:5.2.0.4988:sonar'
                }
            }
        }
    }

    post {

        success {
            echo 'Hiring App build completed successfully.'
        }

        failure {
            echo 'Hiring App build failed.'
        }

        always {
            echo "Hiring App build completed with status: ${currentBuild.currentResult}"
        }
    }
}
