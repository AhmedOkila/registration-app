pipeline {
  agent {
    label "Jenkins-Slave"
  }
  tools {
    maven 'Maven3'
  }
  environment {
    APP_NAME = "register-app"
    RELEASE = "1.0.0"
    DOCKER_USER = "ahmedokila"
    DOCKER_CREDENTIALS_ID = 'Dockerhub'
    IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
  }
  stages {
    stage("CleanUp Workspace") {
      steps {
        cleanWs()
      }
    }
    stage("Clone the Repo") {
      steps {
        git branch: "main",
            url: "https://github.com/AhmedOkila/registration-app.git",
            credentialsId: "Github"
      }
    }
    stage("Build the Application") {
      steps {
        sh "mvn clean package"
        sh "echo $BUILD_NUMBER"
      }
    }
    stage("Build and Push Docker Image") {
      steps {
        script {
          docker.withRegistry("", DOCKER_CREDENTIALS_ID) {
            docker_image = docker.build("${IMAGE_NAME}")
            docker_image.push("${IMAGE_TAG}")
          }
        }
      }
    }
  }
}
