pipeline {
    agent any
    
    tools {
        maven 'Maven' // Ensure this name matches what you configured in Jenkins
    }

    environment {
        LOGS_DIR = 'build_logs' // Directory to store logs
        LOG_FILE = "${LOGS_DIR}/build.log" // Log file path
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                script {
                    // Capture build logs
                    def logFile = new File("${env.WORKSPACE}/${env.LOG_FILE}")
                    bat(script: 'mvn clean install > ' + logFile.absolutePath + ' 2>&1', returnStatus: true)
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                script {
                    // Capture test logs
                    def logFile = new File("${env.WORKSPACE}/${env.LOG_FILE}")
                    bat(script: 'mvn test >> ' + logFile.absolutePath + ' 2>&1', returnStatus: true)
                }
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', allowEmptyArchive: true
                    mail to: 'nelkineldho01@gmail.com',
                         subject: "Build Status - Unit and Integration Tests",
                         body: "The unit and integration tests stage has completed. Please review the results.",
                         attachmentsPattern: "${env.LOG_FILE}"
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
                script {
                    // Capture code analysis logs
                    def logFile = new File("${env.WORKSPACE}/${env.LOG_FILE}")
                    bat(script: 'sonar-scanner >> ' + logFile.absolutePath + ' 2>&1', returnStatus: true)
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                script {
                    // Capture security scan logs
                    def logFile = new File("${env.WORKSPACE}/${env.LOG_FILE}")
                    bat(script: 'dependency-check.bat --project Jenkins-CICD-Pipeline --scan . >> ' + logFile.absolutePath + ' 2>&1', returnStatus: true)
                }
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/dependency-check-report.html', allowEmptyArchive: true
                    mail to: 'nelkineldho01@gmail.com',
                         subject: "Build Status - Security Scan",
                         body: "The security scan stage has completed. Please review the results.",
                         attachmentsPattern: "${env.LOG_FILE}"
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                script {
                    // Capture deployment logs
                    def logFile = new File("${env.WORKSPACE}/${env.LOG_FILE}")
                    bat(script: 'scp target/app.jar ec2-user@staging-server:/app >> ' + logFile.absolutePath + ' 2>&1', returnStatus: true)
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                script {
                    // Capture integration test logs
                    def logFile = new File("${env.WORKSPACE}/${env.LOG_FILE}")
                    bat(script: 'mvn integration-test -Denv=staging >> ' + logFile.absolutePath + ' 2>&1', returnStatus: true)
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                script {
                    // Capture production deployment logs
                    def logFile = new File("${env.WORKSPACE}/${env.LOG_FILE}")
                    bat(script: 'scp target/app.jar ec2-user@production-server:/app >> ' + logFile.absolutePath + ' 2>&1', returnStatus: true)
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
            mail to: 'nelkineldho01@gmail.com',
                 subject: "Pipeline Success",
                 body: "The pipeline completed successfully.",
                 attachmentsPattern: "${env.LOG_FILE}"
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
            mail to: 'nelkineldho01@gmail.com',
                 subject: "Pipeline Failure",
                 body: "The pipeline failed. Please review the logs for details.",
                 attachmentsPattern: "${env.LOG_FILE}"
        }
    }
}
