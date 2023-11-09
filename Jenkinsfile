// pipeline {

//   environment {
//     dockerimagename = "bravinwasike/react-app"
//     dockerImage = ""
//   }

//   agent any

//   stages {

//     // stage('Checkout Source') {
//     //   steps {
//     //     git 'https://github.com/laplace-ro/test-deployment'
//     //   }
//     // }

//     stage('Build image') {
//       steps{
//         script {
//           dockerImage = docker.build dockerimagename
//         }
//       }
//     }

//     stage('Pushing Image') {
//       environment {
//                registryCredential = 'dockerhub-credentials'
//            }
//       steps{
//         script {
//           docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
//             dockerImage.push("latest")
//           }
//         }
//       }
//     }

//     stage('Deploying React.js container to Kubernetes') {
//       steps {
//         script {
//           kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
//         }
//       }
//     }

//   }

// }
pipeline {
  environment {
    dockerimagename = "bravinwasike/react-app"
    dockerImage = ""
  }

  agent any

  stages {
    // stage('Checkout Source') {
    //   steps {
    //     git 'https://github.com/laplace-ro/test-deployment'
    //   }
    // }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhub-credentials'
        dockerHubUser = credentials('dockerHub').username
        dockerHubPassword = credentials('dockerHub').password
      }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
      post {
        success {
          stage('Docker Push') {
            agent any
            steps {
              withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh 'docker push bravinwasike/react-app:latest'
              }
            }
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }
  }
}

