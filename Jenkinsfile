pipeline { 
    agent any 
    stages {
        stage("Create Selenium Hub") { 
            	steps {
                	sh "docker-compose up -d selenium-hub chrome firefox"
		}
        }
	stage("Run Tests") { 
           	 steps {
                	sh "docker-compose up search-module"    
            }
        }
	stage("Bring Grid Down"){
		steps{
			sh "docker-compose down"
		}
	}
	}
	post {
        cleanup {
            /* clean up our workspace */
            deleteDir()
            /* clean up tmp directory */
            dir("${workspace}@tmp") {
                deleteDir()
            }
            /* clean up script directory */
            dir("${workspace}@script") {
                deleteDir()
            }
        }
	}
}
