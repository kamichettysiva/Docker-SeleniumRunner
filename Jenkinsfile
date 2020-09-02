pipeline {
    agent any
    stages {
        stage('Create Hub') {
            steps {
                sh '/usr/local/bin/docker-compose up -d selenium-hub chrome firefox'
            }
        }
		stage('Run Test') {
            steps {
		sh 'docker image rm kamichettysiva/dockertest:latest'    
                sh '/usr/local/bin/docker-compose up search-module'
            }
        }	
    }
	post {
		always{
			archiveArtifacts artifacts:'reports/**'
			 sh '/usr/local/bin/docker-compose down'
			cucumber buildStatus: 'UNSTABLE',
                	reportTitle: 'Cucumber Report',
                	fileIncludePattern: '**/cucumber-html-report.json',
                	trendsLimit: 10,
                	classifications: [
                    		[
                        	'key': 'Browser',
                        	'value': 'chrome'
                    		]
               		]
		}
	}

}

