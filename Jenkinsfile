pipeline {
	agent { label 'slave_d1'}
	stages {
        stage('install newman') {
			steps{
				script{
					try {
					  sh """
					  sudo apt update
					  sudo apt -y install nodejs npm
					  sudo npm install -g newman
					  sudo npm install -g newman-reporter-htmlextra
					  sudo npm install -g newman-reporter-junitfullreport
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
		stage('publication des resultats au format html') {
		    steps{
				println "publication du rapport"
				publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'results', reportFiles: 'htmlextra.html', reportName: 'HTML Report', reportTitles: ''])
			}
		}
	}
}