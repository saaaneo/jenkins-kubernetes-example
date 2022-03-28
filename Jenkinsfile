pipeline{
    agent any
	options {
	skipDefaultCheckout(true)
    }
    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/saaaneo/jenkins-kubernetes-example.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t aalhad/nodejsapp-1.0:latest .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhub_id', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u aalhad -p ${dockerhubpwd}'
                 }  
                 sh 'docker push aalhad/nodejsapp-1.0:latest'
                }
            }
        }
    
    stage('Deploy App on k8s') {
      steps {
            sshagent(['k8s']) {
            sh "scp -o StrictHostKeyChecking=no nodejsapp.yaml ubuntu@172.31.94.113:/home/ubuntu"
            script {
                try{
                    sh "ssh ubuntu@172.31.94.113 kubectl create -f ."
                }catch(error){
                    sh "ssh ubuntu@172.31.94.113 kubectl create -f ."
					}
				}
			}
      
		}
	}
  }
}
