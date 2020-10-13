pipeline {

  environment {
    IMAGE_TAG = "${GIT_COMMIT[0..6]}"
  }

  agent any
  stages {

    stage('Checkout code') {
      steps {
        checkout scm
        sh 'mkdir code'
          dir('code') {
          git url: "https://github.com/gruntwork-io/sample-app-docker.git"
          }
        }
      }

    stage('test') {
      steps {
          dir('code') {
            nodejs(nodeJSInstallationName: 'nodejs') {
              sh 'npm install'
              sh 'npm run test'
            }
          }
      }
    }
    stage('docker build/push') {
      steps {
        script {
          dir('code') {
            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
            docker.build("omria/sample-app-docker:${IMAGE_TAG}", '.').push()
            }
          }
        }
      }
    }
    stage('deploy kubernetes') {
      steps {
        kubernetesDeploy(configs: "deploy.yml", enableConfigSubstitution: true, kubeconfigId: "kindconfig")
      }
    }
  }
}
