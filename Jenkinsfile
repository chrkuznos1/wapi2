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

                    customImage.inside ("-u root:root --privileged -p 8000:8080") {
                        sh 'pwd && ls -la'
                        sh 'apt update && apt install curl -y'
                        sh 'curl -v http://localhost:8080/swagger'
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
                sh "docker rmi \$(docker images --format '{{.ID}}' --filter=dangling=true)"
                sh "docker rmi \$(docker images ${registry})"
                sh '''
                    if [[ $? -ne 0 ]] ; then exit 0 else echo "OK" fi
                '''

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