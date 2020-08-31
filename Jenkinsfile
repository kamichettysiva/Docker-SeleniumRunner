pipeline {
    stages {
        stage('Create Hub') {
            steps {
                sh 'docker-compose up -d selenium-hub chrome firefox'
            }
        }
	stage('Run Test') {
            steps {
                sh 'docker-compose up search-module'
            }
        }
        stage('Exit Hub') {
            steps {
                sh 'docker-compose down'
            }
        }
        
    }
}
