pipeline {
	agent any
	parameteors {
		string(name: 'tomcat_dev', defaultValue: '192.168.2.253:8090', description: 'Staging server')
                string(name: 'tomcat_prod', defaultValue: '192.168.2.253:9090', description: 'Production server')
	}	

	triggers {
		pollSCM('* * * * *')
	}

	
	tools {
		maven 'localMaven'
	}

	stages{
		stage('Build'){
			steps {
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/*.war'
				}
			}
		}

		stage ('Deplyment') {
			parallel{
				stage('Deploy to Staging'){
					steps {
						build job: 'deploy-to-staging'
					}
				}

		                stage('Deploy to Production'){
					steps {
						build job: 'deploy-to-prod'
					}
                		        post {
						success {	
							echo 'Code deploied to Production'
						}
						failure {
							echo 'Prod deployment failed!'
						}	
                		        }
               		 }
		}
	}
}

