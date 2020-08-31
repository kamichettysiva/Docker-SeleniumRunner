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
		stage('Report Extraction') {
            steps {
                sh '/usr/local/bin/docker-compose up volumes'
            }
        }
        stage('Exit Hub') {
            steps {
                sh '/usr/local/bin/docker-compose down'
            }
        }
		stage('Generate HTML report') {
			steps {
				cucumber buildStatus: 'STABLE',
                reportTitle: 'My report',
                fileIncludePattern: '**/*.json',
                trendsLimit: 10,
                classifications: [
                    [
                        'key': 'Browser',
                        'value': 'Firefox'
                    ]
                ]
			}
		}
    }
}

