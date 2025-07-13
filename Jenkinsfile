pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
        jdk 'JDK21'
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
                configFileProvider([configFile(fileId: 'global-settings', variable: 'SETTINGS_XML')]) {
                    sh 'mvn deploy -s $SETTINGS_XML'
                }
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
