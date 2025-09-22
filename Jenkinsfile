pipeline {
  agent any
    environment {
    DOCKERHUB_CRED = 'dockerhub-creds'
    IMAGE_NAME = 'your-dockerhub-username/static-site' // <-- replace username
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build Image') {
      steps {
        script {
          sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
        }
      }
    }
    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CRED}",
                                          usernameVariable: 'DOCKER_USER',
                                          passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push ${IMAGE_NAME}:${BUILD_NUMBER}
            docker logout
          '''
        }
      }
    }
  }
}

