pipeline {
	agent { label 'slave_d1'}
	stages {
        stage('install newman') {
			steps{
				script{
					try {
					  sh """
					  sudo apt update
					  sudo apt install nodejs npm
					  npm install -g newman
					  npm install -g newman-reporter-htmlextra
					  npm install -g newman-reporter-junitfullreport
						  """
					}
					catch (exc) {
					  println "Failed to install newman"
					  throw(exc)
					}
				}
			}
        }
		stage('lance test') {
			steps{
				sh """
				newman run test-client.postman_collection.json -r cli,junitfullreport,htmlextra --reporter-junitfullreport-export 'result.xml' --reporter-htmlextra-export './results/htmlextra.html'
				"""
			}
		}
	}
}