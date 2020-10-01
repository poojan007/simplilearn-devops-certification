pipeline {
  environment {
    registry = "poojansharma/dockerizing-jenkins-pipeline"
    registryCredential = 'poojandockerhub'
  }
  agent any
  stages {
	stage('Cloning Git') {
	steps {
		git 'https://github.com/poojan007/simplilearn-devops-certification.git'
	      }
	}

        stage('Building image') {
        steps{
            script {
            dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
        }
        }

        stage('Deploy Image') {
        steps{
            script {
            docker.withRegistry( '', 'dockerhub' ) {
                dockerImage.push()
            }
            }
        }
        }

        stage('Remove Image') {
        steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
        }
        }
   }   
}

node {
    stage('Execute Image'){
        def customImage = docker.build("poojansharma/dockerizing-jenkins-pipeline:${env.BUILD_NUMBER}")
        customImage.inside {
            sh 'echo This is the code executing inside the container.'
        }
    }
}
