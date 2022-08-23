pipeline {
    agent any
        environment {
        registry = "hackathonteam9/python-app"
        registryCredential = 'Dockerhub_Credentials'
        dockerImage = ''
    }
    stages {

        stage ('checkout') {
            steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[ credentialsId: 'Github_Credentials', url: 'https://github.com/arghbhat/HackathonTest.git']]])
            }
        }
       
        stage ('Build docker image') {
            steps {
                script {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
       
         // Uploading Docker images into Docker Hub
    stage('Upload Image') {
     steps{   
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            }
        }
      }
    }



      stage ('K8S Deploy') {
        steps {
            script {
                sh 'sed -i "s/tag/${BUILD_NUMBER}/" Kubernetes-deploy.yaml'
                kubernetesDeploy(
                    configs: 'Kubernetes-deploy.yaml',
                    kubeconfigId: 'Kube-config-file',
                    enableConfigSubstitution: true
                    )           
               
            }
        }
    }
   
    
  
    }  
}
