pipeline {
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
	stage('Docker Build') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'docker_host', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cd /opt/docker; docker build -t aalhad/nodejsapp-1.0:latest .; docker push aalhad/nodejsapp-1.0:latest; docker rmi -f aalhad/nodejsapp-1.0:latest', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'Dockerfile, package.json, server.js')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    	stage('Deploy App on k8s') {
      		steps {
         	   sshPublisher(publishers: [sshPublisherDesc(configName: 'kube_master', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'nodejsapp.yaml')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        	    script {
          	      try{
          	          	sh "ssh aalhad@10.0.61.137 kubectl create -f ."
              	  	}catch(error){
                    		sh "ssh aalhad@10.0.61.137 kubectl create -f ."
            			}
			}
		     }
		}
    }
}
