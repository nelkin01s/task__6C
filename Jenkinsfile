pipeline {
    agent any
    
    tools {
        maven 'Maven' // Ensure this name matches what you configured in Jenkins
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Example: Uncomment the following line if you want to run Maven build
                // bat 'mvn clean install'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Example: Uncomment the following line if you want to run Maven tests
                // bat 'mvn test'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', allowEmptyArchive: true
                    archiveArtifacts artifacts: '**/target/*.log', allowEmptyArchive: true
                    script {
                        def emailSubject = currentBuild.result == 'SUCCESS' ? "Build Success - Unit and Integration Tests" : "Build Failure - Unit and Integration Tests"
                        def emailBody = "The unit and integration tests stage has completed. Please review the attached log file for details."
                        emailext(
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

        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
                // Example: Uncomment the following line if you want to run code analysis
                // bat 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Example: Uncomment the following line if you want to run security scan
                // bat 'dependency-check.bat --project Jenkins-CICD-Pipeline --scan .'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/dependency-check-report.html', allowEmptyArchive: true
                    script {
                        def emailSubject = currentBuild.result == 'SUCCESS' ? "Build Success - Security Scan" : "Build Failure - Security Scan"
                        def emailBody = "The security scan stage has completed. Please review the attached report for details."
                        emailext(
                            to: 'nelkineldho01@gmail.com',
                            subject: emailSubject,
                            body: emailBody,
                            attachmentsPattern: '**/dependency-check-report.html',
                            mimeType: 'text/html'
                        )
                    }
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                // Example: Uncomment the following line if you want to deploy to staging
                // bat 'scp target/app.jar ec2-user@staging-server:/app'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Example: Uncomment the following line if you want to run integration tests
                // bat 'mvn integration-test -Denv=staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Example: Uncomment the following line if you want to deploy to production
                // bat 'scp target/app.jar ec2-user@production-server:/app'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
