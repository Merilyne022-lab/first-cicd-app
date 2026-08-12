pipeline {
    agent any

    tools {
        maven 'mymaven 3.9'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=first-cicd-app -Dsonar.sources=src/main -Dsonar.tests=src/test -Dsonar.host.url=http://localhost:9000 -Dsonar.login=admin -Dsonar.password=admin'
            }
        }
    }
}
