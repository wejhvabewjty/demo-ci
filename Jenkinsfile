pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
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
                // Compilar antes de pasar spotbugs para evitar errores
                sh 'mvn compile spotbugs:spotbugs || true'
            }
        }
    }
}
