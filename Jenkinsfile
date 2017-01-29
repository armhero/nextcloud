#!groovy

node('armhf') {
  stage('Checkout') {
    checkout scm
  }

  stage('Build') {
    sh 'sudo docker build -t armhero/nextcloud:\044{BRANCH_NAME} .'
  }

  stage('Push') {
    withCredentials([
      usernamePassword(credentialsId: '1d448f61-46d6-4af8-a517-9a06866447bb',
      passwordVariable: 'DOCKER_PASSWORD',
      usernameVariable: 'DOCKER_USERNAME')
    ]) {
      sh '''#!/bin/bash -xe
        sudo docker login -u \044{DOCKER_USERNAME} -p \044{DOCKER_PASSWORD}

        if [[ "\044{BRANCH_NAME}" == "master" ]]; then
          # when we are in the master branch, then set a new tag
          sudo docker tag armhero/nextcloud:\044{BRANCH_NAME} armhero/jenkins-slave:latest

          sudo docker push armhero/nextcloud:latest
        else
          sudo docker push armhero/nextcloud:${BRANCH_NAME}
        fi

      '''
    }
  }
}