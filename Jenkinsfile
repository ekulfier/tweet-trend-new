pipeline {
    agent {
        node {
            label 'maven'
        }
    }
environment {
	PATH = "/opt/apache-maven-3.9.4/bin:$PATH"
}

    stages {
        stage("build"){
			steps {
				echo "--------- build started -----------"
				sh 'mvn clean deploy -Dmaven.test.skip=true'
				echo "--------- build complete -----------"
			}
		}

		stage("test") {
			steps {
				echo "--------- unit test strated -----------"
				sh 'mvn surefire-report:report'
				echo "--------- unit test complete -----------"
			}
		}

		stage('SonarQube analysis') {
			environment {
				scannerHome = tool 'coding-sonar-scanner'
			}
			steps {
				withSonarQubeEnv('coding-sonarqube-server') { // If you have configured more than one global server connection, you can specify its name
					sh "${scannerHome}/bin/sonar-scanner"
				}
			}
		}
    }
}
