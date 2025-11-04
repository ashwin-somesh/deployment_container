pipeline {
  agent any

  environment {
    DOCKERHUB = credentials('dockerhub')
    IMAGE = "ashwin/myapp"
  }

  stages {
    stage("Checkout") {
      steps {
        checkout scm
      }
    }

    stage("Build Docker Image") {
      steps {
        sh "docker build -t $IMAGE:${BUILD_NUMBER} ."
      }
    }

    stage("Login to DockerHub") {
      steps {
        sh "echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin"
      }
    }

    stage("Push Image") {
      steps {
        sh "docker push $IMAGE:${BUILD_NUMBER}"
      }
    }

    stage("Deploy Container") {
      steps {
        sh "docker rm -f myapp || true"
        sh "docker run -d --name myapp -p 8080:80 $IMAGE:${BUILD_NUMBER}"
      }
    }
  }
}
