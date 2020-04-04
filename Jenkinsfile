def mvn
pipeline {
  agent { label 'master' }
    tools {
      maven 'Maven'
      jdk 'JAVA_HOME'
    }
  stages {
    stage('Git Checkout') {
      steps {
        git credentialsId: 'git_creds_dileep', url: 'https://github.com/Dileeprithvi/yankils-hello-world.git'
      }
    }
    stage ('Maven Build') {
      steps {
        script {
          mvn= tool (name: 'Maven', type: 'maven') + '/bin/mvn'
        }
        sh "${mvn} clean install"
      }
    }
  stage('Build Docker Image'){
    steps{
      sh 'docker build -t dileep95/yankils-hello:$BUILD_NUMBER .'
    }
  }
  stage('Docker Container'){
    steps{
      withCredentials([usernameColonPassword(credentialsId: 'docker_dileep_creds', variable: 'DOCKER_PASS')]) {
      sh 'docker push dileep95/yankils-hello:$BUILD_NUMBER'
	  sh 'docker run -d -p 8070:8070 --name yankils dileep95/yankils-hello:$BUILD_NUMBER'
	   sh 'docker exec -it yankils /bin/bash'
	     sh 'exit'
	      sh 'docker restart yankils'
    }
    }
  }  

  }
}
