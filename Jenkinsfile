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
          dockerimage = docker.build DOCKERHUB_REPO + ":V$BUILD_NUMBER"
        }
       }
    }

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('', 'DOCKERHUB_CREDENTIALS') {
          dockerimage.push("V$BUILD_NUMBER")
          dockerimage.push('latest')
          }
}
}
}

    stage('Deploy docker Container') {
      steps{
        script {
          sh "docker pull ${env.DOCKERHUB_REPO}:${env.BUILD_ID}"
          sh "docker run -d --name myapp -p 80:80 ${env.DOCKERHUB_REPO}:${env.BUILD_ID}"

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

