pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
        jdk 'JDK21'
    }

    environment {
        GITHUB_USERNAME = 'wejhvabewjty'
        GITHUB_TOKEN = credentials('github-token')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/wejhvabewjty/demo-ci.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Code Quality') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Deploy to GitHub Packages') {
            steps {
                sh 'mvn deploy -s jenkins/settings.xml'
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'target/site',
                reportFiles: 'checkstyle.html',
                reportName: 'Checkstyle Report'
            ])
        }
    }
}