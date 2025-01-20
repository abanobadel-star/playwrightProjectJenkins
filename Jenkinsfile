pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/abanobadel-star/playwrightProjectJenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'node -v'
                        sh 'npm install'
                    } else {
                        bat 'node -v'
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Install Playwright Browsers') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npx playwright install'
                    } else {
                        bat 'npx playwright install'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npx playwright test --project=chromium'
                    } else {
                        bat 'npx playwright test --project=chromium'
                    }
                }
            }
        }

        stage('Publish Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'playwright-report',
                    reportFiles: 'index.html',
                    reportName: 'Playwright Test Report'
                ])
            }
        }
    }

    post {
        always {
            stage('Open Report') {
                steps {
                    script {
                        def reportPath = "file:///C:/ProgramData/Jenkins/.jenkins/jobs/Playwright_Pipeline/builds/${env.BUILD_NUMBER}/htmlreports/Playwright_20Test_20Report/index.html"
                        echo "Opening report: ${reportPath}"
                        bat "start ${reportPath}"
                    }
                }
            }
        }
    }
}
