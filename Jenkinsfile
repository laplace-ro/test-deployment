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
    registryCredential = 'dockerhub-credentials'
  }

  agent any

  stages {
    // ... (restul codului rămâne neschimbat)

    stage('Pushing Image') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: registryCredential, passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
            docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
              dockerImage.push("latest")
            }
          }
        }
      }
    }

    // ... (restul codului rămâne neschimbat)
  }

  post {
    success {
      script {
        if (currentBuild.resultIsBetterOrEqualTo("SUCCESS")) {
          echo 'Running Docker Push'
          withCredentials([usernamePassword(credentialsId: registryCredential, passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
            sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"
            sh 'docker push bravinwasike/react-app:latest'
          }
        }
      }
    }
  }
}
