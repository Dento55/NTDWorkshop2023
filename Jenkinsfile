pipeline {
    agent any
    stages {
        stage('Build API container') {
            steps {
                bat 'docker build . -t margus/movie-api'
            }
        }
		
		stage('Run API container') {
            steps {
                bat 'docker run -p 3000:3000 --network bridge --name api-container --rm -d margus/movie-api'
            }
        }
		
		stage('Build Test container') {
            steps {
                bat 'docker build tests -t  margus/movie-api-tests'
            }
        }
		
		stage('Execute tests') {
            steps {
			        bat 'docker volume create test-reports'
					bat 'docker run -e PORT=3000 -e BASE_URI=172.17.0.2 --network bridge -v test-reports:/usr/src/app --rm margus/movie-api-tests'
            }
        }
		stage('Reporting') {
            steps {
			  
				bat 'docker container create --name report-container -v test-reports:/root alpine'
				bat "docker cp report-container:/root/reports/. ${WORKSPACE}/reports"
            }
        }
		 stage('Publish Test Results') {
            steps {
                junit skipPublishingChecks: true, testResults: 'reports/jest-junit.xml'
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'test-report.html',
                    reportName: 'Test Results'
                ]
            }
        }
		
    }
	post {
        always {
            
			bat 'docker kill api-container'
			bat 'docker rm report-container'
			bat 'docker volume rm test-reports'
		}
	}
}