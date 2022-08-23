@Library('docker-shared-lib')_

pipeline {
    agent any
	environment {
             HOME  = "${WORKSPACE}"
             }
	
	
	 stages {
		stage ('cleanWs & checkout scm') {
                  steps {
                     script {
                        deleteDir()
                        cleanWs()
                        checkout scm
                            }
                         }
                 }
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
			 dockerBuild.deploy()
                                      }
                               }
                           }
                }
      post { 
          always {
            script {
                cleanWs()
                 }
              }
         }

}
