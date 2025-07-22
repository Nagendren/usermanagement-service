```pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
    ECR_REPO = 'usermanagement-service'
    ECR_URI = "your_account_id.dkr.ecr.us-east-1.amazonaws.com/${ECR_REPO}"
    COMMIT_SHA = ''
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
        script {
          COMMIT_SHA = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
        }
      }
    }

    stage('Code Quality Check (Optional)') {
      when {
        expression { return params.RUN_SONAR == true }
      }
      steps {
        sh 'mvn sonar:sonar -Dsonar.projectKey=usermgmt -Dsonar.host.url=http://<sonar-host>:9000 -Dsonar.login=<token>'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('Docker Build & Push to ECR') {
      steps {
        script {
          sh '''
          aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_URI
          docker build -t $ECR_URI:$COMMIT_SHA .
          docker push $ECR_URI:$COMMIT_SHA
          '''
        }
      }
    }
  }

  post {
    success {
      build job: 'Deploy-To-ECS', parameters: [string(name: 'IMAGE_TAG', value: COMMIT_SHA)]
    }
  }
}
```
