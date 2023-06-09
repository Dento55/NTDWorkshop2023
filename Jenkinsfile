pipeline {
    agent any
    stages {
        stage('My First Stage') {
            steps {
                bat 'docker build . -t margus/movie-api'
            }
        }
    }
}