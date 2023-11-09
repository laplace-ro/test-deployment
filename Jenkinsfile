pipeline {

  environment {
    dockerimagename = "bravinwasike/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Docker Push') {
      steps {
      	withCredentials([usernamePassword(credentialsId: 'jenkins', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
         
            sh 'docker tag ${dockerimagename} jenkins742/jenkins-deploy'
            sh 'docker push jenkins742/jenkins-deploy'
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        withKubeConfig([credentialsId: 'kubedev', serverUrl: 'http://localhost:8443']) {
            sh 'ls -al'
            sh 'pwd'
            sh "microk8s kubectl apply -f deployment.yaml"
            sh "microk8s kubectl apply -f service.yaml"
            sh "microk8s kubectl apply -f ingress.yaml"
        }
      }
    }

  }

}

