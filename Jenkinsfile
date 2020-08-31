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
                failedFeaturesNumber: 1,
                failedScenariosNumber: 1,
                skippedStepsNumber: 1,
                failedStepsNumber: 1,
                classifications: [
                        [key: 'Commit', value: '<a href="${GERRIT_CHANGE_URL}">${GERRIT_PATCHSET_REVISION}</a>'],
                        [key: 'Submitter', value: '${GERRIT_PATCHSET_UPLOADER_NAME}']
                ],
                reportTitle: 'My report',
                fileIncludePattern: '/var/lib/jenkins/workspace/pragra-selenium/report/cucumber-reports/**/*.json',
                sortingMethod: 'ALPHABETICAL',
                trendsLimit: 100
		}
	}
}

