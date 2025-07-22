pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Ravindransivam/usermanagement-service.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }
}

