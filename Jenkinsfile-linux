pipeline {
    agent any

    triggers {
        cron('H 0 * * *') // se ejecuta todos los días a medianoche (UTC)
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gugoansa/golog.git'
            }
        }

        stage('Setup Node.js') {
            steps {
                // instala Node.js 18
                sh 'curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -'
                sh 'sudo apt-get install -y nodejs'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
                sh 'npx playwright install --with-deps'
            }
        }

        stage('Run Playwright Tests') {
            steps {
                sh 'npx playwright test'
            }
        }

        stage('Archive Report') {
            steps {
                archiveArtifacts artifacts: 'playwright-report/**', fingerprint: true
            }
        }
    }

    post {
        always {
            junit 'playwright-report/results.xml'
        }
    }
}
