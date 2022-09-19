pipeline{  
    environment {
    registry = "sushantdocker/psassignment"
    }
  agent any
  stages {
           stage('Build') {
          
           steps {
                
                   sh 'pwd'
                   sh 'sudo docker build .'
                      }
       }
       
      
       stage('Publish') {
           environment {
               registryCredential = 'dockerhub'
           }
           steps{
              
              script {
                 
                 docker.withRegistry('', registryCredential){
                 def appimage = docker.build registry + ":$BUILD_NUBER"
                 appimage.push()
                      
                  }
              }
           }
       }
       stage ('Deploy') {
           steps {
               script{
                   def image_id = registry + ":$BUILD_NUMBER"
                   sh "sed -i 's|image_id|$image_id|g' deployment.yml"
                   sh "kubectl apply -f deployment.yml -f service.yml"
                   sh "kubectl rollout status deployment springboot"
                   sh "kubectl get service springboot-svc"
                }
           }
       }
   }
}
