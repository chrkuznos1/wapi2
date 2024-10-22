pipeline {
    agent any
    environment {
        //DOCKER_IMAGE_NAME = "ptran32/nodejs-express-app"
	   def registry = "christakisg4/mydemocontainer"
       def registryCredential = 'dockerhub_creds'
	   def customImage = ''
       def GIT_COMMIT_REV = "1.0"
    } 
    // options {
    // skipDefaultCheckout(true)
    // }

//node {
    stages {

//        stage('Git Clone Source') {
//            steps {
//                git url: 'https://github.com/ptran32/nodejs-express-app.git'
//            }
//        }

        stage('Test and Build Docker Image') {
            // when {
            //     branch 'master'
            //     }
            steps {
                script {
                    //GIT_COMMIT_REV = sh (script: 'git log -n 1 --pretty=format:"%h"', returnStdout: true)
                    customImage = docker.build("${registry}:${GIT_COMMIT_REV}-${env.BUILD_NUMBER}")

                    customImage.inside {
                        sh 'pwd && ls -la'
                                        }
                }
            }
        }
        stage('Push Docker Image') {
            // when {
            //     branch 'master'
            // }
            steps {
                script {
                    docker.withRegistry('', 'dockerhub_creds') {
                        customImage.push("${GIT_COMMIT_REV}-${env.BUILD_NUMBER}")
                        customImage.push("latest")
                    }
                }
            }
        }
		stage('Remove Unused docker image') {
			steps{
				//sh "docker rmi ${registry}:${GIT_COMMIT_REV}-${env.BUILD_NUMBER}"
                sh "docker rmi $(docker images ${registry})"
                sh "docker rmi $(docker images -f "dangling=true")"
			}
		}

}       
    post {
		cleanup {
            deleteDir()
               dir("${workspace}@tmp") {
            deleteDir()
                                        }
                }   
          }
	//}
}