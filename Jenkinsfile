pipeline{
    agent any
    environment {
    DOCKER_TAG = "getVersion()"
   }
    stages{ 
        stage('SCM'){
        steps{
            git branch: 'main', credentialsId: 'github', url: 'https://github.com/dik12345/dockeransiblejenkins.git'
        }    
    }

    stage('MAVEN BUILD'){
        steps{
            sh "mvn clean package"

        }    
     
    }
    stage('DOCKER BUILD'){
        steps{
            sh "docker build  . -t shine1234/webapp:${DOCKER_TAG}"

        }    
     
    }
    stage('DOCKER PUSH'){
       steps{
           withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
    // some block
    }
           sh "docker login -u shine1234 -p ${dockerhubpwd}"
       }
           sh "docker push shine1234/webapp:${DOCKER_TAG}"

       }    
      stage('DOCKER DEPLOY'){
       steps{
    ansiblePlaybook credentialsId: 'ansible', disableHostKeyChecking: true, extras: 'DOCKER_TAG', installation: 'ansible', inventory: 'dev.inv', playbook: 'docker-deploy.yml'

        }    
     }

   def get_version(){
   def commitHash = sh returnStdout: true, script: '''git rev-parse --short HEAD'''
   return commitHash
   }
}
}
