pipeline {
    // Playwright with Node, see https://playwright.dev/docs/ci#jenkins
    agent {
        docker {
            // See https://mcr.microsoft.com/en-us/artifact/mar/playwright
            image 'mcr.microsoft.com/playwright:v1.54.0-noble'
            args '-u root:root'
        }
    }

    // Playwright with Node, see https://playwright.dev/docs/ci
    stages {
        stage('Install Dependencies') {
            steps {
                // Install NPM packages
                sh 'npm ci'
                // Install Playwright browsers and dependencies
                sh 'npx playwright install --with-deps'
            }
        }

        // Playwright with Node, see https://playwright.dev/docs/ci
        stage('Run Tests') {
            steps {
                sh 'npx playwright test'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
            publishHTML([
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'playwright-report',
                reportFiles: 'index.html',
                reportName: 'Playwright HTML Report'
            ])
            junit 'results.xml'
        }
        failure {
            echo 'Tests failed! Check the report for details.'
        }
    }
}
