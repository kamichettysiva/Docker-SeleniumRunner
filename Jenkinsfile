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
        
    }
}
