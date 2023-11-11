pipeline {
	 agent {
        docker {
            // image 'node:20.9.0-alpine3.18' 
			// Note: apparently using old version doesnt let the page display...
			image 'node:21.1.0-alpine3.18' 
            args '-p 3000:3000' 
        }
    }
	stages {
		stage('Checkout SCM') {
			steps {
				git 'https://github.com/Rxelxius/simple-node-js-react-npm-app.git'
			}
		}
		stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'OWASP Dependency-Check Validation'
				dependencyCheckPublisher pattern: 'dependency-check-report.xml'
			}
		}
        stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
	
	}
}