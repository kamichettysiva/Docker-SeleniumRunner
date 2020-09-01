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
                sh '/usr/local/bin/docker-compose up search-module'
            }
        }	
    }
	post {
		always{
			archiveArtifacts artifacts:'report/**'
			 sh '/usr/local/bin/docker-compose down'
			cucumber buildStatus: 'UNSTABLE',
                	reportTitle: 'My report',
                	fileIncludePattern: '**/*.json',
                	trendsLimit: 10,
                	classifications: [
                    		[
                        	'key': 'Browser',
                        	'value': 'chrome'
                    		]
               		]
	}
}

