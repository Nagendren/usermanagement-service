pipeline {
    agent any

    tools {
        maven 'Maven 3'  // Make sure this matches your Jenkins tool name
        jdk 'JDK 11'     // Adjust to your JDK version as configured in Jenkins
    }

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=false"
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
            post {
                always {
                    junit 'target/surefire-reports/*.xml' // Reports for unit tests
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        // Optional: Add more stages like Deploy or Publish Artifacts here
    }

    post {
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}