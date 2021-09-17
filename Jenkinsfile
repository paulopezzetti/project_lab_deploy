pipeline {
  environment {
    registry = "paulopezzetti/trade:0.1"   
    registryCredential = "dockerhub"
    container_name = "trade"
    dockerImage = ''
  }
  agent any
  stages {
    stage('Efetuando Clone do repositório') {
      steps {
        git 'https://github.com/paulopezzetti/trade.git'
      }
    }
    
    stage('Buildando imagem') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    
    stage ('parando container atual'){
      steps {
         sh '[ -z $(docker ps -q --filter "name=$container_name") ] || docker stop $container_name && docker container prune -f'
         /*sh 'docker container prune -f'*/
      }
   }
    
   stage ('Iniciando novo container'){
      steps {
          sh 'docker run -d -p 3000:3000 --name $container_name $registry:$BUILD_NUMBER'
      }
   }
  
   stage('Efetuando deploy da imagem') {
   steps{
    script {
      docker.withRegistry( '', registryCredential ) {
        dockerImage.push()
         }
       }
      }
    }
    
    stage('Removendo imagens sem uso') {
      steps{
        //sh 'docker images rmi $registry:$BUILD_NUMBER'
        sh 'docker image prune -f -a'
      }
    } 
  }
}


