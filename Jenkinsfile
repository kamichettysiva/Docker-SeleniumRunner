pipeline { 
    agent any 
    stages {
        stage("Run Test") { 
            steps {
                sh "/usr/local/bin/docker-compose up"
            }
        }
		stage("Bring Grid Down"){
			steps{
				sh "/usr/local/bin/docker-compose down"
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
