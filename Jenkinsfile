pipeline {
	agent any
	tools {
		maven 'M3'
	}
	stages{
		stage('Build'){
			steps {
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/targaet/*.war'
				}
			}
		}
	}
}

