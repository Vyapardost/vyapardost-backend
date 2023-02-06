pipeline{
  agent any
  environment {
    AWS_ACCOUNT_ID="326135120403"
    AWS_DEFAULT_REGION="us-east-1"
    IMAGE_REPO_NAME="vyapardost-backend"
    IMAGE_TAG="latest"
    REPOSITORY_URI="${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
  }
  stages{
    stage("Logging into AWS ECR"){
      steps{
          script {
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 326135120403.dkr.ecr.us-east-1.amazonaws.com"
          }
      }
    }
    stage("Cloning git"){
      steps{
        checkout scmGit(branches: [[name: '*/development']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Vyapardost/vyapardost-backend.git'], [credentialsId: 'GithubAccess', url: 'https://github.com/Vyapardost/vyapardost-backend.git']])
      }
    }
    stage("Building Docker image"){
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      } 
    }
    stage("Pushing to ECR"){
      steps{
        script {
          sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
          sh "docker push ${REPOSITORY_URI}"
        }
      }
    } 
  }
}