//Windows Version
pipeline {
    agent any

    triggers {
        // 🕐 Ejecutar todos los días a las 9 AM
        cron('H 9 * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gugoansa/golog.git'
            }
        }

        stage('Setup Node.js') {
            steps {
                bat '''
                if not exist "C:\\Program Files\\nodejs\\node.exe" (
                    echo Node.js no encontrado. Descargalo e instalalo manualmente desde https://nodejs.org/en/download/windows
                    exit /b 1
                ) else (
                    node -v
                    npm -v
                )
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                cd playwright
                npm install
                '''
            }
        }

        stage('Run Playwright Tests') {
            steps {
                bat '''
                cd playwright
                npx playwright test --reporter=list
                '''
            }
        }

        stage('Archive Report') {
            steps {
                junit 'playwright/test-results/*.xml'
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline finished.'
        }
    }
}
