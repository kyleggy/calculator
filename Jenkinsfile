pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:kyleggy/calculator.git'
            }
        }
        
        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }
        
        stage('Test') {
            steps {
                sh './gradlew test'
            }
            post {
                always {
                    junit 'build/test-results/test/*.xml'
                }
            }
        }
        stage("Code coverage") {
            steps {
                sh "./gradlew jacocoTestReport"
                publishHTML (target: [
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: "JaCoCo Report"
                ])
                sh "./gradlew jacocoTestCoverageVerification"
            }
        }
        stage("Static code analysis") {
            steps {
                sh "./gradlew checkstyleMain"
                publishHTML (target: [
                    reportDir: 'build/reports/checkstyle',
                    reportFiles: 'main.html',
                    reportName: "Checkstyle Report"
                ])
            }
}
    }
    
    post {
        success {
            echo 'Pipeline succeeded! Build completed successfully.'
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
        always {
            cleanWs()
        }
    }
}

