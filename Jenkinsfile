pipeline {
    agent any
    
    tools {
        maven 'Maven' // Ensure this name matches what you configured in Jenkins
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                // bat 'mvn clean install'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
               // bat 'mvn test'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', allowEmptyArchive: true
                    // Ensure logs are archived if needed
                    archiveArtifacts artifacts: '**/target/*.log', allowEmptyArchive: true
                    emailext(
                        subject: "Build Status - Unit and Integration Tests",
                        body: "The unit and integration tests stage has completed. Please review the results.",
                        attachmentsPattern: '**/target/*.log',
                        to: 'nelkineldho01@gmail.com'
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
                // Uncomment if you are using SonarQube
                // bat 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                // Uncomment if you are using dependency-check
                // bat 'dependency-check.bat --project Jenkins-CICD-Pipeline --scan .'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/dependency-check-report.html', allowEmptyArchive: true
                    // Ensure logs are archived if needed
                    archiveArtifacts artifacts: '**/dependency-check-report/*.log', allowEmptyArchive: true
                    emailext(
                        subject: "Build Status - Security Scan",
                        body: "The security scan stage has completed. Please review the results.",
                        attachmentsPattern: '**/dependency-check-report/*.log',
                        to: 'nelkineldho01@gmail.com'
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                // Uncomment if you are deploying to a server
                // bat 'scp target/app.jar ec2-user@staging-server:/app'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Uncomment if you are running integration tests on staging
                // bat 'mvn integration-test -Denv=staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Uncomment if you are deploying to a server
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
            emailext(
                subject: "Pipeline Failure - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The pipeline ${env.JOB_NAME} #${env.BUILD_NUMBER} failed. Please check the build logs for details.",
                attachmentsPattern: '**/target/*.log',
                to: 'nelkineldho01@gmail.com'
            )
        }
    }
}
