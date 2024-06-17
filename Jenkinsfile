pipeline {
   agent any

   environment {
     DOCKERHUB_CREDENTIALS = credentials('dockerhub')
     DOCKERHUB_REPO = 'smritidevopslearning/project2'
     DOCKER_CONTAINER_NAME = 'myapp'
   }

   stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/Smritidevopslearning/Project2.git' , branch: 'main'
      }
    }
    
    stage('Build Docker Image') {
      steps {
        script {
          image = docker.build DOCKERHUB_REPO + ":V$BUILD_NUMBER"
        }
       }
    }
    stage('Login to dockerhub') {
       steps {
          script {
             sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
          }
       }
    }
    stage('Push Docker Image') {
      steps {
         script {
           image.push()
         }
           }
        }

    stage('Deploy docker Container') {
      steps{
        script {
          sh "docker stop ${DOCKER_CONTAINER_NAME} || true"
          sh "docker rm ${DOCKER_CONTAINER_NAME} || true"
          sh "docker pull ${env.DOCKERHUB_REPO}:V${BUILD_NUMBER}"
          sh "docker run -p 80:80 --name $(DOCKER_CONTAINER_NAME) -d ${env.DOCKERHUB_REPO}:V${BUILD_NUMBER}"

}
}
}

    stage('Approval') {
            steps {
                // Manual approval stage
                input message: 'Approve deployment?', ok: 'Deploy'
            }
        }

}
 post {
   success {
     echo 'Pipeline executed successfully!'
   }

   failure {
      echo 'Pipeline execution failed!'
        }
}

} 

