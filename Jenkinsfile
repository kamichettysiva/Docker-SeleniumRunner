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
        stage('Exit Hub') {
            steps {
                sh '/usr/local/bin/docker-compose down'
            }
        }
	stage('cucumber reports'){
		steps {
			sh 'cucumber failedFeaturesNumber: -1, failedScenariosNumber: -1, failedStepsNumber: -1, fileIncludePattern: '**/*.json', pendingStepsNumber: -1, skippedStepsNumber: -1, sortingMethod: 'ALPHABETICAL', undefinedStepsNumber: -1'
	}
        
    }
}
