@Library('docker-shared-lib')_

pipeline {
    agent any
	 stages {
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
