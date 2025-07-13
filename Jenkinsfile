pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
        jdk 'JDK21'
    }

    environment {
        GITHUB_REPO = 'wejhvabewjty/demo-ci'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "https://github.com/${GITHUB_REPO}.git"
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

        stage('Checkstyle') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Deploy to GitHub Packages') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
                    // Creamos el settings.xml con los valores ya sustituidos
                    writeFile file: 'settings.xml', text: """
                    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
                            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                            xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">
                    <servers>
                        <server>
                        <id>github</id>
                        <username>${GITHUB_USERNAME}</username>
                        <password>${GITHUB_TOKEN}</password>
                        </server>
                    </servers>
                    </settings>
                    """
                    sh 'mvn deploy -s settings.xml'
                }
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                reportDir: 'target/site',
                reportFiles: 'checkstyle.html',
                reportName: 'Checkstyle Report',
                allowMissing: false,
                keepAll: true,
                alwaysLinkToLastBuild: true
            ])
        }
    }
}
