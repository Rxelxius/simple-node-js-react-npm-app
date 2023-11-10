pipeline {
	 agent {
        docker {
            image 'node:20.9.0-alpine3.18' 
            args '-p 5000:3000' 
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
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
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
	
		// stage('OWASP Dependency-Check Vulnerabilities') {
		// 	steps {
		// 		dependencyCheck additionalArguments: ''' 
		// 					-o './'
		// 					-s './'
		// 					-f 'ALL' 
		// 					--prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
				
		// 		dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		// 	}
		// }
	}	
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}