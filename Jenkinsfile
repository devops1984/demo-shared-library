pipeline {
    agent any
	environment {
	    PATH = "$PATH:/usr/share/maven/bin"
	    imagename = "k2r2t2/demoproject2"
	    dockerImage = ""	
	}
	 stages {
                stage('Maven Build'){
		    steps{
			     sh " mvn clean package -Dv=${BUILD_NUMBER}"   
			}
		}
		stage('Build Image') {                               	
                    steps {
			    script {
				 dockerImage = docker.build imagename
			    }
                       }
                }
		stage('Push Image') {   
			environment {
				registryCredential = 'dockerhub'
			}
                    steps {
			    script {
				    docker.withRegistry( 'https://registry.hub.docker.com' , registryCredential ){
					    dockerImage.push("latest")
				    }
			    }   
                       }
                }
		stage("kubernetes deployment"){
		  steps {
                        withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8S', namespace: '', serverUrl: '']]) {
                                   sh "kubectl apply -f deployment.yml"
				   sh "kubectl rollout restart deployment.v1.apps/demoproject-deployment"
                            }
		  }
             }
       } 
 }
