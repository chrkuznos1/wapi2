pipeline {
    agent any
    environment {
        //DOCKER_IMAGE_NAME = "ptran32/nodejs-express-app"
	   def registry = "christakisg4/mydemocontainer"
       def registryCredential = 'dockerhub_creds'
	   def customImage = ''
    } 
    options {
    skipDefaultCheckout(true)
    }

    stages {
node {
//        stage('Git Clone Source') {
//            steps {
//                git url: 'https://github.com/ptran32/nodejs-express-app.git'
//            }
//        }

        stage('Test and Build Docker Image') {
            when {
                branch 'master'
                }
            steps {
                script {
                    env.GIT_COMMIT_REV = sh (script: 'git log -n 1 --pretty=format:"%h"', returnStdout: true)
                    customImage = docker.build("${registry}:${GIT_COMMIT_REV}-${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_creds') {
                        customImage.push("${GIT_COMMIT_REV}-${env.BUILD_NUMBER}")
                        customImage.push("latest")
                    }
                }
            }
        }
		stage('Remove Unused docker image') {
			steps{
				sh "docker rmi ${registry}:${GIT_COMMIT_REV}-${env.BUILD_NUMBER}"
			}
		}	
		stage('Clean Workspace') {
            steps {
            deleteDir()
            }
        }
}    
	}
}