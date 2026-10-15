
pipeline {
    agent any

    environment {
        CF_PROJECT = 'devops-test1-hothithang'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub'
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
                withCredentials([
                    string(credentialsId: 'cloudflare-api-token',
                           variable: 'CLOUDFLARE_API_TOKEN'),
                    string(credentialsId: 'cloudflare-account-id',
                           variable: 'CLOUDFLARE_ACCOUNT_ID')
                ]) {
                    sh '''
                        npx --yes wrangler@latest pages deploy . \
                          --project-name="$CF_PROJECT" \
                          --branch=main
                    '''
                }
            }
        }
    }

    post {
        success {
            withCredentials([
                string(credentialsId: 'telegram-bot-token',
                       variable: 'TELEGRAM_BOT_TOKEN'),
                string(credentialsId: 'telegram-chat-id',
                       variable: 'TELEGRAM_CHAT_ID')
            ]) {
                sh '''
                    curl -fsS -X POST \
                      "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
                      --data-urlencode "chat_id=${TELEGRAM_CHAT_ID}" \
                      --data-urlencode "text=CI/CD SUCCESS: Website deployed to https://devops-test1-hothithang.pages.dev"
                '''
            }
        }

        failure {
            withCredentials([
                string(credentialsId: 'telegram-bot-token',
                       variable: 'TELEGRAM_BOT_TOKEN'),
                string(credentialsId: 'telegram-chat-id',
                       variable: 'TELEGRAM_CHAT_ID')
            ]) {
                sh '''
                    curl -fsS -X POST \
                      "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
                      --data-urlencode "chat_id=${TELEGRAM_CHAT_ID}" \
                      --data-urlencode "text=CI/CD FAILED: Check Jenkins console output."
                '''
            }
        }
    }
}
