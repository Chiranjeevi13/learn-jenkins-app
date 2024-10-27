pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    echo "node :" $(node --version)
                    echo "npm :" $(npm --version)
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    echo "Test Stage"
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('E2E'){
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:1.39.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    echo "E2E Stage"
                    npm install -g serve
                    serve -s build
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
