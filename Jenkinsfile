pipeline {
    agent any

    // 🕒 Programación automática: cada día a las 3:00 AM
    triggers {
        cron('0 3 * * *')
    }

    environment {
        CI = 'true' // asegura que Playwright corra en modo headless
    }

    stages {

        stage('Checkout') {
            steps {
                // Clona la rama main explícitamente
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
                bat 'npm ci'
            }
        }

        /*stage('Run Playwright Tests') {
            steps {
                // Genera reportes HTML + JUnit XML
                bat 'npx playwright test --reporter="html,junit"'
            }
        }*/
        stage('Run Playwright Tests') {
            steps {
             // Usa la config predeterminada, que ya tiene HTML + JUnit
              bat 'npx playwright test'
             }
        }


        stage('Archive Reports') {
            steps {
                // Ajusta la ruta de los reportes
                archiveArtifacts artifacts: 'playwright-report/**', fingerprint: true
                junit 'playwright-report/results.xml'
            }
        }
    }

    post {
        always {
            echo "✅ Pipeline finished."
        }
        failure {
            echo "❌ Build failed. Revisa los logs o el reporte HTML."
        }
    }
}
