pipeline {
    agent any
    
    tools {
        maven 'Maven' // Ensure this name matches what you configured in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                bat 'mvn clean install'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                bat 'mvn test'
            }
            post {
                always {
                    // Archive test results and logs
                    archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', allowEmptyArchive: true
                    archiveArtifacts artifacts: '**/target/*.log', allowEmptyArchive: true
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
                // Example code analysis step
                // bat 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Example security scan step
                // bat 'dependency-check.bat --project Jenkins-CICD-Pipeline --scan .'
            }
            post {
                always {
                    // Archive security scan results
                    archiveArtifacts artifacts: '**/dependency-check-report.html', allowEmptyArchive: true
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                // Example deploy step
                // bat 'scp target/app.jar ec2-user@staging-server:/app'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Example integration test step
                // bat 'mvn integration-test -Denv=staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Example deploy step
                // bat 'scp target/app.jar ec2-user@production-server:/app'
            }
        }
    }

    post {
        always {
            // Send email with logs attached
            script {
                def logFile = "${env.WORKSPACE}/build.log"
                def emailSubject = currentBuild.result == 'SUCCESS' ? "Pipeline Success" : "Pipeline Failure"
                def emailBody = "The pipeline ${currentBuild.result.toLowerCase()}. Please review the attached log file for details."
                
                // Send email with attachment
                emailext (
                    to: 'nelkineldho01@gmail.com',
                    subject: emailSubject,
                    body: emailBody,
                    attachmentsPattern: '**/target/*.log',
                    mimeType: 'text/plain'
                )
            }
        }
    }
}
