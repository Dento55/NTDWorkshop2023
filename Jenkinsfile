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
                bat 'docker run -p 3000:3000 --network bridge -d margus/movie-api'
            }
        }
		
		stage('Build Test container') {
            steps {
                bat 'docker build tests -t  margus/movie-api-tests'
            }
        }
		
		stage('Execute tests') {
            steps {
			   // bat 'set DOCKER_BUILDKIT=0'
                bat 'docker run -e PORT=3000 -e BASE_URI=172.17.0.0 --network bridge -v test-reports:/usr/src/app --rm  margus/movie-api-tests'
				bat 'docker container create --name report-container -v test-reports:/root alpine'
				bat 'docker cp report-container:/root/reports/. ./reports'
            }
        }
		
    }
	post {
        always {
            
			bat 'docker kill margus/movie-api'
			
		}
	}
}