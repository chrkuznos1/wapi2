pipeline {
    agent { dockerfile true }

    stages {
        stage('Build and Test') {
            steps {
                script {
                    try {
                        // Log the current directory and list contents for debugging
                        sh 'pwd'
                        sh 'ls -la'
                        sh 'apt update && apt install curl telnetd iproute2  -y'
                        sh 'ip a'
                        sh 'curl -X GET http://172.17.0.2:8080/WeatherForecast -H accept: text/plain'
                        // Run your actual command
                        sh ''' sleep 29 '''
                    } catch (Exception e) {
                        echo "Error occurred: ${e.getMessage()}"
                        error "Build failed"
                    }
                }
            }
        }
    }
}



// pipeline {
//   agent { dockerfile true }
//   // environment {}
//   // tools {
//   //   .NET SDK 'net_sdk_8'
//   // }
//   stages {
//     stage( 'Build and Test' ) {
//       steps {
//         sh '''
//           sleep 60
//         '''
//       }
//     }
//   }
// }

// https://www.youtube.com/watch?v=ymI02j-hqpU

// pipeline {
//   agent any
//   // /*
//   //  * Run everything on an existing agent configured with a label 'docker'.
//   //  * This agent will need docker, git and a jdk installed at a minimum.
//   //  */
//   // agent {
//   //   node {
//   //     label 'docker'
//   //   }
//   // }

//   // // using the Timestamper plugin we can add timestamps to the console log
//   // options {
//   //   timestamps()
//   // }

//   environment {
//     //Use Pipeline Utility Steps plugin to read information from pom.xml into env variables
//     IMAGE='weather'
//     //readMavenPom().getArtifactId()
//     VERSION='1.0.1'
//     //readMavenPom().getVersion()
//   }

//   // stages {
//   //   stage('Build') {
//   //     agent {
//   //       docker {
//   //         /*
//   //          * Reuse the workspace on the agent defined at top-level of Pipeline but run inside a container.
//   //          * In this case we are running a container with maven so we don't have to install specific versions
//   //          * of maven directly on the agent
//   //          */
//   //         reuseNode true
//   //         image 'maven:3.5.0-jdk-8'
//   //       }
//   //     }
//   //     steps {
//   //       // using the Pipeline Maven plugin we can set maven configuration settings, publish test results, and annotate the Jenkins console
//   //       withMaven(options: [findbugsPublisher(), junitPublisher(ignoreAttachments: false)]) {
//   //         sh 'mvn clean findbugs:findbugs package'
//   //       }
//   //     }
//   //     post {
//   //       success {
//   //         // we only worry about archiving the jar file if the build steps are successful
//   //         archiveArtifacts(artifacts: '**/target/*.jar', allowEmptyArchive: true)
//   //       }
//   //     }
//   //   }

//     // stage('Quality Analysis') {
//     //   parallel {
//     //     // run Sonar Scan and Integration tests in parallel. This syntax requires Declarative Pipeline 1.2 or higher
//     //     stage ('Integration Test') {
//     //       agent any  //run this stage on any available agent
//     //       steps {
//     //         echo 'Run integration tests here...'
//     //       }
//     //     }
//     //     stage('Sonar Scan') {
//     //       agent {
//     //         docker {
//     //           // we can use the same image and workspace as we did previously
//     //           reuseNode true
//     //           image 'maven:3.5.0-jdk-8'
//     //         }
//     //       }
//     //       environment {
//     //         //use 'sonar' credentials scoped only to this stage
//     //         SONAR = credentials('sonar')
//     //       }
//     //       steps {
//     //         sh 'mvn sonar:sonar -Dsonar.login=$SONAR_PSW'
//     //       }
//     //     }
//     //   }
//     // }

// stages {
//     stage('echo parameters') {
//   steps {
//     sh """ 
//     pwd
//     echo "${workspace}" 
//     echo $USER
//     """
//     //# cleanWs()
//   }
// }


//     stage('Build and Publish Image') {
//       // when {
//       //   branch 'master'  //only run these steps on the master branch
//       // }
//       steps {
//         /*
//          * Multiline strings can be used for larger scripts. It is also possible to put scripts in your shared library
//          * and load them with 'libaryResource'
//          */
//         sh """
//           pwd
//           ls -la 
//           docker build -t ${IMAGE} .
//           docker tag ${IMAGE} christakisg4/${IMAGE}:${VERSION}
//           docker login -u christakisg4 -p Sydney.1984
//           docker push christakisg4/${IMAGE}:${VERSION}
//         """
//       }
//     }
// }


//   post {
//     failure {
//       // notify users when the Pipeline fails
//       mail to: 'kuznos@logismos.gr',
//           subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
//           body: "Something is wrong with ${env.BUILD_URL}"
//     }
//   }
// }