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

    stage('Push Docker Image') {
      steps {
        script {
           docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
           image.push("V$BUILD_NUMBER")
           image.push('latest')
           }
        }
}
}

    stage('Deploy docker Container') {
      steps{
        script {
          sh "docker pull ${env.DOCKERHUB_REPO}:V${BUILD_NUMBER}"
          sh "docker run -p 80:80 --name=myapp -d ${env.DOCKERHUB_REPO}:V${BUILD_NUMBER}"

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

