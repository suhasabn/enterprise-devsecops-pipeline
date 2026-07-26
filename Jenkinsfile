pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build, Test & Coverage') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Publish JaCoCo Report') {
            steps {
                sh '''
                    echo "===== Checking JaCoCo Report ====="
                    ls -la target || true
                    ls -la target/site || true
                    ls -la target/site/jacoco || true
                '''

                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'target/site/jacoco',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Coverage Report'
                ])
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }

        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
