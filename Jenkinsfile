pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                // sh 'mvn clean install'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
               // sh 'mvn test'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', allowEmptyArchive: true
                    mail to: 'nelkineldho01@gmail.com',
                         subject: "Jenkins Pipeline: Test Stage ${currentBuild.currentResult}",
                         body: "The Test stage has ${currentBuild.currentResult}. Please find the attached logs.",
                         attachLog: true
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Analyzing the code...'
               // sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
               // sh 'dependency-check.sh --project Jenkins-CICD-Pipeline --scan .'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/dependency-check-report.html', allowEmptyArchive: true
                    mail to: 'nelkineldho01@gmail.com',
                         subject: "Jenkins Pipeline: Security Scan Stage ${currentBuild.currentResult}",
                         body: "The Security Scan stage has ${currentBuild.currentResult}. Please find the attached logs.",
                         attachLog: true
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                //sh 'scp target/app.jar ec2-user@staging-server:/app'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
               // sh 'mvn integration-test -Denv=staging'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
               // sh 'scp target/app.jar ec2-user@production-server:/app'
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
