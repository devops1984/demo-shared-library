@Library('docker-shared-lib')_

pipeline {
    agent any
	environment {
             HOME  = "${WORKSPACE}"
             }
	 stages {
                stage('Maven Build'){
		    steps{
			     sh " mvn clean package -Dv=${BUILD_NUMBER}"   
			}
		}
		stage('Container build') {
                   when {
                      allOf {
                             expression { dockerfiles }
                             branch "main"
                     }
                }
                   steps {
                      script {
                         dockerBuild.login()
                         dockerBuild.build(env.DOCKER_TAG)
                         dockerBuild.push(env.DOCKER_TAG)
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
