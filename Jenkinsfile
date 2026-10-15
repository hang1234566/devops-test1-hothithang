pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is checked out from GitHub'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Static HTML website: no dependencies required'
            }
        }

        stage('Build') {
            steps {
                bat 'if exist index.html (echo Build Success) else (exit /b 1)'
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
```