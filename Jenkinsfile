pipeline {
   agent any

   environment {
     DOCKERHUB_CREDENTIALS = credentials('dockerhub')
     DOCKERHUB_REPO = 'smritidevopslearning/project2'
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

     stage('Login to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub
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
          sh "docker pull ${env.DOCKERHUB_REPO}:${env.BUILD_ID}"
          sh "docker run -p 80:80 --name=myapp -d ${env.DOCKERHUB_REPO}:${env.BUILD_ID}"

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

