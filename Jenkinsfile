pipeline { 
    agent any 
    stages {
        stage("Run Test") { 
            steps {
                sh "docker-compose up"
            }
        }
		stage("Bring Grid Down"){
			steps{
				sh "docker-compose up"
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
