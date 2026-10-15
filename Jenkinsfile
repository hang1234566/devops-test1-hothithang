pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Static HTML website: no dependencies required'
            }
        }

        stage('Build') {
            steps {
                sh 'test -f index.html'
                echo 'Build Success'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Build passed. Ready to deploy website.'
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESS'
        }
        failure {
            echo 'BUILD FAILED'
        }
    }
}
