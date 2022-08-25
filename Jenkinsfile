@Library('docker-shared-lib')_

pipeline {
    agent any
	/*environment {
             HOME  = "${WORKSPACE}"
             }*/
	
	
	 stages {
		/*stage ('cleanWs & checkout scm') {
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
		}*/
		stage('Container build') {
                   steps {
                      script {
			 build.mavenbuild()
                         build.login()
                         build.buildimage(env.DOCKER_TAG)
                         build.pushimage(env.DOCKER_TAG)
			 build.deploy()
                                      }
                               }
                           }
                }
}
