pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/user/sample-app.git'
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build & Push') {
      steps {
        sh '''
        docker build -t my-app:latest .
        docker tag my-app:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
        docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
        '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/deployment.yaml'
      }
    }
  }
}
